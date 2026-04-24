# Android Notification Bridge - Minimal MVP Specification

## 1. Overview
The **Android Notification Bridge** is a lightweight, background-first Android application designed to intercept local system notifications from specific medical apps (e.g., Medtronic MiniMed), extract critical health data (Glucose values, IOB) via regex, and dispatch this data to a configured Webhook URL.

This document outlines the **absolute minimal** technical specification required to implement this app from scratch, removing all non-essential libraries and complex UI elements, while still ensuring reliable data delivery.

---

## 2. Simplifications for the Minimal MVP

To achieve the smallest possible footprint and fastest development time, the following simplifications have been made compared to a production-grade app:
1. **Zero External Dependencies**: Removed `OkHttp`, `WorkManager`, `androidx.preference`, and `kotlinx.serialization`. The app will rely purely on built-in Android/Java APIs (`HttpURLConnection`, `org.json.JSONObject`).
2. **Basic Retry Mechanism**: Instead of a complex `WorkManager` setup, failed payloads are temporarily stored in a local queue (e.g., a JSON array in `SharedPreferences`). The app will attempt to flush this queue the next time a notification triggers a webhook send.
3. **Basic UI**: No complex Settings fragments. Just a single `MainActivity` with basic `EditText` fields saving to `SharedPreferences`.
4. **Hardcoded Regex**: Advanced regex overrides are omitted. The MVP uses hardcoded regex patterns known to work for the target apps.

---

## 3. Project Setup & Architecture

### 3.1. Gradle Dependencies (`build.gradle.kts` - app level)
Keep it strictly to the defaults provided by a new Android Studio project.
```kotlin
dependencies {
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.11.0")
    // Note: No external networking or serialization libraries required.
}
```

### 3.2. AndroidManifest.xml
Requires standard internet permissions and the `BIND_NOTIFICATION_LISTENER_SERVICE` permission.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.notificationbridge">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="NotifyBridge"
        android:theme="@style/Theme.NotificationBridge">
        
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <service
            android:name=".BridgeNotificationService"
            android:label="Notification Bridge Listener"
            android:permission="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE"
            android:exported="true">
            <intent-filter>
                <action android:name="android.service.notification.NotificationListenerService" />
            </intent-filter>
        </service>
    </application>
</manifest>
```

---

## 4. Core Implementation: NotificationListenerService

### 4.1. Basic Service Setup & Debouncing
```kotlin
package com.example.notificationbridge

import android.service.notification.NotificationListenerService
import android.service.notification.StatusBarNotification
import android.content.Context

class BridgeNotificationService : NotificationListenerService() {
    
    private var lastValue: Float = Float.NaN
    private var lastPostTime: Long = 0L

    override fun onNotificationPosted(sbn: StatusBarNotification) {
        val prefs = applicationContext.getSharedPreferences("BridgePrefs", Context.MODE_PRIVATE)
        val targetPackage = prefs.getString("TARGET_PACKAGE", "com.medtronic.diabetes.minimedmobile.eu")
        
        if (sbn.packageName != targetPackage) return

        // 1. Debounce (Prevent duplicate parsing within 50 seconds)
        val diffSecs = (sbn.postTime - lastPostTime) / 1000
        if (diffSecs < 50) return 

        // 2. Extract Data
        val glucose = NotificationParser.extractGlucose(sbn, applicationContext)
        
        if (!glucose.isNaN()) {
            if (glucose == lastValue && diffSecs < 250) {
                return // Same value within 4 mins -> redundant
            }
            
            lastValue = glucose
            lastPostTime = sbn.postTime
            
            val webhookUrl = prefs.getString("WEBHOOK_URL", "")
            if (!webhookUrl.isNullOrEmpty()) {
                // 3. Dispatch Webhook
                WebhookEmitter.sendPayload(applicationContext, webhookUrl, sbn.postTime, sbn.packageName, glucose)
            }
        }
    }
}
```

---

## 5. Data Extraction (`NotificationParser.kt`)

Stripped down to exactly what is needed to parse standard extras and `RemoteViews` using hardcoded regex.

```kotlin
package com.example.notificationbridge

import android.service.notification.StatusBarNotification
import android.content.Context
import android.util.Log

object NotificationParser {
    private val defaultGlucoseRegex = """(?:^|\s)(\d+(\.\d)?)(?=\s|\p{S}|$)""".toRegex()

    fun extractGlucose(sbn: StatusBarNotification, context: Context): Float {
        val extras = sbn.notification.extras
        val title = extras.getCharSequence("android.title")?.toString()
        val text = extras.getCharSequence("android.text")?.toString()

        parseString(title)?.let { return it }
        parseString(text)?.let { return it }

        var value = processRemoteViews(sbn.notification.contentView, context)
        if (!value.isNaN()) return value

        return processRemoteViews(sbn.notification.bigContentView, context)
    }

    private fun parseString(input: String?): Float? {
        if (input.isNullOrEmpty()) return null
        val match = defaultGlucoseRegex.find(input)
        return match?.groupValues?.get(1)?.toFloatOrNull()
    }

