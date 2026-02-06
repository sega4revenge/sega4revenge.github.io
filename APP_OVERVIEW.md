# DoseMate - Application Overview

## 📋 Table of Contents
- [Introduction](#introduction)
- [Core Features](#core-features)
- [Technical Architecture](#technical-architecture)
- [Module Breakdown](#module-breakdown)
- [State Management](#state-management)
- [Cloud Services](#cloud-services)
- [User Experience](#user-experience)
- [Development](#development)

---

## Introduction

**DoseMate** is a medication management and reminder application built with Flutter for iOS and Android. The app helps users track prescriptions, manage medication schedules, and maintain health records with robust cloud synchronization and calendar integration.

### Key Highlights
- 💊 **Prescription Management**: Manual entry with detailed medication tracking
- 📅 **Smart Scheduling**: Flexible medication timers (Morning, Noon, Evening, Night)
- ☁️ **Cloud Sync**: Google Drive (Android/iOS) and iCloud (iOS) integration
- 🗓️ **Calendar Integration**: Sync to Google Calendar and Apple Calendar
- 📱 **QR Sharing**: Export/import prescriptions via QR codes
- 🎨 **Modern UI**: Material Design 3 with dark mode
- 🌍 **Multi-Language**: English and Vietnamese support
- 🔔 **Local Notifications**: Reliable medication reminders

---

## Core Features

### 💊 Prescription Management

#### Manual Entry
Users can create prescriptions with comprehensive details:

**Prescription Information:**
- Name (e.g., "Daily Vitamins", "Heart Medication")
- Color coding (8 preset colors for visual distinction)
- Cover image (optional)
- Start options: Now, Never, Scheduled (custom date)
- End date (optional)
- Sync preferences (Calendar, Backup)

**Medication Details** (Multiple medications per prescription):
- Name
- Icon (pill, tablet, capsule, syringe, bottle, etc.)
- Dosage (e.g., "500mg", "10ml")
- Quantity per dose (e.g., "2 tablets")
- Unit (pills, tablets, capsules, ml, mg, bottles, vials, packages)
- Frequency (Daily, Weekly, Custom)
- Timing (Morning, Noon, Evening, Night - multiple selections)
- Meal timing (Before meals, After meals, With meals, Empty stomach, Any time)
- Duration (number of days)
- Notes (special instructions)

#### QR Code Import/Export
- **Export**: Generate QR code containing full prescription data
  - Includes all prescription metadata
  - All medication details with icons, units, and schedules
  - Color and sync preferences
  - Shareable via any messaging app
  
- **Import**: Scan QR code to instantly add prescription
  - Full data validation
  - Preview before saving
  - Automatic conflict resolution

### 📅 Daily Timeline & Dashboard

#### Home Screen
The Home screen provides a comprehensive daily view:

**Real-Time Clock**
- Current time display
- Dynamic greeting (Good Morning/Afternoon/Evening/Night)
- Updates every second

**Medication Timeline**
Organized by time of day:
- **Morning** (6:00 AM - 11:59 AM)
- **Noon** (12:00 PM - 4:59 PM)
- **Evening** (5:00 PM - 8:59 PM)
- **Night** (9:00 PM - 5:59 AM)

**Medication Cards**
Each medication displays:
- Medication icon and name
- Dosage and quantity
- Prescription name (with color badge)
- Scheduled time
- Status indicator (Taken/Missed/Pending)

**Quick Actions**
- ✅ **Take**: Mark medication as taken (records timestamp)
- ⏭️ **Skip**: Skip this dose
- Tap card to view full prescription details

**Hurray Card**
- Appears when all daily medications are completed
- Celebratory message and confetti animation
- Motivational reinforcement for adherence

### 🗓️ Calendar Integration

#### In-App Calendar
- **Monthly View**: Calendar grid with medication markers
- **Weekly View**: Detailed week timeline
- **Day Markers**: Visual indicators for scheduled medications
  - Single dot for days with medications
  - Multiple dots for high-density days
- **Date Navigation**: Tap date to see that day's schedule
- **Month Navigation**: Swipe or use arrows

#### External Calendar Sync

**Google Calendar** (Android & iOS)
- One-time Google sign-in
- Auto-sync toggle (enable/disable)
- Creates calendar events for each medication dose
- Event format:
  ```
  Title: 💊 Take [Medication] [Dosage]
  Description: Prescription details, dosage, notes
  Time: Scheduled medication time
  ```
- Manual sync button
- Remove all events option

**Apple Calendar** (iOS Only)
- Native iOS EventKit integration
- Permission request flow
- Auto-sync when enabled
- Same event format as Google Calendar
- Respects iOS privacy settings

### ☁️ Cloud Sync & Backup

#### Multi-Platform Strategy

**Google Drive** (Android & iOS)
- Google Sign-In authentication (shared with Calendar)
- JSON backup to App Data folder
- Auto-backup on data changes (optional)
- Manual backup/restore buttons
- Encrypted storage
- Full prescription export/import

**iCloud Drive** (iOS Only)
- Auto-enabled on iOS devices
- No login required (uses Apple ID)
- Background sync to iCloud Documents
- Auto-backup toggle
- Manual backup/restore
- Seamless iOS integration

#### Sync Features
- **Offline-First**: All data stored locally in Hive
- **Auto-Sync**: Configurable per service
- **Manual Controls**: Backup and Restore buttons
- **Error Handling**: User-friendly error messages with auto-retry
- **Conflict Resolution**: Last-write-wins strategy

#### Backup Format
```json
{
  "version": "1.0",
  "exportDate": "2026-01-25T22:00:00Z",
  "prescriptions": [
    {
      "id": "uuid",
      "name": "Daily Vitamins",
      "color": 2,
      "medications": [...],
      "startOption": "now",
      "endDate": "2026-02-25T00:00:00Z"
    }
  ]
}
```

### 🔔 Smart Notifications

#### Local Notifications
- Scheduled for each medication dose
- Notification content:
  - Title: "Time to take [Medication]"
  - Body: Dosage and quantity details
  - Icon: Medication icon
- Persistent reminders
- Respects Do Not Disturb mode

#### Notification Settings
- Global enable/disable toggle
- Per-prescription mute (future)
- System notification settings integration

### 📊 Prescription List & Details

#### Prescription List
- Grid/List view of all prescriptions
- Color-coded cards
- Status indicators:
  - 🟢 **Active**: Currently running
  - 🟡 **Scheduled**: Starts in future
  - 🔴 **Expired**: End date reached
  - ⚫ **Not Scheduled**: Never started
- Quick actions menu
- Search and filter (future)

#### Prescription Detail View
- Complete prescription information
- All medications with full details
- Timeline visualization
- Current status
- Edit prescription
- Export to QR code
- Delete prescription
- Share via QR

---

## Technical Architecture

### Technology Stack

```yaml
Flutter SDK: 3.9+
Dart: 3.9+

Core Dependencies:
  # State Management
  flutter_bloc: ^9.1.1          # BLoC pattern
  freezed: ^3.1.0               # Immutable models
  freezed_annotation: ^3.1.0
  json_annotation: ^4.9.0
  
  # Dependency Injection
  get_it: ^9.2.0                # Service locator
  
  # Navigation
  auto_route: ^11.1.0           # Type-safe routing
  
  # Local Storage
  hive: ^2.2.3                  # NoSQL database
  hive_flutter: ^1.1.0
  
  # Authentication & Cloud
  google_sign_in: ^7.2.0        # Google OAuth
  googleapis: ^15.0.0           # Google APIs (Drive, Calendar)
  
  # UI Components
  table_calendar: ^3.2.0        # Calendar widget
  mobile_scanner: ^7.1.4        # QR scanner
  pretty_qr_code: ^3.3.0        # QR generator
  
  # Notifications
  flutter_local_notifications: ^18.0.1
  timezone: ^0.9.0
  device_calendar: ^4.3.2       # iOS EventKit
  
  # Utilities
  intl: ^0.20.2                 # Internationalization
  easy_localization: ^3.0.8     # i18n
  path_provider: ^2.1.4         # File paths
  share_plus: ^12.0.1           # Share functionality
  
  # Firebase
  firebase_core: ^4.3.0
  firebase_analytics: ^12.1.0
  firebase_crashlytics: ^5.0.6
  firebase_messaging: ^16.1.0
  
  # Monetization
  purchases_flutter: ^9.10.4    # RevenueCat (IAP)
  google_mobile_ads: ^5.2.0     # AdMob
  
  # Deep Linking
  app_links: ^6.3.2

Dev Dependencies:
  build_runner: ^2.4.13
  auto_route_generator: ^11.0.0
  freezed_codegen: ^3.1.0
  json_serializable: ^6.8.0
  hive_generator: ^2.0.1
```

### Project Structure

```
lib/app/
├── bloc/                       # State management
│   ├── core/                  # App-level BLoCs
│   │   ├── session/          # Authentication state
│   │   ├── setting/          # App settings & sync
│   │   ├── revenue_cat/      # Subscriptions
│   │   └── timer/            # Real-time clock
│   └── features/             # Feature BLoCs
│       ├── home/             # Home dashboard
│       ├── prescription_detail/
│       ├── prescription_form/
│       ├── prescription_list/
│       ├── calendar/
│       └── subscription/
│
├── data/                      # Data layer
│   ├── local/                # Hive storage
│   │   ├── hive_service.dart
│   │   ├── settings_adapter.dart
│   │   └── prescription_adapter.dart
│   └── repositories/         # Repository implementations
│       ├── prescription_repository.dart
│       ├── settings_repository.dart
│       └── auth_repository.dart
│
├── models/                    # Data models
│   ├── prescription_model.dart
│   ├── medication_model.dart
│   ├── settings_model.dart
│   ├── prescription_color.dart
│   └── sync/                 # Cloud sync models
│       ├── google_setting.dart
│       ├── apple_setting.dart
│       ├── google_drive_setting.dart
│       ├── google_calendar_setting.dart
│       ├── icloud_setting.dart
│       └── apple_calendar_setting.dart
│
├── pages/                     # UI screens
│   ├── core/
│   │   ├── splash/           # Splash screen
│   │   ├── tab/              # Bottom navigation
│   │   │   ├── home/        # Home timeline
│   │   │   └── calendar/    # Calendar view
│   │   └── settings/         # Settings screens
│   │       ├── settings_view.dart
│   │       ├── sync_settings_view.dart
│   │       ├── theme_settings_view.dart
│   │       └── language_settings_view.dart
│   ├── add_prescription/     # Prescription form
│   ├── prescription_detail/  # Detail view
│   ├── prescription_list/    # List view (future)
│   ├── qr_scanner/          # QR import
│   └── debug/               # Debug tools
│
├── services/                  # Business services
│   ├── notification_service.dart
│   ├── google_calendar_service.dart
│   ├── icloud_calendar_service.dart
│   ├── device_id_service.dart
│   ├── app_links_service.dart
│   ├── auth/
│   │   └── google_auth_service.dart
│   └── sync/
│       ├── backup_service.dart
│       ├── cloud_storage_service.dart
│       ├── google_drive_storage_service.dart
│       └── icloud_storage_service.dart
│
├── router/                    # Navigation
│   └── app_router.dart       # Auto-generated routes
│
├── config/                    # Configuration
│   ├── environment_config.dart
│   ├── app_theme.dart
│   └── app_constants.dart
│
├── components/               # Reusable widgets
│   └── layout/
│       └── base_layout.dart
│
└── utils/                    # Utilities
    ├── app_logger.dart
    ├── message_utils.dart
    ├── date_utils.dart
    └── app_exception.dart
```

---

## Module Breakdown

### 🏠 Home Module

**Purpose**: Daily medication dashboard with timeline

**Key Components:**
- `home_view.dart` - Main home screen
- `medication_timeline_widget.dart` - Timeline by time of day
- `medication_schedule_card.dart` - Individual medication card
- `home_bloc.dart` - Home state management

**Features:**
- Real-time clock with greetings
- Medication timeline (Morning/Noon/Evening/Night)
- Quick take/skip actions
- Hurray card for completion
- Pull to refresh

**State:**
```dart
HomeState {
  List<PrescriptionModel> prescriptions,
  Map<String, MedicationStatus> statuses,
  DateTime selectedDate,
  bool showHurray,
  HomeStatus status
}
```

### 💊 Prescription Module

**Components:**
- `prescription_form_view.dart` - Add/Edit form
- `prescription_detail_view.dart` - Detail screen
- `qr_export_dialog.dart` - QR generation
- `prescription_detail_bloc.dart` - State management

**Features:**
- Dynamic medication list (add/remove)
- Form validation
- Color selection
- Icon picker for medications
- Schedule configuration
- QR export with full data
- Edit existing prescriptions
- Delete with confirmation

### 📷 QR Scanner Module

**Components:**
- `qr_scanner_view.dart` - Scanner screen
- Mobile scanner integration

**Features:**
- QR code scanning
- Data validation
- Import preview
- Conflict handling

### 📅 Calendar Module

**Components:**
- `calendar_view.dart` - Calendar screen
- Table calendar integration

**Features:**
- Month/week toggle
- Medication markers
- Day detail view
- Navigation controls

### ⚙️ Settings Module

**Screens:**
1. **Main Settings** (`settings_view.dart`)
   - Subscription status
   - Sync services link
   - Notifications toggle
   - Theme selection
   - Language selection
   - Debug tools (debug mode only)
   - Danger zone (clear database)

2. **Sync Settings** (`sync_settings_view.dart`)
   - Google services (Drive + Calendar)
   - Apple services (iCloud + Calendar)
   - Connection status
   - Auto-sync toggles
   - Manual backup/restore
   - Pro feature locks (free tier)

3. **Theme Settings** (`theme_settings_view.dart`)
   - Light mode
   - Dark mode
   - System mode
   - Preview samples

4. **Language Settings** (`language_settings_view.dart`)
   - English
   - Vietnamese
   - Apply button

---

## State Management

### BLoC Architecture

**Core Principles:**
- Unidirectional data flow
- Immutable state (Freezed)
- Event-driven
- Testable business logic

### Global BLoCs

#### SettingBloc
**Purpose**: App settings and sync management

**State:**
```dart
SettingState {
  ThemeMode themeMode,           // Light/Dark/System
  String locale,                 // 'en', 'vi'
  bool notificationsEnabled,
  GoogleSetting google,          // Drive + Calendar
  AppleSetting apple,            // iCloud + Calendar
  SettingStatus status,
  String? errorMessage
}
```

**Key Events:**
- `load()` - Initialize settings
- `changeTheme(ThemeMode)` - Update theme
- `toggleNotifications(bool)` - Enable/disable notifications
- `connectGoogleDrive()` - Google OAuth
- `manualBackupGoogleDrive()` - Trigger backup
- `restoreFromGoogleDrive()` - Restore data
- `toggleGoogleCalendarAutoSync(bool)` - Calendar sync
- Similar events for iCloud/Apple Calendar

**Error Handling:**
```dart
// Auto-clear error to prevent duplicate popups
void _emitErrorAndClear(Emitter emit, String message) {
  emit(state.copyWith(status: Error(), errorMessage: message));
  Future.delayed(100ms, () {
    if (!isClosed) add(clearError());
  });
}
```

#### SessionBloc
**Purpose**: Authentication management

**States:**
- Initial
- Loading
- Authenticated
- Unauthenticated

#### RevenueCatBloc
**Purpose**: Subscription management

**State:**
```dart
RevenueCatState {
  CustomerInfo? customerInfo,
  bool isPro,
  EntitlementInfo? activeEntitlement
}
```

### Feature BLoCs

#### HomeBloc
- Watch prescriptions stream
- Calculate medication status
- Handle take/skip actions
- Determine hurray display

#### PrescriptionDetailBloc
- Load prescription
- Update medication status
- Export QR
- Delete prescription

#### PrescriptionFormBloc
- Form state management
- Validation
- Medication list management
- Save/update prescription

---

## Cloud Services

### Google Services

#### Authentication
```dart
GoogleAuthService
├── signIn() → GoogleSignInAccount
├── signOut()
├── restoreSession() (on app start)
└── isSignedIn → bool
```

Shared authentication for Drive and Calendar.

#### Google Drive Backup
```dart
GoogleDriveStorageService
├── initialize()
├── uploadBackup(json)
├── downloadBackup() → String?
└── Storage: appDataFolder/dosemate_backup.json
```

**Backup Content:**
- All prescriptions
- Medications
- Schedules
- Settings (exclude auth tokens)

#### Google Calendar Sync
```dart
GoogleCalendarService
├── initialize()
├── syncPrescription(prescription)
├── syncAllPrescriptions(List<Prescription>)
├── removeAllPrescriptionEvents()
└── Event format: "💊 Take [Med] [Dosage]"
```

**Auto-Sync Triggers:**
- Prescription created/updated
- Medication status changed
- Settings toggle enabled

### Apple Services (iOS)

#### iCloud Drive
```dart
ICloudStorageService
├── initialize()
├── uploadBackup(json)
├── downloadBackup() → String?
└── Path: iCloud Documents/backup.json
```

Auto-enabled, no login required.

#### Apple Calendar (EventKit)
```dart
ICloudCalendarService
├── initialize()
├── requestPermissions() → bool
├── hasPermissions() → bool
├── syncPrescription(prescription)
├── removeAllPrescriptionEvents()
└── Permission flow: Request → Settings → Sync
```

---

## User Experience

### Theming

**Theme Modes:**
- **Light**: Clean, bright Material Design 3
- **Dark**: OLED-friendly with reduced brightness
- **System**: Follows device settings

**Theme Persistence:**
Custom Hive adapter saves theme preference:
```dart
// Read/Write order matches for backward compatibility
read: sync_data → theme/locale/notifications
write: sync_data → theme/locale/notifications
```

**Color Schemes:**
- Primary: Material You dynamic colors
- Surface variants for depth
- Semantic colors (error, success, warning)

### Localization

**Supported Languages:**
- English (en)
- Vietnamese (vi)

**Implementation:**
- EasyLocalization package
- JSON translation files
- Locale persistence in SettingsModel
- Hot reload support

**Translation Coverage:**
- All UI text
- Error messages
- Notification content
- Calendar events

### Accessibility
- Semantic labels for screen readers
- High contrast mode support
- Large touch targets (min 44x44)
- Clear visual hierarchy
- WCAG AA compliance

---

## Data Flow

### Prescription Creation
```
User Input → PrescriptionFormBloc
    ↓
Validate fields
    ↓
Save → PrescriptionRepository
    ↓
Write → HiveService (Local DB)
    ↓ (if auto-sync enabled)
Trigger → BackupService
    ↓
Upload → [Google Drive | iCloud]
    ↓ (if calendar sync enabled)
Create Events → [Google Calendar | Apple Calendar]
    ↓
Emit Success → Navigate back
```

### Medication Status Update
```
User Tap (Take/Skip) → HomeBloc
    ↓
Update status with timestamp
    ↓
Save → PrescriptionRepository
    ↓
Write → HiveService
    ↓
Emit Updated State
    ↓
UI Refresh (Timeline updates)
```

### Cloud Sync Flow
```
Data Change → Repository Stream
    ↓
Listen → BackupService
    ↓
Create JSON backup
    ↓
Platform Check:
├─ iOS → ICloudStorageService
└─ Android/iOS → GoogleDriveStorageService
    ↓
Upload to cloud
    ↓
Success/Error → SettingBloc → UI
```

---

## Development

### Code Generation

**Freezed (Models):**
```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

**Auto Route (Navigation):**
```bash
flutter pub run build_runner watch
```

**Hive (Adapters):**
Generated with freezed, registered in `HiveService`.

### Build Commands

**Debug:**
```bash
flutter run --debug
```

**Profile:**
```bash
flutter run --profile
```

**Release:**
```bash
# Android
flutter build apk --release
flutter build appbundle --release

# iOS
flutter build ipa --release
```

### Debug Features

**Debug Mode** (`kDebugMode = true`):
- Debug settings screen visible
- Detailed logging
- State inspection
- Performance overlay

**Production** (`kDebugMode = false`):
- Debug UI hidden
- Minimal logging
- Crashlytics enabled
- Analytics enabled

---

## Monetization

### Free Tier
- Basic prescription management
- Local storage
- Manual entry
- QR import/export
- Local notifications
- **Limitations:**
  - Cloud sync locked
  - Calendar sync locked
  - Banner ads displayed

### Pro Tier (RevenueCat)
- **Ad-free** experience
- **Cloud sync** (Google Drive / iCloud)
- **Calendar integration** (Google / Apple)
- Unlimited prescriptions
- Priority support
- Future premium features

**Pricing** (Example):
- Monthly: $2.99
- Yearly: $19.99 (save 44%)

**Paywall Triggers:**
- Tap "Sync Services" in Settings
- After X prescriptions (free limit)
- Special promotion banners

---

## Performance

### Optimizations
- **Hive**: Fast local storage with lazy loading
- **ListView.builder**: Efficient list rendering
- **Cached images**: Reduce memory usage
- **Debounced API calls**: Prevent rate limiting
- **Token refresh**: Auto-refresh Google tokens

### Security
- **HTTPS only**: All network calls
- **Secure tokens**: Flutter Secure Storage (planned)
- **Local-first**: Data always available offline
- **User consent**: Explicit sync permissions

---

## Roadmap

### Planned Features
- [ ] Medication interaction warnings
- [ ] Refill reminders
- [ ] Prescription photo attachment
- [ ] Doctor appointment tracking
- [ ] Health metrics (blood pressure, glucose)
- [ ] Family member accounts
- [ ] Wear OS / Apple Watch support
- [ ] Web dashboard
- [ ] Medication inventory management

### Technical Improvements
- [ ] End-to-end encryption
- [ ] Real-time multi-device sync
- [ ] Offline queue with conflict resolution
- [ ] Advanced analytics
- [ ] Push notifications (Firebase)
- [ ] Biometric authentication

---

## License

Proprietary - All rights reserved © 2026 DoseMate

