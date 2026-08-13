# Divyesh Suthar

### Full-Stack Mobile Engineer · Founder of TYP · Product Builder

I design, build, secure, instrument, and ship production software end-to-end.

My primary focus is building real products rather than isolated demos — covering the complete lifecycle from product architecture and frontend engineering to backend systems, authentication, payments, analytics, security, deployment, and production operations.

Currently building **TYP**, a relationship intelligence platform engineered from the ground up as a solo developer.

---

## TYP

[![Google Play](https://img.shields.io/badge/Google_Play-LIVE-success?style=for-the-badge\&logo=googleplay\&logoColor=white)](https://play.google.com/store/apps/details?id=com.typ.app)
[![Website](https://img.shields.io/badge/Website-typapp.com-blue?style=for-the-badge\&logo=googlechrome\&logoColor=white)](https://www.typapp.com)

**TYP** is a couple-centric interactive relationship platform built around shared experiences, behavioral signals, relationship intelligence, and longitudinal records.

The product is intentionally positioned as a:

> **Classified emotional archive.**

Rather than a therapy product, generic quiz application, or social network, TYP is designed around the idea that a relationship can be observed through the answers, decisions, interactions, patterns, and signals generated over time.

### Core Product

```text
Shared Activity
      ↓
Answers & Interactions
      ↓
Behavioral Signals
      ↓
Relationship Intelligence
      ↓
Scores / Narratives / Recommendations
      ↓
Longitudinal Relationship Record
```

### Major Features

* Scenarios
* Truth Mode
* Quiz Mode
* Relationship Scores
* Relationship States
* Master Relationship Graph
* Behavioral Signals
* Recommendations
* Badges
* Relationship Arcs
* Narratives
* Cold Cases
* Vault
* Time Capsules
* Past Connections
* Couple Invites
* Real-time Partner Synchronization
* Push Notifications
* Premium Subscriptions
* Activity-level Premium Participation
* Account Management
* Secure App Lock
* PIN and Biometric Protection

---

# Product Architecture

TYP is not a single mobile application isolated from everything else.

The product is built as a complete software ecosystem:

```text
                         TYP
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
     Mobile App       Authentication       Admin
        │                 │                 │
   React Native       Custom Auth         Dashboard
      + Expo           Platform             │
        │                 │                 │
        └─────────────────┼─────────────────┘
                          │
                    Firebase Backend
                          │
       ┌──────────────────┼──────────────────┐
       │                  │                  │
   Firestore             Auth              RTDB
       │                  │                  │
       └──────────────────┼──────────────────┘
                          │
                Intelligence Layer
                          │
             ┌────────────┼────────────┐
             │            │            │
            MRG         Scoring     Analytics
             │            │            │
             └────────────┼────────────┘
                          │
                Revenue & Operations
                          │
          ┌───────────────┼────────────────┐
          │               │                │
      RevenueCat      Email System      Notifications
          │               │                │
          └───────────────┼────────────────┘
                          │
                   Production Stores
                    Google Play / iOS
```

I built and maintain the architecture across these layers rather than treating them as separate products.

---

# Mobile Application

### Technology

* React Native
* Expo SDK 53
* React Native 0.79.x
* TypeScript
* Expo Router
* Zustand-based application state
* Firebase SDKs
* React Native Firebase native services where required

The application is designed for Android and iOS from a shared React Native codebase while maintaining platform-specific native configuration where necessary.

---

# Custom Authentication Platform

TYP uses a dedicated authentication experience rather than relying entirely on a generic hosted authentication UI.

### Authentication infrastructure

```text
auth.typapp.com
        │
        ▼
Custom Authentication Application
        │
        ├── Account Creation
        ├── Email Authentication
        ├── Email Verification
        ├── Password Reset
        ├── Google Authentication
        ├── Apple Sign-In
        └── Authentication Routing
```

The authentication layer is integrated with the main Firebase identity system while providing a dedicated branded authentication experience.

### Authentication capabilities

* Email registration
* Email login
* Email verification
* Password reset
* Google OAuth
* Apple Sign-In
* Redirect handling
* Environment-specific client configuration
* Firebase Authentication integration
* Account lifecycle handling
* Logout and session cleanup

The Google authentication flow is instrumented as its own production performance surface, while email registration, login, and logout are also tracked independently.

---

# Email & Account Automation

The authentication system also includes backend-driven email workflows for account lifecycle events.

```text
User Action
    ↓
Authentication Backend
    ↓
Email Verification / Password Reset
    ↓
Automated Email Delivery
    ↓
User Returns to Auth Flow
```

Automated account communication includes:

* Email verification
* Password reset
* Authentication lifecycle emails
* Verification routing
* Secure account recovery

The verification and reset API paths are part of the authentication backend and are separately observable in the production architecture.

---

# Firebase Backend

Firebase is the primary backend infrastructure powering TYP.

### Firebase services used

* Firebase Authentication
* Cloud Firestore
* Firebase Realtime Database
* Firebase Analytics
* Firebase Crashlytics
* Firebase Performance
* Firebase App Check
* Firebase Remote Config

---

# Cloud Firestore

Firestore acts as the primary application database.

It stores and synchronizes areas such as:

```text
Users
Couples
Relationship Instances
Relationship States
Scenarios
Truth
Quizzes
Scores
Badges
Vault
Time Capsules
Notifications
Premium Metadata
Historical Connections
Graph Data
Behavioral Data
```

The application uses:

* Firestore transactions
* `getDoc`
* `setDoc`
* `updateDoc`
* `onSnapshot`
* Batched writes
* Real-time listeners
* Security Rules

The system contains dedicated instrumentation around important Firestore transactions and application operations rather than treating the database as an invisible dependency.

---

# Firebase Realtime Database

Realtime Database is used where low-latency presence and partner state are more appropriate than standard Firestore listeners.

Examples include:

* Presence
* Partner presence
* Real-time emotional state
* Partner event synchronization

Presence writes and partner listeners are separately instrumented through Firebase Performance.

---

# Relationship Instance Architecture

A major part of TYP's backend architecture is separating:

```text
User Identity
      ≠
Relationship
      ≠
Relationship Instance
      ≠
Historical Relationship
```

This prevents deterministic user-pair identifiers from permanently representing every future relationship between the same two users.

The architecture supports:

* Active relationship instances
* Disconnect lifecycle
* Reconnect lifecycle
* Historical relationship preservation
* Instance-aware state
* Safe relationship transitions

This is important for preventing stale relationship data from leaking into a future relationship lifecycle.

---

# Master Relationship Graph

One of the central technical systems inside TYP is the:

## Master Relationship Graph — MRG

MRG acts as a shared intelligence layer across the product.

Instead of every feature maintaining its own independent interpretation of a relationship, information can flow into a common graph.

```text
Scenarios
Truth
Quizzes
Scores
Badges
Timeline
Narratives
Behavior
Partner Activity
       │
       ▼
Master Relationship Graph
       │
       ├── Relationship Intelligence
       ├── Recommendations
       ├── Score Context
       ├── Badge Intelligence
       ├── Relationship Arcs
       ├── Cold Case Ranking
       ├── Dossier Prioritization
       └── Narrative Context
```

The architecture contains dedicated graph initialization, graph loading, graph building, and scoring traces.

---

# Behavioral Intelligence

The long-term recommendation architecture is designed around behavioral signals rather than static rules.

Instead of simply:

```text
Low Trust
    ↓
Recommend Trust
```

the system can evolve toward:

```text
User A Behavior
        +
User B Behavior
        +
Shared Couple Behavior
        +
Historical Interaction
        +
Recency
        +
Novelty
        ↓
Couple Interest Graph
        ↓
Recommendation Ranking
```

Potential signals include:

* Completion
* Skipping
* Re-opening
* Answer changes
* Time spent
* Response speed
* Partner completion speed
* Revisited content
* Quiz completion
* Prediction behavior
* Confidence
* Shared interests
* Agreement
* Conflict
* Recency
* Historical patterns

The goal is to make the product understand the couple's behavior rather than simply react to one score.

---

# Scoring & Relationship Intelligence

TYP maintains dedicated scoring and intelligence systems for:

* Relationship scores
* Scenario signals
* Truth signals
* Quiz signals
* Behavioral patterns
* Relationship state
* MRG scoring
* Timeline generation
* Narrative generation
* Badge evaluation

Scoring and graph systems are separately instrumented through Firebase Performance, including `SCORE_ENGINE`, `MRG_SCORE`, `TIMELINE_BUILD`, and graph processing traces.

---

# Admin Platform

TYP also has a dedicated administrative application.

```text
typ-admin
     │
     ▼
Admin Dashboard
     │
     ├── Content Management
     ├── Collection Management
     ├── User / Relationship Operations
     ├── Product Operations
     └── Internal Administration
```

The admin system is separate from the consumer mobile application and exists specifically for managing the production product and its backend data.

This allows operational tooling to remain separate from user-facing application logic.

---

# Premium & Payments

TYP uses **RevenueCat** for subscription infrastructure.

RevenueCat handles the platform subscription layer while Firebase remains responsible for application-side state and product data.

```text
Google Play / App Store
          │
          ▼
      RevenueCat
          │
          ├── Customer
          ├── Purchase
          ├── Entitlement
          ├── Restore
          └── Subscription Lifecycle
                  │
                  ▼
             TYP App State
```

### Premium infrastructure includes

* RevenueCat configuration
* Customer identification
* Entitlement checks
* Purchase flow
* Restore purchases
* Subscription lifecycle
* Premium state synchronization
* Platform-specific configuration
* Paywall infrastructure

RevenueCat initialization, customer information, purchases, and restore flows are separately traced in production.

---

# Premium Security Model

A major architectural distinction in TYP is:

```text
Premium Ownership
        ≠
Participation Access
```

A premium user does not automatically transfer their subscription entitlement to their partner.

Instead, specific shared activities can become playable for a free partner when the premium partner has already started that activity.

```text
Premium User
     │
     ▼
Starts Premium Activity
     │
     ▼
Existing Shared Activity State
     │
     ▼
Free Partner Can Participate
```

But:

```text
Free Partner
     X
Global Premium
     X
RevenueCat Entitlement
     X
All Premium Content
     X
Premium Categories
```

This keeps monetization ownership individual while still allowing shared couple experiences.

---

# Security

Security is implemented across multiple layers rather than relying on a single permission check.

### Application security

* Firebase Authentication
* Firestore Security Rules
* Firebase App Check
* Relationship-level authorization
* Activity-level authorization
* Premium entitlement boundaries
* Relationship instance isolation
* Secure account lifecycle handling

### Platform security

Android production builds also integrate Google Play integrity/security mechanisms where applicable.

The architecture is designed around:

```text
Authenticated User
        ↓
Authorized Relationship
        ↓
Authorized Activity
        ↓
Authorized Data
```

rather than simply:

```text
Logged In = Access Everything
```

---

# Firebase Analytics

Analytics is not an afterthought in TYP.

A custom analytics layer was built around Firebase Analytics to provide structured product telemetry.

The system includes:

* Custom event definitions
* Screen view tracking
* User identity binding
* User reset handling
* Event parameter controls
* PII filtering
* Parameter limits
* String truncation
* Feature-level analytics

The current analytics registry contains **40 declared event names**, with calls distributed across major product surfaces including authentication, scenarios, truth, quizzes, upgrades, badges, vault, time capsules, notifications, and account operations.

### PII protection

The analytics layer explicitly filters sensitive fields such as:

```text
email
name
text
answer
message
```

before sending event parameters.

This allows product behavior to be measured without turning analytics into a storage mechanism for user-generated private content.

---

# Firebase Crashlytics

Crashlytics is integrated as a production error-reporting layer.

It captures:

* Non-fatal errors
* React render errors
* Background async failures
* User identity
* Relationship context
* Premium state
* Current route

Crashlytics attributes include:

```text
userId
coupleId
activeInstanceId
premium
route
```

The application also routes errors from its `ErrorBoundary` and controlled background async system into Crashlytics.

---

# Firebase Performance

TYP contains a dedicated performance instrumentation layer rather than relying only on generic application profiling.

The system measures areas including:

```text
Boot
Authentication
App Lock
Splash
Firestore
Couples
Premium
RevenueCat
Push Notifications
Presence
Scenarios
Truth
Quizzes
Vault
Time Capsules
Badges
Narratives
Scoring
MRG
Timeline
Navigation
```

The current observability inventory contains approximately **94 unique performance trace names** across the application.

The performance layer also automatically attaches controlled diagnostic attributes such as:

```text
app_version
platform
build_number
release_channel
runtime_version
```

to production traces.

---

# Production Observability

One of the things I care about most is knowing what the application is doing after it ships.

TYP therefore uses:

```text
Firebase Analytics
        +
Firebase Crashlytics
        +
Firebase Performance
        +
Application Diagnostics
        +
Behavior Logging
```

to create a production observability layer.

The system contains dedicated traces for boot, authentication, lock initialization, RevenueCat, push notifications, relationship synchronization, graph processing, scoring, content operations, and other critical flows.

---

# Real-Time Synchronization

TYP is heavily dependent on real-time state.

The architecture uses Firestore listeners and Realtime Database where appropriate for:

* Partner answers
* Scenario completion
* Truth state
* Quiz state
* Relationship state
* Scores
* Badges
* Presence
* Partner events
* Disconnect state

The application also uses local caches and state hydration to prevent temporary listener gaps from producing visible UI regressions.

---

# App Lock & Privacy

TYP includes a dedicated application lock system.

Supported mechanisms include:

* PIN protection
* Biometric authentication
* SecureStore-backed state
* Lock lifecycle management
* Foreground/background evaluation
* Session-aware initialization

The lock system has been extensively instrumented to distinguish:

```text
Auth Restore
    ↓
Lock Initialization
    ↓
SecureStore
    ↓
Lock Evaluation
    ↓
Lock Screen Render
```

rather than treating the entire startup delay as one generic "loading" operation.

---

# Push Notifications

The notification system is integrated with:

* Expo push infrastructure
* Push permissions
* Token acquisition
* Token persistence
* Partner activity notifications
* Deep-link routing
* Notification receipts

Push registration, permission handling, token fetching, and token upload are separately measured through Firebase Performance.

---

# Content Management

The TYP ecosystem includes a production content-management workflow through the admin platform.

This allows the application to keep content and operational data separate from the mobile application's presentation layer.

The architecture supports content around:

```text
Scenarios
Truth
Quizzes
Badges
Relationship Intelligence
Narratives
```

while the mobile application consumes the resulting production data.

---

# Deployment Pipeline

I maintain the complete release pipeline rather than handing deployment off as a separate responsibility.

### Android

```text
Source
  ↓
Expo / React Native
  ↓
EAS Build
  ↓
Android Production Build
  ↓
Google Play Console
  ↓
Production
```

### iOS

```text
Source
  ↓
Expo / React Native
  ↓
Native iOS Configuration
  ↓
Xcode
  ↓
Provisioning / Signing
  ↓
App Store Connect
```

The iOS pipeline includes Apple Sign-In, RevenueCat platform configuration, native iOS configuration, and platform-specific compatibility work.

---

# Development Infrastructure

The project is maintained across multiple production surfaces:

```text
typ
│
├── Mobile Application
│
├── typ-auth
│   └── Custom Authentication Platform
│
├── typ-admin
│   └── Administrative Dashboard
│
└── site-typ
    └── Public Product Website
```

Each surface has a specific responsibility rather than putting the entire product into one codebase.

---

# Engineering Philosophy

I prefer:

```text
Evidence
   ↓
Isolation
   ↓
Verification
   ↓
Minimal Change
   ↓
Regression Testing
   ↓
Production Approval
```

over:

```text
Guess
   ↓
Rewrite
   ↓
Hope
```

For production issues, I use forensic audits, source-level verification, telemetry, and controlled fixes.

The TYP project has gone through dedicated audits for:

* Authentication
* Firestore permissions
* Relationship lifecycle
* Premium access
* Startup performance
* App Lock
* Real-time synchronization
* MRG
* Scoring
* Observability
* Crash handling
* Production reliability

The goal is not to make the code look sophisticated.

The goal is to make the system behave predictably in production.

---

# Tech Stack

### Mobile

`React Native` · `Expo` · `Expo Router` · `TypeScript` · `JavaScript`

### Frontend

`React` · `React Native` · `Expo` · `HTML` · `CSS` · `Next.js` · `Tailwind CSS`

### Backend

`Firebase Authentication` · `Cloud Firestore` · `Firebase Realtime Database` · `REST APIs`

### Authentication

`Custom Auth Platform` · `Firebase Auth` · `Google OAuth` · `Apple Sign-In` · `Email Verification` · `Password Reset`

### Intelligence

`Master Relationship Graph` · `Scoring Engine` · `Narrative Engine` · `Timeline Engine` · `Badge Engine` · `Behavioral Signals`

### Payments

`RevenueCat` · `Google Play Billing` · `Apple In-App Purchases` · `Entitlements` · `Subscription Lifecycle`

### Analytics & Observability

`Firebase Analytics` · `Firebase Crashlytics` · `Firebase Performance` · `Custom Analytics` · `Behavior Logging`

### Security

`Firestore Security Rules` · `Firebase App Check` · `Play Integrity` · `SecureStore` · `PIN / Biometrics`

### Infrastructure

`Git` · `GitHub` · `EAS Build` · `Xcode` · `Google Play Console` · `App Store Connect` · `Vercel` · `Cloudflare`

---

# Selected Engineering Areas

```text
Mobile Architecture
Real-Time Systems
Firebase Architecture
Authentication Systems
Subscription Infrastructure
Security Engineering
Production Observability
Behavioral Intelligence
Recommendation Systems
Performance Engineering
Cross-Platform Deployment
Admin Tooling
Product Infrastructure
```

---

# Building as a Solo Developer

TYP is not the result of only writing the mobile UI.

As a solo developer, I have worked across the complete product stack:

```text
Product Idea
    ↓
Product Design
    ↓
Mobile Development
    ↓
Backend Architecture
    ↓
Database Design
    ↓
Authentication
    ↓
Security
    ↓
Payments
    ↓
Analytics
    ↓
Crash Monitoring
    ↓
Performance Monitoring
    ↓
Admin Infrastructure
    ↓
Email Automation
    ↓
Notifications
    ↓
Android Deployment
    ↓
iOS Engineering
    ↓
Production Operations
```

That full-stack ownership is what I enjoy most.

---

---

### GitHub Activity

<p align="center">
  <img
    src="https://github-readme-activity-graph.vercel.app/graph?username=Divy1726&theme=tokyo-night&hide_border=true"
    alt="GitHub Activity Graph"
    width="100%"
  />
</p>

# Connect

**TYP:** [typapp.com](https://www.typapp.com)

**Android:** [TYP on Google Play](https://play.google.com/store/apps/details?id=com.typ.app)

**GitHub:** [github.com/Divy1726](https://github.com/Divy1726)

**Email:** [suthardivyesh17@gmail.com](mailto:suthardivyesh17@gmail.com)

---

<p align="center">

### Building products from idea to production.

**TYP**

</p>
