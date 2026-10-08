# PasswordMaster - Secure Password Manager

A cross-platform Flutter password manager with a strong emphasis on security, supporting **Windows, Android, and iOS**. PasswordMaster follows a **zero-knowledge architecture**, with sensitive data encrypted client-side before storage or backup.

---

## 🔐 Authentication System

### Multiple Authentication Methods

#### 1. Email OTP Authentication
- User accounts via email one-time-password (OTP)
- Registration with email verification
- Login via magic link / OTP
- Account deletion through Firebase Cloud Functions
- Session management

#### 2. Master Password
- Argon2id key derivation
  - 19 MiB memory
  - 2 iterations
  - OWASP-aligned configuration
- Legacy PBKDF2 migration support
- Constant-time password verification
- Rate limiting: **5 failed attempts → 5-minute lockout**

#### 3. Windows Hello Biometric Quick-Unlock
- Native C++/WinRT implementation
- Windows `UserConsentVerifier`
- DPAPI-protected envelope
- Envelope bound to device, Windows account, and installation
- Independent envelope released only after successful biometric verification
- Supports fingerprint, facial recognition, and Windows Hello PIN

#### 4. Mobile Biometrics
- Flutter `local_auth` integration
- Secure-storage-backed quick-unlock key
- Platform-native biometric authentication

---

# 🛡️ Security Features

## Encryption

- **AES-256-GCM** for field-level encryption
- **SQLCipher** for database encryption
- **Argon2id** for master-password key derivation
- **PBKDF2 — 100,000 iterations** for backup encryption
- Authenticated encryption with GCM authentication tags

## Key Management

### Windows
- Windows DPAPI
- Installation-specific entropy
- Vault keys require master password or Windows Hello
- Device/account/installation binding

### Android & iOS
- Android Keystore
- iOS Keychain
- FlutterSecureStorage integration

### Key Separation

Separate cryptographic keys are used for:
- Database encryption
- Field encryption
- Backup encryption
- Windows biometric envelopes

### Memory Protection

- Sensitive key material is zeroized after use where supported
- Windows native implementation uses `SecureZeroMemory`
- Sensitive values are minimized in memory where practical

---

# 🔒 Platform-Specific Windows Security

PasswordMaster provides native Windows security functionality through C++/WinRT and Flutter MethodChannels.

## Windows Hello

- Native Windows Hello integration
- `UserConsentVerifier` API
- Biometric-protected key envelope
- DPAPI protection
- Device and installation binding

## Sensitive Clipboard

- Sensitive values can be copied with clipboard-history exclusion
- Clipboard clearing after **30 seconds**
- Windows cloud clipboard synchronization exclusion where supported
- Native Windows clipboard protection

## DPAPI

Windows-specific secrets are protected using Windows Data Protection API (DPAPI), with installation-specific entropy and device/account binding.

---

# 🚦 Rate Limiting & Account Protection

PasswordMaster includes protection against repeated authentication attempts.

### Local Login Protection

- Failed login attempts are tracked
- **5 failed attempts trigger a 5-minute lockout**
- Login state is persisted securely

### Cloud Rate-Limit State

Rate-limit state can also be synchronized through Firebase Cloud Functions to provide additional protection around account authentication workflows.

---

# 🔑 Password Vault

## Password Entry Management

PasswordMaster provides complete CRUD functionality for password entries.

### Supported Fields

- Title
- Username
- Password
- URL
- Notes

Sensitive fields are encrypted using **AES-256-GCM**.

### Vault Features

- Create password entries
- Read password entries
- Update password entries
- Delete password entries
- PIN / favorite entries
- Search
- Filter
- Category / organization support
- Soft delete
- 30-day automatic cleanup of deleted items
- Creation and modification timestamps

---

# 📋 Secure Clipboard

PasswordMaster provides secure handling of copied credentials.

### Features

- One-tap password copy
- Automatic clipboard clearing
- Configurable timeout behavior
- Windows clipboard-history exclusion
- Windows cloud clipboard exclusion where supported

Default sensitive clipboard clearing timeout:

**30 seconds**

---

# 📄 Secure Document Management

PasswordMaster is more than a password manager — it also provides an encrypted document vault.

## Secure Document Storage

Supported document types include:

- PDF
- Images
- Documents

Documents are encrypted using **AES-256-GCM**.

### Document Metadata

The application tracks:

- File name
- File type
- File size
- Creation date
- Encryption IV
- Authentication tag
- File reference

---

# 🧰 Document Conversion Tools

## 1. Image to PDF

Convert one or multiple images into PDF documents.

### Features

- Custom page sizes:
  - Fit
  - A4
  - US Letter
