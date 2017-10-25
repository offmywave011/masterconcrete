# Scanbot SDK

## Introduction

The Scanbot SDK brings scanning and document creation capabilities to your mobile apps. It contains modules which are individually licensable as license packages. For more details visit our website https://scanbot.io/sdk.html


## Requirements

Operating system:
- Android 4.1 (API Level 16) and higher

Hardware:
- Rear-facing camera with autofocus
- Supported CPU/ABI architecture: `armeabi-v7`, `arm64-v8a`, `x86`
  (https://developer.android.com/ndk/guides/arch.html)
  The Scanbot SDK contains native libraries for armeabi-v7 and x86. The arm64-v8a devices use our armeabi-v7 library since it is fully compatible to arm64.

Internet connection: 
- Internet access is **not** required, since all SDK operations are performed on device.
- Internet can be used optionally to download OCR language files from a custom server. Alternatively those files can be provided as assets in the app package.



## Getting started

[[Getting started]]
