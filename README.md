# Room - Mobile Grooming App

## Project Overview

Room is a comprehensive mobile application for on-demand salon services in South Africa. The platform connects clients with vetted professionals (barbers, stylists, nail technicians) who come to the client's home or office for services.

### Three Integrated Apps

1. **Client App** - Book services, track professionals, manage payments
2. **Staff/Driver App** - Accept jobs, navigate to locations, capture gate codes
3. **Admin Dashboard** - Live dispatch, staff management, bookings overview

## Tech Stack

- **Frontend**: Flutter (Dart)
- **Backend**: Firebase (Authentication, Firestore, Cloud Storage)
- **Maps**: Google Maps SDK
- **State Management**: Provider
- **Payment**: PayStack (for future integration)
- **Hosting**: Firebase/Google Cloud

## Architecture & File Structure

```
room-app-flutter/
├── lib/
│   ├── main.dart (Entry point with state management setup)
│   ├── theme/
│   │   └── colors.dart (Design system)
│   ├── models/
│   │   ├── user_model.dart
│   │   ├── booking_model.dart
│   │   └── job_model.dart
│   ├── services/
│   │   ├── auth_service.dart
│   │   ├── database_service.dart
│   │   └── google_maps_service.dart
│   ├── providers/
│   │   ├── auth_provider.dart (Auth state management)
│   │   ├── booking_provider.dart
│   │   └── location_provider.dart
│   ├── screens/
│   │   ├── auth/
│   │   │   ├── login_screen.dart
│   │   │   └── signup_screen.dart
│   │   ├── client/
│   │   │   ├── home_screen.dart
│   │   │   ├── booking_screen.dart
│   │   │   └── profile_screen.dart
│   │   ├── staff/
│   │   │   ├── job_screen.dart
│   │   │   └── history_screen.dart
│   │   └── admin/
│   │       ├── dispatch_dashboard.dart
│   │       └── stats_screen.dart
│   └── widgets/
│       └── reusable_components.dart
├── android/
│   └── app/src/main/AndroidManifest.xml (Maps API key)
├── ios/
│   └── Runner/AppDelegate.swift (Maps API key)
├── pubspec.yaml
└── README.md
```

## Setup Instructions

### 1. Prerequisites

- Flutter SDK (latest stable)
- Firebase Account
- Google Cloud Console Project
- GitHub Account

### 2. Firebase Configuration

#### Create Firebase Project
1. Go to https://console.firebase.google.com
2. Create new project: `room-app-sa`
3. Enable: Authentication (Phone), Firestore Database (Production Mode), Cloud Storage

#### Initialize Firebase in Flutter
```bash
flutter pub add firebase_core firebase_auth cloud_firestore provider google_maps_flutter
dart pub global activate flutterfire_cli
flutterfire configure
```

### 3. Google Maps Setup

#### Get API Key
1. Go to Google Cloud Console (room-app-sa project)
2. Enable: Maps SDK for Android, Maps SDK for iOS, Places API
3. Create API Key in Credentials

#### Configure Android
Edit `android/app/src/main/AndroidManifest.xml`:
```xml
<application ...>
    <meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="YOUR_API_KEY_HERE"/>
</application>
```