- Portrait / Landscape orientation
- Margin configuration:
  - None
  - Small
  - Big
- Image rotation
- Image reordering
- Merge multiple images into one PDF
- Create separate PDFs

---

## 2. PDF to Images

Convert PDF pages into image files.

### Supported Formats

- JPG
- PNG

### Features

- Page-by-page extraction
- Individual page conversion

---

## 3. PDF to DOCX

Available on Windows.

### Implementation

- External `converter.exe`
- PowerShell integration
- Microsoft Word COM automation

---

## 4. DOCX to PDF

Available on Windows.

### Implementation

- PowerShell automation
- Microsoft Word integration

---

## 5. PDF Compression

Reduce PDF file size while attempting to preserve document quality.

### Features

- Target-size optimization
- Quality preservation
- Output-size comparison

---

# 📑 PDF Organization & Editing

PasswordMaster includes PDF organization and editing capabilities.

### Features

- Drag-and-drop page reordering
- Page rotation:
  - 90°
  - 180°
  - 270°
- Page duplication
- Page deletion
- Add external PDFs
- Add external images
- Merge multiple files
- Undo / redo
- Zoom controls
- Page preview

---

# 💾 Backup & Recovery

PasswordMaster supports both cloud and local backup.

## Backup Destinations

### 1. Google Drive Cloud Sync

- Google OAuth authentication
- Platform-specific authentication flows
- Automatic `PDManager` folder creation
- Storage quota display
- Backup history
- Cloud backup management

### 2. Local Storage

- Available-drive detection
- Custom backup directory
- Local backup management
- Android internal storage support
- Android SD-card support

---

# 🔐 Encrypted Backup

All backups are encrypted before storage.

### Backup Security

- Custom backup passphrase
- Password-strength validation
- Encrypted vault backup
- Encrypted document backup
- Backup integrity verification
- Cross-device restore
- Master-password authorization for restore

### Backup Passphrase Requirements

A valid backup passphrase requires:

- Minimum 8 characters
- Uppercase letter
- Lowercase letter
- Number
- Special character

---

# 🔄 Automatic Backup

PasswordMaster supports automatic backup workflows.

### Features

- Debounced backup triggers
- Automatic Google Drive synchronization
- Automatic backup pruning
- Configurable backup behavior
- Separate vault and document backups

### Backup Retention

The application keeps the latest **2 backup sets** and automatically prunes older backups.

---

# 📥 Import & Restore

Encrypted backups can be restored on supported devices.

### Restore Process

1. Select encrypted backup
2. Authenticate
3. Provide required backup/master credentials
4. Verify backup integrity
5. Decrypt backup
6. Restore vault and/or documents

### Cross-Device Recovery

Encrypted backups can be used for cross-device restoration when the required authorization and credentials are available.

---

# ⚙️ Application Settings

## User Interface

- Dark mode
- Custom themes
- Gradient-based UI
- Responsive layouts
- Desktop layout
- Mobile layout
- Google Fonts integration

## Vault Settings

Users can configure:

- Master password
- Windows Hello
- Automatic backup
- Google Drive connection
- Security-related settings

## Account Management

- Profile editing
- Google Drive disconnect
- Account deletion
- Logout
- Explicit logout tracking

---

# 📊 Data Models

## PasswordItem

Represents a password-vault entry.

### Fields

- `id`
- `title`
- `username`
- `password`
- `url`
- `notes`
- `createdAt`
- `updatedAt`
- `isPinned`
- `isDeleted`

Sensitive fields are encrypted using AES-256-GCM.

## DocumentItem

Represents an encrypted document.

### Fields

- `id`
- `name`
- `fileType`
- `filePath`
- `sizeBytes`
- `createdAt`
- `ivBase64`
- `tagBase64`

## ImagePdfItem

Represents image/PDF conversion data.

### Fields

- File reference
- Image dimensions
- Rotation angle

## PdfSettings

Controls PDF generation settings.

### Page Size
- Fit
- A4
- Letter

### Orientation
- Portrait
- Landscape

### Margin
- None
- Small
- Big

---

# 💻 Native Windows Platform Channels

PasswordMaster uses Flutter Platform Channels to communicate with native Windows security components.

## `windows_hello.cpp`

| Method | Purpose |
|---|---|
| `availability` | Check Windows Hello availability |
| `enroll` | Create biometric-protected envelope |
| `unlock` | Release protected envelope after biometric verification |
| `invalidate` | Cancel pending biometric operations |

## `user_protection.cpp`

