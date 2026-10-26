# Messaging Platform Compatibility Report (Google Pixel 6a / Android 14+)

This report documents verified notification behavior, inline-reply capabilities (`RemoteInput`), and official API boundaries for **WhatsApp**, **Instagram Direct Messages**, and **Facebook Messenger** on Android 14+.

| Platform | Android Package | Notification Detection (`NotificationListenerService`) | Direct Inline Reply (`RemoteInput`) | Official Cloud API Status for Personal Accounts | Fallback Behavior |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WhatsApp** | `com.whatsapp` | **Supported** (Exposes sender in `EXTRA_TITLE` and message in `EXTRA_TEXT` / `EXTRA_BIG_TEXT` when notification content is visible) | **Supported** on standard 1:1 and group text notifications via `Notification.Action` + `RemoteInput` | WhatsApp Business Platform API applies only to registered Business phone numbers, not personal WhatsApp accounts | If user hides notification content on lockscreen or notification action expires, copies reply to clipboard & launches chat |
| **Instagram Direct** | `com.instagram.android` | **Supported** when DM push notifications are enabled in Instagram Settings | **Partial / Conditional** (Standard text DM notifications often expose `RemoteInput`, whereas media/reel shares, vanish mode, or bundled alerts omit `RemoteInput`) | Meta Instagram Graph API for Messaging is restricted to Instagram Professional (Business/Creator) accounts with connected Facebook Pages—**not available for personal DM inboxes** | Displays clear **"Manual Send Required"** badge, copies selected reply to clipboard, and fires `contentIntent` to open the exact Instagram DM thread |
| **Facebook Messenger** | `com.facebook.orca` | **Supported** when Android notifications are active | **Supported on standard notifications**; may be suppressed if Messenger Chat Heads / Android Bubbles already have the thread open in the foreground | Meta Messenger Platform API is strictly for Facebook Pages messaging customers—**not personal Messenger profiles** | Displays **"Manual Send Required"** badge, copies reply to clipboard, and opens the Messenger thread |

## Critical Platform Limitations Disclosed to Users

1. **Notification-Only Scope**: `NotificationListenerService` only receives messages that trigger an Android status-bar notification. Messages arriving while the user is actively viewing that chat inside WhatsApp/Instagram/Messenger, muted chats with notifications disabled, or deleted messages are not exposed by Android.
2. **No Unauthorized Automation**: SmartReply AI strictly avoids unauthorized `AccessibilityService` screen-scraping, root access, or modified client APKs, ensuring compliance with Android security policies and preventing account bans.