#### Configure iOS
Edit `ios/Runner/AppDelegate.swift`:
```swift
import GoogleMaps

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(...) -> Bool {
    GMSServices.provideAPIKey("YOUR_API_KEY_HERE")
    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

## Key Features Implemented

### 1. Authentication (OTP-based)
- Phone number login
- SMS verification
- Test numbers: +27 123456789 (code: 123456)

### 2. Client Features
- Browse available professionals (with verified badges)
- Book services with gate code
- Live OTP system for job verification
- Track bookings in real-time
- Premium "Room Black" subscription
- Loyalty punch card system

### 3. Staff Features
- View assigned jobs
- OTP verification to start job
- Gate code visibility
- "SOS" panic button for safety
- Inventory alerts (low stock notifications)
- Job completion tracking

### 4. Admin Features
- Live dispatch board
- Real-time job status tracking
- Staff management
- Service configuration
- Analytics dashboard

### 5. Safety & Trust
- Verified professional badges
- Background check indicators
- OTP job verification
- Panic button for emergencies
- In-app safety alerts

### 6. Advanced Features
- Real-time location tracking (Maps)
- Gate code management
- Multi-stop route optimization
- Offline-first mode
- Push notifications (FCM)
- Dark/Light mode toggle

## Database Schema (Firestore)

```
Collections:
├── users/
│   └── {userId}
│       ├── role: string ("client" | "staff" | "admin")
│       ├── name: string
│       ├── email: string
│       ├── phone: string
│       ├── isVerified: boolean
│       ├── subscriptionTier: string ("free" | "black")
│       └── metadata: map
│
├── bookings/
│   └── {bookingId}
│       ├── clientId: string
│       ├── clientName: string
│       ├── staffId: string
│       ├── serviceName: string
│       ├── address: string
│       ├── gateCode: string
│       ├── otpCode: string
│       ├── price: number
│       ├── status: string ("pending"|"confirmed"|"en_route"|"in_progress"|"completed")
│       ├── location: GeoPoint
│       └── timestamp: timestamp
│
├── services/
│   └── {serviceId}
│       ├── name: string
│       ├── price: number
│       └── duration: number
│
├── staff/
│   └── {staffId}
│       ├── name: string
│       ├── phone: string
│       ├── role: string
│       ├── rating: number
│       ├── isVerified: boolean
│       └── photo: string (URL)
│
└── inventory/
    └── {staffId}
        ├── product: string
        ├── quantity: number
        └── threshold: number
```

## Security Rules (Firestore)

See `firestore_rules.txt` for complete security configuration.

**Key Principles:**
- Clients: Read/write only their own data
- Staff: Read assigned jobs, update status
- Admin: Full read/write access

## API Integrations

### Google Maps
- Directions API - Calculate routes and ETA
- Places API - Address autocomplete and search
- Distance Matrix - Optimize multi-stop routes

### PayStack (Future)
- Payment processing
- Card management
- Transaction history

### Firebase Cloud Messaging
- Push notifications
- Job alerts
- Status updates

## Environment Variables

Create `.env` file in project root:
```
FIREBASE_PROJECT_ID=room-app-sa-89857
GOOGLE_MAPS_API_KEY=YOUR_KEY_HERE
PAYSTACK_PUBLIC_KEY=YOUR_KEY_HERE
```

## Getting Started

```bash
# Clone the repository
git clone https://github.com/ngoni-nf/room-app-flutter.git
cd room-app-flutter

# Install dependencies
flutter pub get

# Configure Firebase
flutterfire configure

# Run the app
flutter run
```

## Testing

### Login Credentials (Test Mode)
- **Client**: client@room.co.za (password: any)
- **Staff**: staff@room.co.za (password: any)
- **Admin**: admin@room.co.za (password: any)

### Test Phone Numbers
- +27 123456789 (OTP: 123456)

## Deployment

### Android
```bash
flutter build apk --release
flutter build appbundle --release
```

### iOS
```bash
flutter build ios --release
```

## Important Notes

1. **Billing Setup**: Google Cloud requires billing account for Maps API usage
2. **SHA-1 Certificate**: Required for Android Maps integration
3. **App Store Requirements**: Privacy policy, POPIA compliance (South Africa)
4. **Load Shedding Resilience**: Implement offline-first architecture
5. **Data Costs**: Consider users on limited data connections

## Roadmap

- [ ] Payment integration (PayStack)
- [ ] Push notifications (FCM)
- [ ] Real-time chat support
- [ ] Video call integration
- [ ] Advanced analytics
- [ ] Multi-language support
- [ ] AI chatbot (Gemini integration)

## Support & Contributing

For issues or contributions, please create a GitHub issue or pull request.

## License

MIT License

## Authors

- Development by Ngoni Nyazema
- Design & Architecture guidance
- Firebase & Google Cloud setup

---

**Last Updated**: January 13, 2026
**Project Status**: MVP Ready for Testing