| Method | Purpose |
|---|---|
| `copySensitive` | Copy sensitive data with clipboard protections |
| `clearSensitiveClipboard` | Clear sensitive clipboard contents |
| DPAPI helpers | Encrypt/decrypt protected Windows secrets |

---

# 🏗️ Services Architecture

PasswordMaster is organized around dedicated service classes.

| Service | Purpose |
|---|---|
| `DatabaseHelper` | SQLCipher database, CRUD, migrations |
| `CryptoService` | AES-256-GCM encryption/decryption |
| `KdfService` | Argon2id key derivation |
| `KeystoreService` | Cross-platform secure key storage |
| `WindowsVaultKeys` | Windows-specific vault key management |
| `WindowsHelloService` | Windows Hello MethodChannel |
| `SensitiveClipboard` | Secure clipboard operations |
| `GDriveService` | Google Drive OAuth and file operations |
| `AutoBackupHelper` | Debounced automatic backup triggers |
| `ClerkAuthService` | Email OTP authentication |
| `DocumentConverterService` | PDF/image conversion |
| `RateLimitingService` | Login-attempt tracking |
| `SettingsService` | Application settings persistence |
| `AppSecureStorage` | FlutterSecureStorage wrapper |

---

# ⭐ Key Technical Highlights

## 1. Zero-Knowledge Architecture

Sensitive vault information is encrypted on the client before being stored or synchronized.

The server does not receive plaintext vault passwords or master encryption keys.

## 2. Defense in Depth

PasswordMaster combines:

- Argon2id
- AES-256-GCM
- SQLCipher
- DPAPI
- Android Keystore
- iOS Keychain
- Windows Hello
- Secure clipboard handling
- Rate limiting
- Backup encryption
- Key separation
- Memory cleanup

## 3. Platform-Native Security

Security-sensitive Windows functionality is implemented natively using:

- C++
- C++/WinRT
- Windows Hello
- UserConsentVerifier
- DPAPI

Mobile biometric authentication uses native platform capabilities through `local_auth`.

## 4. Cryptographic Migration

PasswordMaster supports migration from legacy cryptographic implementations.

Examples include:

- PBKDF2 → Argon2id
- Legacy password storage → field-level encryption
- Older vault formats → current encrypted database format

## 5. Memory Protection

Sensitive cryptographic material is cleared from memory where technically possible.

Windows native code uses:

```cpp
SecureZeroMemory()
```

## 6. Installation Binding

Windows biometric key envelopes are protected using device/account/installation-specific security mechanisms.

This helps prevent simply copying application data to another Windows installation and automatically recovering vault keys.

## 7. Recovery Authorization

Sensitive recovery operations can use short-lived authorization tokens.

Default recovery-token lifetime:

**10 minutes**

---

# 📁 Project Structure

```text
PasswordMaster/
│
├── android/
├── ios/
├── windows/
│   └── runner/
│       ├── windows_hello.cpp
│       ├── user_protection.cpp
│       └── ...
│
├── lib/
│   ├── models/
│   │   ├── password_item.dart
│   │   ├── document_item.dart
│   │   ├── image_pdf_item.dart
│   │   └── pdf_settings.dart
│   │
│   ├── services/
│   │   ├── database_helper.dart
│   │   ├── crypto_service.dart
│   │   ├── kdf_service.dart
│   │   ├── keystore_service.dart
│   │   ├── windows_vault_keys.dart
│   │   ├── windows_hello_service.dart
│   │   ├── sensitive_clipboard.dart
│   │   ├── gdrive_service.dart
│   │   ├── auto_backup_helper.dart
│   │   ├── clerk_auth_service.dart
│   │   ├── document_converter_service.dart
│   │   ├── rate_limiting_service.dart
│   │   ├── settings_service.dart
│   │   └── app_secure_storage.dart
│   │
│   ├── screens/
│   │   ├── login/
│   │   ├── dashboard/
│   │   ├── vault/
│   │   ├── documents/
│   │   ├── backup/
│   │   ├── settings/
│   │   ├── pdf/
│   │   └── ...
│   │
│   └── ...
│
├── pubspec.yaml
└── README.md
```

---

# 📱 Supported Platforms

| Platform | Support |
|---|---|
| Windows | ✅ |
| Android | ✅ |
| iOS | ✅ |

### Windows-Specific Features

- Windows Hello
- Native DPAPI
- Native clipboard protection
- PDF/DOCX conversion through Microsoft Word automation

---

# 🛠️ Technology Stack

## Frontend

- Flutter
- Dart
- Responsive UI
- Material-based interface
- Google Fonts

## Security

- AES-256-GCM
- Argon2id
- PBKDF2
- SQLCipher
- Windows DPAPI
- Windows Hello
- Android Keystore
- iOS Keychain
- FlutterSecureStorage

