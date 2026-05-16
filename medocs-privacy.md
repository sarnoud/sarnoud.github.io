# Privacy Policy for Medocs

**Last Updated: May 15, 2026**

This Privacy Policy describes how the Medocs application ("we", "us", or "our") handles your personal information when you use our web application and associated services.

## 1. Data Collection and Use

Medocs is designed to help you track medicine intake using your own Google ecosystem. We collect and process the following information:

- **Google Account Information:** When you log in via Google OAuth2, we receive your email address. This is used solely to identify you and map your session to your specific configuration and Google Spreadsheet.
- **Medicine Logs:** Any data you enter (medicine name, date, time, and optional dosage notes) is sent to a Google Spreadsheet that you or your administrator has configured.
- **Health Data (Optional):** If you explicitly enable Google Health/Fit synchronization, we may read activity data (e.g., steps, activity types) to log them to your spreadsheet. This data is only accessed with your explicit consent and is never stored on our servers beyond transient processing.

## 2. Data Storage and Security

- **Storage Location:** All your medicine logs are stored directly in a Google Spreadsheet. We do not maintain a separate database of your logs.
- **Authentication:** We use industry-standard Google OAuth2 for authentication. We do not see or store your Google password.
- **Cookies:** We use a secure, HTTP-only session cookie (`auth_tokens`) to maintain your logged-in state. This cookie contains encrypted tokens to verify your identity with Google.
- **Service Account:** The application uses a dedicated Google Service Account to write to your spreadsheet. This account must be granted "Editor" access to your specific spreadsheet for the application to function.

## 3. Data Sharing and Disclosure

- **No Third-Party Sharing:** We do not sell, trade, or otherwise transfer your personal information or medicine logs to third parties.
- **Google Services:** Your data is shared with Google services (Sheets, Fit) as part of the core functionality of the application. Please refer to [Google's Privacy Policy](https://policies.google.com/privacy) for how they handle your data.

## 4. User Control and Data Deletion

- **Access and Correction:** Since your data resides in your Google Spreadsheet, you have full control over it. You can view, edit, or delete any entry directly in the spreadsheet at any time.
- **Account Disconnection:** You can revoke the application's access to your Google account at any time via your [Google Account Security settings](https://myaccount.google.com/permissions).
- **Session Termination:** Using the "Logout" feature in the application will clear all local authentication cookies and cached session data.

## 5. Changes to This Policy

We may update this Privacy Policy from time to time. Any changes will be reflected by the "Last Updated" date at the top of this document.
