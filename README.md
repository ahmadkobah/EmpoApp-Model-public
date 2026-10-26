# EmpoApp

EmpoApp is a mobile recruiting application built with Flutter. It connects two kinds of users:
job seekers ("employees"), who search and apply for jobs, and employers, who publish job listings
and review applicants. This repository holds the user-interface prototype of the app.

---

## Table of Contents

1. [Overview](#overview)
2. [Project Status](#project-status)
3. [Repository Layout Note](#repository-layout-note)
4. [How It Works](#how-it-works)
5. [Screens and Components](#screens-and-components)
6. [Technology Stack](#technology-stack)
7. [Project Structure](#project-structure)
8. [Getting Started](#getting-started)
9. [Configuration](#configuration)
10. [Known Issues](#known-issues)
11. [Roadmap](#roadmap)
12. [License](#license)

---

## Overview

### Purpose

Finding a job and finding the right candidate are both slow processes. EmpoApp aims to bring both
sides into one app: a job seeker can browse, filter and apply for jobs and follow the progress of
an application, and an employer can create job listings, see who applied and move a candidate
through review steps.

### Intended users

- **Job seekers (employees)** who want to search for jobs by type and category, save jobs, apply
  with a resume or portfolio and track their applications.
- **Employers** who want to publish job listings, review applications and contact candidates.

A user can switch between the employee side and the employer side inside the app.

### What the app contains today

- Splash screen, onboarding slides and a role selection screen ("I am an Employee" /
  "I am an Employer")
- Login, registration, forgot-password, reset-password and code-verification screens for both roles
- Job seeker area: home feed, search, filters, search results, job details, apply screen,
  confirmation, application tracking, saved jobs, messages, notifications, profile, resume and
  portfolio, settings
- Employer area: home, new job listing with preview, "your listings", applications list,
  applicant review, progress tracker, saved items, messages, notifications, settings
- Job type and job category selection after registration (for example Full-time, Part-time,
  Freelance, Internship; Designer, Marketing, Developer, Administrative, Management, Others)

---

## Project Status

**Prototype / UI only.** The app is a set of screens with navigation between them. It is not
connected to any backend.

- There is no server, database, API client or authentication service in the code. Login and
  registration buttons navigate to the next screen; no credentials are checked or stored.
- Job listings, company names (for example sample "Google" and "Facebook" jobs), profiles and
  messages are static sample content written into the widgets.
- The only external package used in the Dart code is `url_launcher`, used on the employer's
  applicant review screen to start a phone call.
- There are no automated tests.
- The application ID is still the Flutter default `com.example.empoapp`, and release builds are
  signed with the debug key.

---

## Repository Layout Note

The repository history has two commits:

| Commit | Content |
|---|---|
| `Final Upload App` | The Flutter project as an unpacked source tree (`lib/`, `assets/`, `android/`, `ios/`, `linux/`, ...) |
| `Zip App` (current `main`) | The source tree was removed and replaced by a single archive, `empoapp.zip` (about 4 MB) |

On the current `main` branch the source code is therefore only available inside `empoapp.zip`.
The description in this README is based on the unpacked source tree from the `Final Upload App`
commit. The contents of the archive were not checked separately; it is expected to contain the
same project.

---

## How It Works

The app is a single Flutter `MaterialApp`. All screens are Flutter widgets in `lib/`, and the
user moves between them with `Navigator.push` / `Navigator.pushReplacement`.

```
+--------------+    +------------------+    +------------------------+
| SplashScreen | -> | WelcomeScreen    | -> | Onboarding SetA..SetD  |
| (3 seconds)  |    |                  |    | (4 intro slides)       |
+--------------+    +------------------+    +-----------+------------+
                                                        |
                                                        v
                                              +-------------------+
                                              | SplitScreen       |
                                              | choose your role  |
                                              +----+---------+----+
                                                   |         |
                               "I am an Employee"  |         |  "I am an Employer"
                                                   v         v
                       +---------------------------+         +---------------------------+
                       | Login / Register          |         | Login / Register          |
                       | Forgot / Reset / Verify   |         | Forgot / Reset / Verify   |
                       +-------------+-------------+         +-------------+-------------+
                                     |                                     |
                                     v                                     v
                       +---------------------------+         +---------------------------+
                       | HomeScreen (job seeker)   | <-----> | HomeEmployer (employer)   |
                       | search, filter, apply,    | switch  | new listing, your         |
                       | track, saved, messages,   |  role   | listings, applications,   |
                       | profile, resume, settings |         | review, progress tracker  |
                       +---------------------------+         +---------------------------+
```

1. `main.dart` starts `EmpoApp`, a `MaterialApp` with a Material 3 theme, and opens the
   `SplashScreen`.
2. After about three seconds the splash screen replaces itself with the `WelcomeScreen`.
3. The user goes through four onboarding slides (`SetA` to `SetD`) or skips them.
4. On the `SplitScreen` the user chooses the employee or the employer role.
5. Each role has its own login, registration and password-recovery flow. New job seekers choose
   a job type (`JobType`) and job category (`JobKind`) after registration.
6. Job seekers land on `HomeScreen`; employers land on `HomeEmployer`. Both have a bottom
   navigation and a settings screen from which the user can switch to the other role or log out.

---

## Screens and Components

All files are in `lib/`. Files ending in `employer` or `employee` are the variant of a screen
for that role.

### Start and onboarding

| File | Screen |
|---|---|
| `main.dart` | App entry point (`EmpoApp`) |
| `splashscreen.dart` | Animated splash screen with the logo |
| `welcomescreen.dart` | Welcome screen |
| `seta.dart`, `setb.dart`, `setc.dart`, `setd.dart` | Onboarding slides |
| `splitscreen.dart` | Role selection: employee or employer |

### Accounts (both roles)

| File | Screen |
|---|---|
| `loginorregister.dart`, `loginorregisteremployer.dart` | Choice between login and registration |
| `login.dart`, `loginemployer.dart` | Login form |
| `register.dart`, `registeremployer.dart` | Registration form |
| `forgotpassword.dart`, `forgotpasswordemployer.dart` | Request a password reset |
| `resetpassword.dart`, `resetpasswordemployer.dart` | Enter a new password |
| `verify.dart`, `verifyemployee.dart` | Enter a verification code |
| `confirmation.dart`, `confirmationemployee.dart` | Confirmation after verification |
| `jobtype.dart`, `jobkind.dart` | Choose preferred job type and job category |

### Job seeker area

| File | Screen |
|---|---|
| `homescreen.dart` | Home with bottom navigation, job feed and logout dialog |
| `searchoption.dart`, `searchresults.dart`, `jobresults.dart` | Job search and results |
| `filteroption.dart` | Filter options (chips) |
| `googlejob.dart`, `facebookjob.dart`, `application.dart` | Sample job detail screens |
| `applyscreen.dart`, `confirmationscreen.dart` | Apply for a job and confirmation |
| `trackjob.dart` | Track an application |
| `saved.dart` | Saved jobs |
| `messagesscreen.dart`, `notificationsscreen.dart` | Messages and notifications |
| `profilepage.dart`, `resumeportfolio.dart` | Profile, resume and portfolio |
| `settings.dart`, `switchingtomployer.dart` | Settings and switch to the employer role |

### Employer area

| File | Screen |
|---|---|
| `homeemployer.dart` | Employer home with bottom navigation |
| `newlisting.dart`, `previewscreen.dart`, `successful.dart` | Create a job listing, preview it, success screen |
| `yourlisting.dart` | The employer's own listings |
| `applications.dart` | Applications received |
| `employeereview.dart` | Review an applicant, with a "call" button (`url_launcher`) |
| `progresstracker.dart` | Move an applicant through review steps |
| `searchoptionemployer.dart`, `filteroptionemployer.dart` | Search and filters |
| `savedemployer.dart`, `messagesemployer.dart`, `notificationemployer.dart` | Saved items, messages, notifications |
| `settingsemployer.dart`, `switchingtoemployee.dart` | Settings and switch to the employee role |

### Resources

| File / folder | Content |
|---|---|
| `lib/res/assets_res.dart` | Constants with the paths of the image assets |
| `assets/` | About 70 images: logos, icons, onboarding pictures, sample profile photos and company logos |

---

## Technology Stack

| Area | Technology |
|---|---|
| Language | Dart |
| UI framework | Flutter (stable channel), Material 3 widgets |
| Packages used in code | `flutter/material`, `flutter/gestures`, `url_launcher` |
| Font | Poppins (referenced by name in the widgets) |
| Android build | Gradle with Kotlin DSL (`build.gradle.kts`), Java/Kotlin target 11, NDK 27.0.12077973 |
| iOS build | Xcode project `Runner` (Swift `AppDelegate`) |
| Other platforms | A `linux/` folder from the Flutter template |
| Backend | None |

---

## Project Structure

Source tree as found in the `Final Upload App` commit:

```
EmpoApp-Model/
|-- .gitignore
|-- .metadata                 Flutter project metadata
|-- analysis_options.yaml     Dart analyzer / lint settings
|-- android/                  Android host project (Kotlin, Gradle KTS)
|   `-- app/
|       |-- build.gradle.kts
|       `-- src/main/kotlin/com/example/empoapp/MainActivity.kt
|-- ios/                      iOS host project (Xcode, Swift)
|-- linux/                    Linux host project (CMake)
|-- assets/                   Images and icons used by the screens
`-- lib/
    |-- main.dart             Entry point
    |-- res/
    |   `-- assets_res.dart   Asset path constants
    |-- splashscreen.dart
    |-- welcomescreen.dart
    |-- seta.dart ... setd.dart
    |-- splitscreen.dart
    |-- login*.dart, register*.dart, forgotpassword*.dart, resetpassword*.dart, verify*.dart
    |-- homescreen.dart       Job seeker home
    |-- homeemployer.dart     Employer home
    `-- ...                   About 55 screen files in total
```

Current `main` branch:

```
EmpoApp-Model/
|-- empoapp.zip               Archive of the Flutter project
`-- README.md
```

---

## Getting Started

### Prerequisites

- Flutter SDK (stable channel) with Dart
- Android Studio with the Android SDK, or Xcode on macOS for iOS
- An emulator, simulator or a connected device

### 1. Get the source

```bash
git clone https://github.com/ahmadkobah/EmpoApp-Model-public.git
cd EmpoApp-Model-public
```

Then either unpack the archive:

```bash
unzip empoapp.zip
```

or check out the commit with the unpacked source tree:

```bash
git checkout ba488e0bb171a04f58b6b1a3a603aa2e1d4302a5
```

### 2. Add a `pubspec.yaml` if it is missing

The unpacked source tree in the repository has no `pubspec.yaml` (the Flutter project file with
the app name, dependencies and asset list). If the archive does not contain one either, create
it in the project root. It needs at least:

- `name: empoapp`
- the dependencies `flutter` (SDK) and `url_launcher`
- the `assets/` folder under `flutter: assets:`
- optionally the Poppins font under `flutter: fonts:`

### 3. Install dependencies and run

```bash
flutter pub get
flutter run
```

### 4. Build

```bash
flutter build apk      # Android
flutter build ios      # iOS, on macOS
```

---

## Configuration

The app has no environment variables, configuration files or API keys. The following values are
set in code and should be changed before a real release:

| Setting | Where | Current value |
|---|---|---|
| Android application ID / namespace | `android/app/build.gradle.kts` | `com.example.empoapp` |
| Android app label | `android/app/src/main/AndroidManifest.xml` | `empoapp` |
| Release signing | `android/app/build.gradle.kts` | Debug signing config |
| Theme seed color | `lib/main.dart` | `Colors.blue` |
| Phone number for the "call" button | `lib/employeereview.dart` | Hard-coded placeholder |

---

## Known Issues

- **Source only in a zip file on `main`.** The latest commit replaced the source tree with
  `empoapp.zip`, so the code cannot be browsed or reviewed directly on the default branch.
- **`pubspec.yaml` is missing** from the committed source tree. Without it, `flutter pub get` and
  `flutter run` fail.
- **Wrong import path** in `lib/homeemployer.dart`: `import 'settingsemployer.dart.';` has an
  extra dot at the end and will not compile until it is corrected to `'settingsemployer.dart'`.
- **No backend.** Login, registration, verification, applications, messages and listings are
  not saved anywhere and show static sample data.
- **Placeholder screens.** `homescreen.dart` and `homeemployer.dart` contain a
  `PlaceholderScreen` widget for tabs that are not built yet, and `messagesscreen.dart` only has a
  commented-out link to a "new message" screen.
- **Hard-coded phone number** in `lib/employeereview.dart` for the call button; it should come
  from the applicant's data.
- **Inconsistent class names**, for example `loginemployer`, `loginorregister` and
  `Registeremployer` do not follow Dart's UpperCamelCase style, and a file is named
  `switchingtomployer.dart`.
- **No tests.**

---

## Roadmap

These items are not implemented. They are the natural next steps for this prototype:

- Restore the unpacked source tree on `main` and add a complete `pubspec.yaml`
- Connect a backend for authentication, job listings, applications and messages
- Replace sample data with real data models and state management
- Implement the placeholder tabs and the new-message screen
- Set a real application ID and a release signing configuration
- Add widget and integration tests

---

## License

Private. All rights reserved.

Author: Ahmad Kobah
