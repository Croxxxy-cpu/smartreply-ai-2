# Test Plan & Implementation Matrix — SmartReply AI

## 1. Feature Implementation Status Classification

| Feature Category | Status | Notes |
| :--- | :--- | :--- |
| **Material Design 3 UI (Dark/Light Mode, Pixel 6a Layout, Dashboard, Reply Sheet, Personalization, Privacy)** | **Fully Implemented** | Interactive in Canvas Web App + Native Jetpack Compose source in `MainActivity.kt` |
| **10 Distinct Context-Adaptive Reply Suggestions (Natural, Friendly, Funny, Witty, Professional, Flirty, Romantic, Short, Thoughtful, Creative)** | **Fully Implemented** | Automatically suppresses Flirty/Romantic/Funny when work, urgent, or sensitive context is detected |
| **Multilingual Generation: English, Hindi (हिन्दी), and Odia (ଓଡ଼ିଆ)** | **Fully Implemented** | Supported in both live OpenAI/Gemini prompts and built-in offline fallback engine |
| **Dual AI Provider Layer (ChatGPT Only, Gemini Only, Both Combined + Deduplicated + Ranked, AI Recommended)** | **Fully Implemented** | Parallel execution, similarity deduplication, and thumbs-up/down category weight ranking |
| **Android `NotificationListenerService` & `RemoteInput` Inline Reply** | **Implemented in Native Kotlin Source** | Requires compiling/installing the Android project on the physical Pixel 6a and granting Notification Access |
| **Meta Graph API for Instagram/Messenger Direct Integration** | **Requires Account Eligibility** | Only eligible for Business/Creator accounts linked to a Facebook Page; personal accounts use Android Notification + Manual Fallback |

## 2. Verification Test Cases

| Test ID | Scenario | Steps | Expected Outcome |
| :--- | :--- | :--- | :--- |
| **TC-01** | Incoming WhatsApp Notification (Phase 1 MVP) | Simulate or receive a WhatsApp message with `RemoteInput` available | App detects `com.whatsapp`, extracts sender & message, generates 10 distinct suggestions, and shows **Inline Reply Ready** |
| **TC-02** | Instagram DM without `RemoteInput` (Fallback) | Simulate an Instagram reel/DM notification where `RemoteInput` is absent | App shows **"Manual Send Required"** badge; tapping Send copies reply & opens Instagram DM without falsely claiming the message was auto-sent |
| **TC-03** | Serious / Work Context Safety Filter | Input message: *"Urgent: Client presentation deadline moved to 4 PM today"* | Categories 3, 4, 6, 7 adapt from Funny/Witty/Flirty/Romantic to Direct, Action-Oriented, Supportive, and Polite Decline |
| **TC-04** | Multilingual Output (English, Hindi, Odia) | Switch language selector between **English**, **Hindi (हिन्दी)**, and **Odia (ଓଡ଼ିଆ)** | All 10 suggestions regenerate in natural English, Devanagari Hindi, or Odia script |
| **TC-05** | Combined Provider Deduplication & Ranking | Select **Both Providers** mode and thumbs-up a preferred tone (e.g., `Thoughtful`) | Suggestions from ChatGPT and Gemini are merged, duplicates removed, and preferred tone ranked higher |
| **TC-06** | Explicit User Approval Before Sending | Tap **Send** on any suggestion | Confirmation prompt appears before transmitting; user can Cancel, Edit, or Approve & Send |
| **TC-07** | Error Handling (Disabled Permission, Missing Key, Offline, Rate Limit, Empty Text) | Trigger each state in the Error & Edge-Case Simulator | Displays clear, non-crashing error banner with actionable recovery button and manual fallback |
| **TC-08** | Privacy Blocklist & Data Wipe | Add a contact to Blocked Contacts or disable Instagram; click **Delete All Stored Data** | Blocked contact/app notifications are ignored; Data Wipe clears all stored preferences, history, and keys |