## Authentication & Cloud

- Clerk
- Firebase Cloud Functions
- Google OAuth
- Google Drive API

## Windows Native

- C++
- C++/WinRT
- Windows Runtime APIs
- Microsoft Word COM automation
- PowerShell

---

# 🚀 Getting Started

## Prerequisites

Install:

- Flutter SDK
- Dart SDK
- Android Studio for Android development
- Xcode for iOS development
- Visual Studio with Windows desktop development tools for Windows
- Microsoft Word for Windows DOCX/PDF automation features

## 1. Clone the Repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd PasswordMaster
```

## 2. Install Dependencies

```bash
flutter pub get
```

## 3. Configure Authentication

Configure the required:

- Clerk project
- Firebase project
- Firebase Cloud Functions
- Email authentication
- Email verification
- Account deletion functions

## 4. Configure Google Drive

Create/configure a Google Cloud project and enable the required Google Drive APIs.

Configure OAuth credentials for the supported platforms.

## 5. Run the Application

### Windows

```bash
flutter run -d windows
```

### Android

```bash
flutter run -d android
```

### iOS

```bash
flutter run -d ios
```

---

# 🏗️ Build

## Windows Release

```bash
flutter build windows --release
```

## Android Release

```bash
flutter build apk --release
```

## iOS Release

```bash
flutter build ios --release
```

---

# 🔒 Security Notes

PasswordMaster is designed around client-side protection of sensitive information.

### Important Security Principles

- Sensitive vault data is encrypted before storage
- Master passwords are not transmitted as plaintext
- Encryption keys are separated by purpose
- Biometric data is handled by the operating system
- Windows secrets use native DPAPI protection
- Mobile secrets use platform secure storage
- Backups are encrypted before cloud/local storage
- Sensitive clipboard contents are automatically cleared
- Authentication attempts are rate-limited
- Deleted vault records are automatically cleaned after the configured retention period

> **Important:** No software can guarantee absolute security. The actual security level depends on correct cryptographic implementation, platform security, dependency versions, configuration, and the security of the host device.

---

# 🧪 Security Architecture

```text
                    ┌─────────────────────┐
                    │   User Credentials  │
                    └──────────┬──────────┘
                               │
                     Master Password / OTP
                               │
                               ▼
                    ┌─────────────────────┐
                    │     Argon2id KDF    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Vault Key Layer  │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
        SQLCipher DB     AES-256-GCM       Backup Encryption
              │                │                │
              ▼                ▼                ▼
         Vault Data      Field/Document      Cloud/Local
                           Encryption          Backup
```

---

# 🔐 Windows Hello Security Flow

```text
User
 │
 ▼
Windows Hello
 │
 ▼
UserConsentVerifier
 │
 ├── Fingerprint
 ├── Face
 └── PIN
 │
 ▼
Protected Envelope
 │
 ▼
DPAPI + Installation Binding
 │
 ▼
Vault Key Release
 │
 ▼
Encrypted Vault
```

---

# 📌 Roadmap

Potential future improvements:

- Hardware-backed key attestation
- Passkey authentication
- Security audit logging
- Additional cloud backup providers
- Advanced password health analysis
- Password reuse detection
- Breach/password exposure checking without exposing plaintext passwords
- Secure sharing of selected credentials
- Emergency recovery workflow
- Additional desktop platforms
- Independent third-party security audit

---

# 🤝 Contributing

Contributions are welcome.

Before submitting a pull request:

1. Create a feature branch.
2. Make the required changes.
3. Run tests.
4. Run static analysis.
5. Verify security-sensitive changes carefully.
6. Submit a pull request with a clear description.

For security vulnerabilities, avoid publicly disclosing exploit details before the issue can be responsibly addressed.

---

# 📄 License

Add your project's license information here.

Example:

```text
MIT License
```

---

# 👨‍💻 Project Summary

**PasswordMaster** is a cross-platform secure password and document management application built with Flutter.

It combines:

- Secure password management
- Encrypted document storage
- AES-256-GCM encryption
- SQLCipher database protection
- Argon2id password-based key derivation
- Windows Hello
- Android/iOS biometrics
- Windows DPAPI
- Google Drive encrypted backups
- Local encrypted backups
- PDF conversion and editing
- Secure clipboard handling
- Authentication and rate limiting

The goal is to provide a single application for securely managing passwords, sensitive documents, encrypted backups, and common document-conversion workflows while keeping sensitive data protected on the client device.

---

**Built with Flutter ❤️ for secure cross-platform password and document management.**
