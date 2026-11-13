# Academic Checker — AI Detection & Similarity Desktop App

**Flutter Desktop Application** for academic integrity checking via **Academi.cx API**

**Author:** Mohammed Lazhari Krobba  
**Tech Stack:** Flutter 3.x, Dart, Material 3, Academi.cx API  
**Platform:** Windows, macOS, Linux (Desktop)

---

## 🎯 Highlights

- **Dual Checker:** Text (125-50,000 words) + Document (PDF upload)
- **AI Detection + Similarity:** Two products via Academi.cx API
- **Real-time Polling:** Async job monitoring with progress updates
- **PDF Highlighting:** Generates AI-highlighted PDFs with similarity matches
- **History Persistence:** Local SQLite storage with full check history
- **Report Export:** Download original similarity PDF + AI-highlighted PDF
- **Clean Architecture:** Repository pattern, dependency injection, separation of concerns

---

## 🚀 Quick Start

```bash
git clone https://github.com/your-username/academic-checker.git
cd academic-checker

# Get dependencies
flutter pub get

# Run on desktop
flutter run -d windows   # or macos, linux

# Build release
flutter build windows --release
flutter build macos --release
flutter build linux --release
```

---

## 🏗 Architecture

### Clean Architecture Layers

```
lib/
├── main.dart                    # App entry point
├── models/                      # Domain models
│   ├── check_history.dart
│   ├── check_result.dart
│   ├── check_options_model.dart
│   └── check_options_model.dart
├── screens/                     # UI screens
│   └── home_page.dart
├── services/                    # Business logic & API
│   ├── api_service.dart         # Academi.cx REST client
│   ├── storage_service.dart     # Local SQLite persistence
│   ├── document_service.dart    # PDF validation & processing
│   └── highlighted_pdf_service.dart  # PDF highlighting engine
├── utils/                       # Helpers
│   ├── app_colors.dart
│   ├── app_constants.dart
│   └── ...
├── widgets/                     # Reusable UI components
│   ├── common/
│   │   └── app_header.dart
│   ├── connection/
│   │   └── api_connection_card.dart
│   ├── checker/
│   │   ├── text_checker.dart
│   │   └── document_checker.dart
│   ├── history/
│   │   └── history_panel.dart
│   └── results/
│       └── results_panel.dart
└── utils/
    └── ...
```

### Key Services

| Service | Responsibility |
|---------|---------------|
| `ApiService` | Academi.cx REST client: auth, create checks, poll results, download reports |
| `StorageService` | SQLite: API key, check history, settings |
| `DocumentService` | PDF validation (size, type, page count), file handling |
| `HighlightedPdfService` | Generates AI-highlighted PDF with similarity matches |

---

## 🎯 Features

### Text Checker
- **Word count validation:** 125-50,000 words
- **AI detection minimum:** 1,000 words (billing minimum)
- **Real-time word count** display
- **Options:** AI Detection, Similarity, or both

### Document Checker
- **PDF upload** via file picker
- **Validation:** size (<50MB), type (PDF), pages (<1000)
- **Progress tracking:** upload → processing → complete
- **Original PDF preservation** for highlighting

### Results Panel
- **Status badge:** completed / processing / failed
- **AI Score:** percentage with confidence
- **Similarity Score:** percentage with source count
- **Download actions:** Original similarity report + AI-highlighted PDF

### History Panel
- **Persistent local storage** (SQLite)
- **Tap to reload** previous results
- **Clear history** option
- **Status indicators:** completed, processing, failed

### API Connection
- **Secure key storage** (encrypted in SQLite)
- **Auto-connect** on app startup
- **Connection test** with live status
- **Disconnect** option

---

## 🔧 Configuration

### Academi.cx API Setup
1. Get API key from [Academi.cx](https://academi.cx)
2. Click "Connect" in app
3. Paste API key → test connection
4. App auto-connects on future launches

### Supported Checks
| Check Type | Products | Min Words | Max Words |
|------------|----------|-----------|-----------|
| Text | AI Detection | 1,000* | 50,000 |
| Text | Similarity | 125 | 50,000 |
| Text | Both | 1,000* | 50,000 |
| Document | AI Detection | 1,000* | ~50,000 |
| Document | Similarity | 125 | ~50,000 |
| Document | Both | 1,000* | ~50,000 |

*AI detection has minimum billable length of 1,000 words

---

## 📦 Dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  get: ^4.6.6                    # State management & DI
  file_picker: ^8.0.0            # File selection
  sqflite: ^2.3.0                # SQLite
  path: ^1.9.0                   # Path utilities
  http: ^1.2.0                   # HTTP client
  pdf: ^3.10.0                   # PDF generation
  printing: ^5.11.0              # PDF printing
  path_provider: ^2.1.0          # File system paths
  shared_preferences: ^2.2.0     # Simple prefs
  intl: ^0.19.0                  # Internationalization
  flutter_svg: ^2.0.0            # SVG support
```

---

## 🧪 Testing

```bash
# Run unit tests
flutter test

# Run integration tests
flutter test integration_test/

# Check code style
flutter analyze
```

---

## 🏗 Build & Release

```bash
# Windows
flutter build windows --release
# Output: build/windows/x64/runner/Release/

# macOS
flutter build macos --release
# Output: build/macos/Build/Products/Release/

# Linux
flutter build linux --release
# Output: build/linux/x64/release/bundle/
```

### CI/CD (GitHub Actions)

```yaml
# .github/workflows/flutter-build.yml
name: Build & Release
on:
  push:
    tags: ['v*']
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
      - run: flutter pub get
      - run: flutter test
      - run: flutter build linux --release
      - run: flutter build windows --release
      - uses: softprops/action-gh-release@v1
        with:
          files: build/**/release/*
```

---

## 📸 Screenshots

| Screen | Description |
|--------|-------------|
| ![Home](screenshots/home.png) | Main dashboard with API connection, text & document checkers |
| ![Text Checker](screenshots/text_checker.png) | Text input with word count, options, real-time validation |
| ![Document Checker](screenshots/document_checker.png) | PDF upload with progress, options, file validation |
| ![Results](screenshots/results_panel.png) | AI score, similarity score, download buttons |
| ![History](screenshots/history_panel.png) | Persistent history with reload, clear |

---

## 🔐 Security

- **API key encrypted** in SQLite (AES-256 via `flutter_secure_storage`)
- **No telemetry** — all data stays local
- **HTTPS only** for API communication
- **Input validation** on all user inputs
- **File type verification** (magic bytes + extension)

---

## 📄 License

MIT License — see LICENSE file.