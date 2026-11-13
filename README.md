
name: Build WiFi Radar Pro APK

on:
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Download project
        uses: actions/checkout@v4

      - name: Extract project
        run: |
          unzip -o WiFiRadarPro-Android-Project-v2.zip -d .
          test -f settings.gradle.kts
          test -f app/build.gradle.kts

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Set up Gradle
        uses: gradle/actions/setup-gradle@v6
        with:
          gradle-version: '8.9'

      - name: Build APK
        run: gradle :app:assembleDebug

      - name: Publish APK
        uses: softprops/action-gh-release@v2
        with:
          tag_name: wifi-radar-pro-v1.0.0
          name: WiFi Radar Pro 1.0.0
          files: app/build/outputs/apk/debug/app-debug.apk
          overwrite_files: true
