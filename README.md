# PilferShush Jammer

Research and project page : <https://www.cityfreqs.com.au/pilfer.php>

Android microphone checker and jamming application built for AOSP LineageOS.  

Application for low battery requirement microphone passive jamming.

Calls audioRecord.startRecording() but DOES NOT READ THE AUDIO BUFFER.

Holds microphone access and should block user apps from gaining focus of microphone.

System telephone calls will override and bump the Jammer from the microphone. 

Adds a notification as a reminder for running while in background.

Tested and blocks Google Voice search (user) app.
Currently testing Chrome/Omnibox/Assistant app voice blocking.

Active jammer - tone and white noise versions, boost EQ for higher amplitude.

Scan user installed apps for key features, possible NUHF/ACR SDK package name matches and services/receivers running.

Jammers run as a foreground service

Note: On devices running Android 9 (API level 28) or higher, apps running in the background cannot access the microphone. 
Therefore, your app should record audio only when it's in the foreground or 
**when you include an instance of MediaRecorder in a foreground service.**

Note: Android 10 has new concurrent audio capture policy that means other recording apps can bump a prior recording audio app from the microphone.
see <https://source.android.com/compatibility/android-cdd#5_4_5_concurrent_capture>

Note: Android 11 (API 30) changes to foreground services access to microphone - "while-in-use access" only.
see <https://developer.android.com/about/versions/11/privacy/foreground-services>

Note: APK signing with API30 needs v2/v3 - not v1

Note: currently only testing on ARMv7 with single mic,
need to get aarch64/arm64 device with multiple mics.

Note: Microphone source switch setting from VOICE_COMM to DEFAULT will cause
passive jammer to lose concurrent audio focus and stop blocking the microphone

Note: Voice Assistant always wins (<30), testing BIND_VOICE_INTERACTION (api21/23) permission  
and, possibly RoleManager.ROLE_ASSISTANT, with necessary services code
see <https://github.com/commonsguy/cw-omnibus/tree/master/Assist/TapOffNow>
and android.googlesource.com/platform/frameworks/base/marshmallow-release/./tests/VoiceInteraction/

Note: perennial source of a few crashes : <https://stackoverflow.com/questions/48804456/how-to-fix-android-app-remoteserviceexception-without-any-message-not-related>

**TODO:**
- Android 12 (S, API31) has a "mute microphone" settings switch - determine its scope
- Assistant jammer requires min API 23 (M) for supportsAssist in xml
- background autostart activity for concurrent audio fight via SYSTEM_ALERT_WINDOW permission
- vs AccessibilityService misuse, in Android 10 (does not work in Go version)
- see : <https://developer.android.com/guide/components/activities/background-starts>
- Android 11 changes to concurrent audio
- inconsistent behaviour with widget and jammer service state
- Android 8.x service crashes specifically to its API
- test android.permission.BIND_ACCESSIBILITY_SERVICE (API >= 16 (4.1)))
- get an arm64 multi mic device


**Build:**
- min API 21 (5.0)
- target API 30 (11.x)
- compiled API 30 (11.x)
- Android Studio 4.1.2 stable
- Gradle 6.5


**Changes:**
- 4.6.0 (released February 24, 2021)
- min API bump to 21 (5.0, Lollipop)
- github branch archive2 prior to min api 21 bump
- Android 11 add foregroundServiceType
- add <queries> intent for browser package access
- prototyping Assistant/Omnibox jammer
- add excessive retrigger warning
- catch MediaRecorder placebo exception in some devices
- update Android Studio
- add SDK name


   testing devices
   - EMU : Galaxy Nexus 4.3 (18) (Android Studio AVD, no GApps)
   - EMU : Nexus 4 5.1 (22) (Android Studio AVD, no GApps)
   - EMU : Nexus 5X 7.0 (24) (Android Studio AVD, GApps)
   - EMU : Galaxy Nexus Oreo (27) (Android Studio AVD, GApps)
   - EMU : Pixel 3a 10.0 (29) (Android Studio AVD, GApps)
   - LOW : s4 I9195 (antique) 7.1.2 (25)( /e/ 0.13, Cleanapk)
   - SLO : Mts 5045D (tainted) 7.1.2 (25) (LineageOS 14.1, GApps)
   - MID : Galaxy Tab 2 GT-P5110 (minimal) 7.1.2 (25) (LineageOS 14.1, F-Droid)
   - DEV : s5 G900I (tainted) 10.0 (29)(LineageOS 17.1, GApps)
   - DEV : s5 G900P (useful) 7.1.2 (25) (LineageOS 14.1, F-Droid)
   - PROD: s10 SM-G977B (nominal) 11.0 (30) (LineageOS 18.0, F-Droid)
 
 
**App screenshots:**
- Home fragment
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-v4_home.jpg" height="612px" />

- Inspector fragment
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-v4_inspector.jpg" height="612px" />

- App Entry Info dialog
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-v4_app_entry.jpg" height="612px" />

- Settings fragment
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-v4_settings.jpg" height="612px" />
 
**Active Jammer frequency analysis:**
- Active tone, full NUHF range with random scatter drift test
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-tone_full_nuhf_scatter_drift-test.jpg" height="182px" />
 
- Active tone, slow speed, limited drift test
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-carrier_limit_drift_speed-test.jpg" height="168px" />

- Active tone, carrier and drift limited, fast
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-carrier_limit_conform-test.jpg" height="164px" />

- Active jammer (19 kHz carrier, 1000 Hz limit, EQ on) versus ramp-up audio beacon-like signal : scatter jamming demo
<img src="https://github.com/kaputnikGo/PilferShushJammer/blob/master/images/PS_Jammer-vs-html5_synth.jpg" height="138px" />

# 2021 Kaputnik Go


Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   <http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
