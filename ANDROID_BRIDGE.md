# Android Notification Bridge - Production Grade Specification

## 1. Overview
The **Android Notification Bridge** is a lightweight, background-first Android application designed to intercept local system notifications from specific medical apps (e.g., Medtronic MiniMed), extract critical health data (Glucose values, IOB) via regex, and dispatch this data to a configured Webhook URL.

This document outlines the technical specification required to implement this app with a **production-grade retry mechanism** using Android's `WorkManager`.

---

## 2. Architecture & Reliability

The following architectural choices ensure reliability and maintainability:
1. **WorkManager Integration**: Uses `WorkManager` for all network operations. This provides built-in persistence, constraints (e.g., waiting for network), and a professional retry policy with exponential backoff.
2. **Minimal UI**: A single `MainActivity` with basic `EditText` fields saving to `SharedPreferences`.
3. **Hardcoded Regex**: The app uses optimized regex patterns known to work for the target apps.

---

## 3. Project Setup & Architecture

### 3.1. Gradle Dependencies (`build.gradle.kts` - app level)
```kotlin
dependencies {
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.11.0")
    
    // WorkManager for production-grade retries
    implementation("androidx.work:work-runtime-ktx:2.9.0")
}
```

### 3.2. AndroidManifest.xml
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
import androidx.work.*

class BridgeNotificationService : NotificationListenerService() {
    
    private var lastValue: Float = Float.NaN
    private var lastPostTime: Long = 0L

    override fun onNotificationPosted(sbn: StatusBarNotification) {
        val prefs = applicationContext.getSharedPreferences("BridgePrefs", Context.MODE_PRIVATE)
        val targetPackage = prefs.getString("TARGET_PACKAGE", "com.medtronic.diabetes.minimedmobile.eu")
        
        if (sbn.packageName != targetPackage) return

        val diffSecs = (sbn.postTime - lastPostTime) / 1000
        if (diffSecs < 50) return 

        val glucose = NotificationParser.extractGlucose(sbn, applicationContext)
        
        if (!glucose.isNaN()) {
            if (glucose == lastValue && diffSecs < 250) return
            
            lastValue = glucose
            lastPostTime = sbn.postTime
            
            val webhookUrl = prefs.getString("WEBHOOK_URL", "")
            if (!webhookUrl.isNullOrEmpty()) {
                enqueueWebhookWork(webhookUrl, sbn.postTime, sbn.packageName, glucose)
            }
        }
    }

    private fun enqueueWebhookWork(url: String, time: Long, pkg: String, value: Float) {
        val data = workDataOf(
            "URL" to url,
            "TIME" to time,
            "SOURCE" to pkg,
            "VALUE" to value
        )

        val request = OneTimeWorkRequestBuilder<WebhookWorker>()
            .setInputData(data)
            .setConstraints(Constraints.Builder().setRequiredNetworkType(NetworkType.CONNECTED).build())
            .setBackoffCriteria(BackoffPolicy.EXPONENTIAL, WorkRequest.MIN_BACKOFF_MILLIS, java.util.concurrent.TimeUnit.MILLISECONDS)
            .build()

        WorkManager.getInstance(applicationContext).enqueue(request)
    }
}
```

---

## 5. Data Extraction (`NotificationParser.kt`)
(Unchanged from minimal specification - see previous versions for full code)

---

## 6. Production Webhook Worker (`WebhookWorker.kt`)

Uses `CoroutineWorker` for asynchronous execution and `HttpURLConnection` for a zero-dependency network stack.

```kotlin
package com.example.notificationbridge

import android.content.Context
import androidx.work.CoroutineWorker
import androidx.work.WorkerParameters
import org.json.JSONObject
import java.io.OutputStreamWriter
import java.net.HttpURLConnection
import java.net.URL

class WebhookWorker(appContext: Context, workerParams: WorkerParameters) :
    CoroutineWorker(appContext, workerParams) {

    override suspend fun doWork(): Result {
        val webhookUrl = inputData.getString("URL") ?: return Result.failure()
        val timestamp = inputData.getLong("TIME", 0L)
        val source = inputData.getString("SOURCE") ?: ""
        val value = inputData.getFloat("VALUE", 0.0f)

        return try {
            val payload = JSONObject().apply {
                put("timestamp", timestamp)
                put("source", source)
                put("glucose", value)
            }

            val conn = (URL(webhookUrl).openConnection() as HttpURLConnection).apply {
                requestMethod = "POST"
                setRequestProperty("Content-Type", "application/json; charset=UTF-8")
                doOutput = true
                connectTimeout = 10000
                readTimeout = 10000
            }

            OutputStreamWriter(conn.outputStream).use { it.write(payload.toString()) }

            if (conn.responseCode in 200..299) {
                Result.success()
            } else {
                Result.retry()
            }
        } catch (e: Exception) {
            if (runAttemptCount < 10) Result.retry() else Result.failure()
        }
    }
}
```

---

## 7. Configuration UI (`MainActivity.kt`)
(Unchanged from minimal specification - see previous versions for full code)