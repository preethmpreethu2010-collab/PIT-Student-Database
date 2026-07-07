# APK BUILD GUIDE

## Prerequisites 📋

Before building the APK, ensure you have:

1. **Python 3.9+** installed
2. **Java Development Kit (JDK)** - Version 11 or higher
3. **Android SDK** - For Android development tools
4. **Android NDK** - For native compilation

### Installation Steps

#### On Windows:
```bash
# Install buildozer and dependencies
pip install buildozer cython

# Install Java (if not already installed)
# Download from: https://www.oracle.com/java/technologies/downloads/

# Download Android SDK from: https://developer.android.com/studio
# Or use Android Command Line Tools
```

#### On macOS:
```bash
# Install buildozer using Homebrew
brew install buildozer

# Install Java
brew install openjdk@11

# Set JAVA_HOME
export JAVA_HOME=/usr/libexec/java_home -v 11
```

#### On Linux (Ubuntu/Debian):
```bash
# Install dependencies
sudo apt-get update
sudo apt-get install -y \
    python3-dev \
    openjdk-11-jdk \
    android-sdk \
    build-essential \
    git \
    cython3

# Install buildozer
pip install buildozer cython
```

## Configuration 🔧

The `buildozer.spec` file contains all build configurations:

```spec
[app]
title = PIT Student Database
package.name = pitstudentdb
package.domain = org.pit
```

### Key Settings:

- **title**: App name displayed on device
- **package.name**: Unique identifier (lowercase, no spaces)
- **package.domain**: Reverse domain notation
- **android.api**: Target Android API level (31 = Android 12)
- **android.minapi**: Minimum supported API level (21 = Android 5.0)
- **requirements**: Python packages to include (python3, kivy, pillow)
- **android.permissions**: Requested permissions

## Building the APK 🚀

### Method 1: Using the Build Script
```bash
python build_apk.py
```

### Method 2: Direct Buildozer Command
```bash
# Build debug APK (faster, for testing)
buildozer android debug

# Build release APK (for production, requires signing)
buildozer android release
```

### Build Process Timeline:

1. **Download requirements** (~2-5 minutes)
   - Android SDK
   - Android NDK
   - Python for Android
   
2. **Compile app** (~5-15 minutes)
   - Python files compilation
   - Kivy framework setup
   - Dependencies integration

3. **Package APK** (~2-3 minutes)
   - Create APK file
   - Sign with debug key
   - Optimize size

**Total time: 15-30 minutes** (varies by internet speed and machine)

## Output 📦

After successful build:

```
bin/
└── StudentDatabase-debug.apk    # Ready to install
```

## Installing on Device 📱

### Method 1: Using ADB (Android Debug Bridge)
```bash
# Connect device via USB and enable USB debugging
adb install bin/StudentDatabase-debug.apk

# Or reinstall (uninstall first)
adb install -r bin/StudentDatabase-debug.apk
```

### Method 2: Manual Installation
1. Transfer APK to your Android device
2. Open File Manager
3. Tap the APK file
4. Follow the installation prompts

### Method 3: Android Studio Emulator
1. Start Android emulator
2. Run: `adb install bin/StudentDatabase-debug.apk`

## Troubleshooting 🔍

### Issue: "Buildozer command not found"
```bash
# Solution: Install buildozer
pip install buildozer
```

### Issue: "Java not found"
```bash
# Add JAVA_HOME to environment variables
# Windows: Set in System Properties → Environment Variables
# Linux/Mac: export JAVA_HOME=/path/to/java
java -version  # Verify installation
```

### Issue: "Android SDK not found"
```bash
# Set Android SDK path in buildozer.spec
# Or set environment variable:
export ANDROID_SDK_ROOT=/path/to/android/sdk
```

### Issue: APK too large
- Remove unused assets
- Optimize image files
- Use app bundles instead

### Issue: Build fails at "cython"
```bash
pip install --upgrade cython
pip install --upgrade setuptools
```

### Issue: Permission errors on Linux
```bash
# Run with proper permissions or adjust directory ownership
sudo chown -R $USER ~/.buildozer
```

## Release Build (Production) 📲

For publishing on Google Play Store:

```bash
# Build release APK
buildozer android release

# Sign the APK
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 \
    -keystore my-release-key.keystore \
    bin/StudentDatabase-release-unsigned.apk \
    alias_name

# Align APK
zipalign -v 4 bin/StudentDatabase-release-unsigned.apk \
    bin/StudentDatabase-release.apk
```

## App Store Submission 🎯

To publish on Google Play:
1. Create Google Play Developer Account
2. Set up app listing with screenshots, descriptions
3. Upload signed release APK
4. Complete store listing details
5. Submit for review

## Environment Variables 🖥️

Important variables for build process:

```bash
# Java
export JAVA_HOME=/path/to/jdk

# Android SDK
export ANDROID_SDK_ROOT=/path/to/android-sdk

# Android NDK
export ANDROID_NDK_ROOT=/path/to/android-ndk

# Python
export PATH=/path/to/python:$PATH
```

## Advanced Configuration ⚙️

### Custom App Icon
1. Create 512x512 PNG image
2. Place in project directory: `data/icon.png`
3. Uncomment in `buildozer.spec`:
```spec
android.icon = data/icon.png
```

### Permissions Modification
```spec
android.permissions = INTERNET, WRITE_EXTERNAL_STORAGE, READ_EXTERNAL_STORAGE, CAMERA
```

### Features
```spec
android.features = android.hardware.camera
```

## Resources 📚

- [Buildozer Documentation](https://buildozer.readthedocs.io/)
- [Kivy for Android](https://kivy.org/doc/stable/guide/android.html)
- [Android Developer Guide](https://developer.android.com/guide)
- [Python for Android](https://github.com/kivy/python-for-android)

## Support 💬

For issues:
1. Check Buildozer logs: `.buildozer/android/platform/build/build.log`
2. Search GitHub issues
3. Ask on Kivy community forums

---

**Good luck building! 🎉**
