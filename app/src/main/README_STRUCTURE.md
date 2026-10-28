# Project Architecture & Directory Guide

This document maps out the architecture of the codebase so you know exactly where to write UI design code, JNI bindings, and native C++ logic.

---

## 📱 1. Java Layer: UI Design & Android Services
**Location:** `app/src/main/java/com/android/support/`

* **`Menu.java`** — **UI Design & Material 3 Layouts**
  * Contains all floating window design code, view hierarchies, layout parameters, and Material 3 controls (`MaterialSwitch`, `MaterialButton`, `MaterialCheckBox`, `MaterialRadioButton`, `MaterialTextView`).
  * Modify this file if you want to change the visual styling, colors, dimensions, or add new Material 3 UI widgets.

* **`Launcher.java`** — **Foreground Service & Overlay Manager**
  * Manages the `WindowManager` system overlay lifecycle.
  * Controls when the menu is shown, minimized, or hidden when switching tasks.

* **`MainActivity.java`** — **Application Entry Point**
  * Initial activity launched when opening the app.
  * Handles permission prompts (e.g., overlay permissions) and launches the main app context.

* **`Preferences.java`** — **Settings & Data Persistence**
  * Handles saving and loading toggle states, slider values, text entries, and settings to `SharedPreferences`.

* **`Main.java`** — **Native Library Loader**
  * Loads the compiled native C++ library (`System.loadLibrary("MyLibName")`).

---

## ⚙️ 2. Native Layer: C++ & JNI Interfaces
**Location:** `app/src/main/jni/`

* **`Main.cpp`** — **Native Feature Definitions & Callbacks**
  * Defines the menu feature list returned to Java via `GetFeatureList()`.
  * Contains feature change handling callbacks (`Changes()`) triggered when toggles or sliders are interacted with in the UI.

* **`Menu/`** — **JNI Bridge**
  * `Jni.cpp` & `Menu.cpp`: Connects C++ native callbacks to Java's `Menu.java`.

* **`Includes/`** — **Native Utilities & Macros**
  * `Logger.h`: Android Logcat wrapper (`LOGD`, `LOGE`).
  * `obfuscate.h`: Compile-time string obfuscation helper.
  * `Macros.h` & `Utils.hpp`: Miscellaneous native helper functions.

---

## 🛠️ 3. NDK Build Configuration
**Location:** `app/src/main/jni/`

* **`Android.mk`** — Defines source file lists (`LOCAL_SRC_FILES`), include directories, compiler flags (`-std=c++17`), and library names (`MyLibName`).
* **`Application.mk`** — Configures target ABIs (`armeabi-v7a`, `arm64-v8a`) and C++ STL standard runtime.
