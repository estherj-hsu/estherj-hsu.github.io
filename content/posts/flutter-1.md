---
title: "Flutter Part 1: Install"
date: 2019-11-19 17:03:38
hidden: false
draft: false
tags: ["flutter","App"]
slug: "Flutter 1"
---

## Install (macOS)

 1. Download Flutter SDK and unzip
 2. Add path to `.zshrc`

```zsh
export PATH="$PATH:[PATH_OF_DIRECTORY]/flutter/bin"
```

 3. Pre-download tools

```bash
flutter precache
```

### esther
Make sure that you unzip the flutter folder into the right directory.
If you'd like to move the flutter folder after ran flutter, you might see the same error as mine. 🙈


<!--more-->

### iOS

 1. Install **xcode**

```bash
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
```

 2. Set up **Simulator**

```bash
open -a Simulator
```

 3. Create a flutter project and test with the device

```bash
flutter run
```

### android

 1. Download and install [Android Studio](https://developer.android.com/studio)
 2. Create a Virtual Device (Android Studio > Tools > AVD Manager)
 3. Run with the device

```bash
flutter run
```

### Errors

After moved flutter folder, you might see the error below:

 > Warning! This package referenced a Flutter repository via the .packages file that is no longer available. The repository from which the 'flutter' tool is currently executing will be used instead.

 1. Update path in `.zshrc`
 2. Add `.packages` into `.gitignore`
 3. Recreate `.packages`

```bash
flutter packages get
```