    private fun processRemoteViews(remoteViews: android.widget.RemoteViews?, context: Context): Float {
        if (remoteViews == null) return Float.NaN
        try {
            val appliedView = remoteViews.apply(context, null)
            return traverseAndFind(appliedView.rootView)
        } catch (e: Exception) {
            return Float.NaN
        }
    }

    private fun traverseAndFind(view: android.view.View): Float {
        when (view) {
            is android.widget.TextView -> {
                val text = view.text.toString().replace(",", ".").trim()
                val parsed = parseString(text)
                if (parsed != null && isValidGlucose(parsed)) return parsed
            }
            is android.view.ViewGroup -> {
                for (i in 0 until view.childCount) {
                    val result = traverseAndFind(view.getChildAt(i))
                    if (!result.isNaN()) return result
                }
            }
        }
        return Float.NaN
    }
    
    private fun isValidGlucose(value: Float): Boolean {
        return (value in 20.0..600.0) || (value in 1.1..33.3)
    }
}
```

---

## 6. Minimal Webhook Emitter with Retry (`WebhookEmitter.kt`)

Uses pure Java `HttpURLConnection` wrapped in a standard `Thread`. Implements a basic queued retry system using `SharedPreferences`.

```kotlin
package com.example.notificationbridge

import android.content.Context
import org.json.JSONArray
import org.json.JSONObject
import java.io.OutputStreamWriter
import java.net.HttpURLConnection
import java.net.URL
import kotlin.concurrent.thread

object WebhookEmitter {
    private const val QUEUE_KEY = "FAILED_PAYLOAD_QUEUE"

    fun sendPayload(context: Context, webhookUrl: String, timestamp: Long, sourcePackage: String, glucose: Float) {
        val payload = JSONObject().apply {
            put("timestamp", timestamp)
            put("source", sourcePackage)
            put("glucose", glucose)
        }
        
        enqueuePayload(context, payload)
        flushQueue(context, webhookUrl)
    }

    private fun enqueuePayload(context: Context, payload: JSONObject) {
        val prefs = context.getSharedPreferences("BridgePrefs", Context.MODE_PRIVATE)
        val queueStr = prefs.getString(QUEUE_KEY, "[]")
        val queue = JSONArray(queueStr)
        queue.put(payload)
        prefs.edit().putString(QUEUE_KEY, queue.toString()).apply()
    }

    private fun flushQueue(context: Context, webhookUrl: String) {
        thread {
            val prefs = context.getSharedPreferences("BridgePrefs", Context.MODE_PRIVATE)
            val queueStr = prefs.getString(QUEUE_KEY, "[]")
            val queue = JSONArray(queueStr)
            
            if (queue.length() == 0) return@thread

            val successfulIndices = mutableListOf<Int>()

            for (i in 0 until queue.length()) {
                val payload = queue.getJSONObject(i)
                try {
                    val url = URL(webhookUrl)
                    val conn = url.openConnection() as HttpURLConnection
                    conn.requestMethod = "POST"
                    conn.setRequestProperty("Content-Type", "application/json; charset=UTF-8")
                    conn.doOutput = true
                    conn.connectTimeout = 5000
                    conn.readTimeout = 5000

                    OutputStreamWriter(conn.outputStream).use { writer ->
                        writer.write(payload.toString())
                        writer.flush()
                    }

                    if (conn.responseCode in 200..299) {
                        successfulIndices.add(i)
                    }
                } catch (e: Exception) {
                    e.printStackTrace()
                    break // Stop processing the queue on network failure to preserve order and save battery
                }
            }

            // Remove successful payloads from queue
            if (successfulIndices.isNotEmpty()) {
                val newQueue = JSONArray()
                for (i in 0 until queue.length()) {
                    if (!successfulIndices.contains(i)) {
                        newQueue.put(queue.getJSONObject(i))
                    }
                }
                prefs.edit().putString(QUEUE_KEY, newQueue.toString()).apply()
            }
        }
    }
}
```

---

## 7. Configuration UI (`MainActivity.kt`)

A simple Activity with two input fields and a button to request permissions.

```kotlin
package com.example.notificationbridge

import android.content.Context
import android.content.Intent
import android.os.Bundle
import android.provider.Settings
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val prefs = getSharedPreferences("BridgePrefs", Context.MODE_PRIVATE)
        
        val editPackage = findViewById<EditText>(R.id.editPackage)
        val editWebhook = findViewById<EditText>(R.id.editWebhook)
        val btnSave = findViewById<Button>(R.id.btnSave)
        val btnPermission = findViewById<Button>(R.id.btnPermission)

        editPackage.setText(prefs.getString("TARGET_PACKAGE", "com.medtronic.diabetes.minimedmobile.eu"))
        editWebhook.setText(prefs.getString("WEBHOOK_URL", ""))

        btnSave.setOnClickListener {
            prefs.edit().apply {
                putString("TARGET_PACKAGE", editPackage.text.toString())
                putString("WEBHOOK_URL", editWebhook.text.toString())
                apply()
            }
            Toast.makeText(this, "Saved!", Toast.LENGTH_SHORT).show()
        }

        btnPermission.setOnClickListener {
            startActivity(Intent(Settings.ACTION_NOTIFICATION_LISTENER_SETTINGS))
        }
    }
}
```