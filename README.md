# SmartReply AI — Google Pixel 6a (Android 14+)

**SmartReply AI** is a privacy-first, Material Design 3 Android application engineered for the **Google Pixel 6a**. It integrates **OpenAI ChatGPT** and **Google Gemini** to analyze incoming message notifications from **WhatsApp**, **Instagram Direct Messages**, and **Facebook Messenger**, generating **10 distinct, context-aware reply suggestions** across English, Hindi (हिन्दी), and Odia (ଓଡ଼ିଆ).

## Key Architecture & Features

1. **Official Notification Detection (`SmartReplyNotificationListener.kt`)**
   - Uses Android's documented `NotificationListenerService` after explicit user permission.
   - Filters for `com.whatsapp`, `com.instagram.android`, and `com.facebook.orca`.
   - Suppresses duplicate group summaries (`FLAG_GROUP_SUMMARY`) and deduplicates repeated alerts via SHA-256 hashing.
   - Inspects `Notification.Action` for `RemoteInput` to determine in real time whether direct inline reply is supported.

2. **User-Approved Sending & Honest Fallback (`ReplyDispatcher.kt`)**
   - **Never sends automatically without user confirmation** and never claims a message was sent unless `RemoteInput` execution succeeds.
   - When `RemoteInput` is exposed by the notification, dispatches the user-approved reply via `RemoteInput.addResultsToIntent(...)` and `action.actionIntent.send(...)`.
   - When `RemoteInput` is unavailable (e.g., certain Instagram DM notifications or locked notifications), automatically copies the selected reply to the clipboard and opens the target conversation (`contentIntent` or package launch) with a clear indicator that manual sending is required.

3. **Modular Dual-AI Engine (`SmartReplyOrchestrator.kt` & `AiApiServices.kt`)**
   - Official Retrofit clients for **OpenAI** (`v1/chat/completions`, `gpt-4o-mini`) and **Google Gemini** (`v1beta/models/gemini-2.5-flash:generateContent`).
   - Supports 4 provider modes: **ChatGPT Only**, **Gemini Only**, **Both Providers (Combined, Deduplicated & Ranked Top 10)**, and **AI Recommended Provider**.
   - Generates 10 adaptive categories (`Natural`, `Friendly`, `Funny`, `Witty`, `Professional`, `Flirty`, `Romantic`, `Short`, `Thoughtful`, `Creative`) and automatically replaces `Flirty`/`Romantic`/`Funny` with professional/supportive alternatives when work, urgent, or sensitive context is detected.

4. **Android Keystore Privacy & Personalization (`EncryptedCredentialStore.kt`)**
   - Encrypts OpenAI and Gemini API keys using `MasterKey (AES256_GCM)` and `EncryptedSharedPreferences`.
   - Supports **English**, **Hindi (हिन्दी)**, and **Odia (ଓଡ଼ିଆ)**, plus length, formality, humor, emoji preferences, per-app toggles, blocked contacts, explicit Thumbs-Up/Thumbs-Down feedback weighting, and one-tap **Wipe All User Data**.

## Build & Installation Instructions (Google Pixel 6a)

1. **Open in Android Studio**
   - Extract `SmartReplyAI_Pixel6a_Android_Project.zip` and open the folder in **Android Studio Koala (2024.1.1) or newer** with JDK 17.
2. **Enable USB Debugging on your Google Pixel 6a**
   - Go to **Settings → About phone → tap Build number 7 times** to enable Developer Options.
   - Go to **Settings → System → Developer options → enable USB debugging**.
3. **Build & Install APK**
   - Run `./gradlew assembleDebug` or click **Run 'app'** in Android Studio with your Pixel 6a connected via USB.
4. **Grant Notification Access Permission**
   - On Android 13/14+ (Pixel 6a), if installing an APK outside Play Store, go to **Settings → Apps → SmartReply AI → tap top-right 3 dots → Allow restricted settings**.
   - Open **SmartReply AI → tap Permissions** (or go to **Settings → Notifications → Device & app notifications**) and toggle **SmartReply AI** ON.
5. **Configure API Keys**
   - Enter your **OpenAI API Key** (`sk-...`) and/or **Google Gemini API Key** (`AIzaSy...`) in the Settings panel. Keys are encrypted on-device via the Pixel 6a Titan M2 Android Keystore.
