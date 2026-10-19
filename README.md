# RUBU — Free-First Multi-User Social Media Platform

Welcome to **RUBU** (বাংলা: **রুবু**), a free-first, modern, responsive multi-user social media application designed for seamless community engagement, zero upfront development costs, and future Google Play Store publishing (`com.rubu.social`).

---

## 🚀 Key Features Implemented

### 1. Multi-User Authentication & Accounts
- **Account Registration & Login**: Email & password authentication with unique `@username` validation.
- **Session Management & Logout**: Real-time reactive current user state.
- **Instant Multi-Account Testing (Zero-Cost)**: Integrated account switcher modal permitting instant switching between pre-configured accounts:
  - `@arif` (Arif Rahman) — Standard Creator
  - `@nadia` (Nadia Islam) — Standard Creator
  - `@tahmid` (Tahmid Hasan) — Community Member
  - `@admin` (Rubu Admin) — Verified Platform Administrator
- **Password Reset**: Self-service simulated password recovery flow.

### 2. Live Home Feed & Social Interaction
- **Multi-User Feed**: Posts from all non-blocked users displayed in reverse chronological order.
- **Love Reactions**:
  - Interactive animated heart reactions with count.
  - Multi-account synchronization (e.g. Arif posts, Nadia adds a Love, both see the updated reaction count in real-time).
  - Duplicate reaction prevention.
- **Comments System**:
  - Full comments bottom sheet/dialog per post.
  - Comment deletion restricted to author or authorized admin.
  - Dynamic comment count updates.
- **Post Management**:
  - Create posts with captions up to 500 characters.
  - Zero-cost photographic attachment presets & image URL support.
  - Delete post (owner or administrator only; others strictly denied).

### 3. Profiles, Following & Discovery
- **User Profiles**: Displays avatar, display name, handle, verified badge, bio, and live counts for Posts, Followers, and Following.
- **Follow / Unfollow**: Real-time counter synchronization across both follower and following profiles.
- **Explore & Search**: Real-time search by username or display name, creator recommendations, and trending hashtag topics (`#RubuSocial`, `#BangladeshTech`, etc.).
- **Safety Actions**: Block user (immediately hides user's posts from feed) and Report action.

### 4. In-App Notification Center
- Real-time in-app alerts with unread badge counter for:
  - New Love reactions on user's posts
  - New comments on user's posts
  - New followers
- One-tap "Mark all read" feature.

### 5. Bilingual Support (Bangla & English)
- Instant toggle in the top bar (`EN` / `বাংলা`).
- Full localization across all navigation tabs, labels, actions, dates, and administrative alerts.

### 6. Light & Dark Themes
- High-contrast, modern theme inspired by deep midnight slate (`#0B1120`), vivid cyan/azure primary (`#0284C7`), and energetic rose love accent (`#F43F5E`).
- One-tap theme switcher in the top bar.

### 7. Independent Administrator Panel
- Dedicated route / screen with verified admin authorization gate.
- **Security Invariant**: Non-admins receive an explicit `Access Restricted` shield. Elevation via client profile editing is blocked.
- **Live Statistics**: Total Users, Total Posts, Love Reactions, Pending Reports, Suspended Accounts.
- **User Management**: One-click account suspension and reactivation.
- **Content Moderation**: Review and delete inappropriate posts.
- **User Reports Queue**: Review reported content with resolution actions ("Action Taken" / "Dismiss").
- **Immutable Audit Trail**: Chronological log of all administrative actions.

---

## 🔐 Administrator Security Configuration

To ensure strict zero-trust security:

1. **Local Architecture**: Admin verification checks `currentUser.role == "admin"` and confirms matching with `RubuRepository.ADMIN_UID` (`admin_master_uid`) or `admin@rubu.com`.
2. **Cloud Firestore Rules (`firestore.rules`)**:
   ```javascript
   function isAdministrator() {
     return isAuthenticated() && (
       request.auth.uid == "YOUR_SECURE_ADMIN_UID" ||
       request.auth.token.email == "admin@rubu.com"
     );
   }
   ```
   *To change your admin UID*: Replace `"admin_master_uid"` in `firestore.rules` and `RubuRepository.ADMIN_UID` with your own Firebase Authentication UID after logging in.

---

## 🧪 Testing Checklist & Verification

All automated unit and Robolectric tests passed successfully (`gradle :app:testDebugUnitTest`):
- ✅ **Test 1**: User registration with unique username check (rejects duplicate `@arif`, accepts unique handles).
- ✅ **Test 2**: Login & logout session state.
- ✅ **Test 3**: Post creation & author post counter increment.
- ✅ **Test 4**: Love reaction toggle across accounts (Arif reacts -> Nadia reacts -> total 2 -> Nadia unreacts -> total 1).
- ✅ **Test 5**: Security permissions: Author allowed to delete own post; unauthorized user strictly denied.
- ✅ **Test 6**: Admin authorization: Standard user denied access; configured admin granted access.
- ✅ **Test 7**: Admin moderation: Account suspension toggle & audit log entry generation.
- ✅ **Test 8**: Comments and follow flow between separate accounts.

---

## 📦 Zero-Cost Google Play Release Preparation

When you are ready to export and package RUBU for the Google Play Store:

1. **Application ID**: Configured as `com.rubu.social` in `app/build.gradle.kts`.
2. **Branding & Launcher Icon**: Custom adaptive icon created with Rubu's glowing cyan heart-R emblem and solid `#0F172A` background.
3. **No-Cost Backend (Firebase Spark Plan)**:
   - When ready for cloud sync, provision Firebase Firestore on the free Spark tier (no billing required).
   - Deploy `firestore.rules` for zero-trust authorization.
4. **Generating APK / AAB**:
   - In Google AI Studio: Use the settings menu to download your project ZIP or generate release APK/AAB.
   - Or locally: Run `gradle :app:bundleRelease` to produce an Android App Bundle (`.aab`) ready for upload to Google Play Console.
