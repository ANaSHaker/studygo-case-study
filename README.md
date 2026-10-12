<p align="center"><img src="assets/banner.png" alt="StudyGo" width="100%"></p>
<h1 align="center">StudyGo (ستادي جو)</h1>
<p align="center"><b>One education app for students, teachers and parents in Palestine: find a teacher, book and pay for lessons, sit exams, and learn with an AI assistant.</b></p>
<p align="center">
  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20iOS%20%7C%20iPad-2E6BD8?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/Flutter-3.x-02569B?style=flat-square&logo=flutter&logoColor=white" alt="Flutter">
  <img src="https://img.shields.io/badge/Architecture-Clean-F59E0B?style=flat-square" alt="Clean Architecture">
  <img src="https://img.shields.io/badge/State-Bloc%20%2B%20GetIt-8A2BE2?style=flat-square" alt="Bloc + GetIt">
  <img src="https://img.shields.io/badge/Routing-GoRouter-0F766E?style=flat-square" alt="GoRouter">
  <img src="https://img.shields.io/badge/Firebase-FCM%20%7C%20Crashlytics%20%7C%20Remote%20Config-FFCA28?style=flat-square&logo=firebase&logoColor=black" alt="Firebase">
  <a href="https://play.google.com/store/apps/details?id=com.study.go"><img src="https://img.shields.io/badge/Google%20Play-Live-34A853?style=flat-square&logo=googleplay&logoColor=white" alt="Google Play"></a>
  <a href="https://apps.apple.com/sa/app/study-go/id6777997182"><img src="https://img.shields.io/badge/App%20Store-Live-0D96F6?style=flat-square&logo=appstore&logoColor=white" alt="App Store"></a>
  <img src="https://img.shields.io/badge/Case%20Study-EdTech-111827?style=flat-square" alt="Case Study">
</p>

> ℹ️ This is a case study. The source code belongs to the client and is not included.

## Overview

**StudyGo** connects students in Jerusalem and across Palestine with specialised teachers for the **Tawjihi** and **Bagrut** (Arabic) curricula, including the 3–5 unit tracks. It is one app with three roles:

- **Students** find teachers, book private or group lessons, buy study programs, sit exams and use an AI study assistant.
- **Teachers** go through an activation wizard. Then they manage subjects, sessions, files, exams and grading, and see their earnings, wallet and subscription.
- **Parents** link their children, follow sessions and exam results, and get AI help to read reports and write to teachers.

**My role was the full Flutter app.** That covered the architecture, every screen for all three roles, payments, notifications, the AI features and the store releases for Android, iPhone and iPad. The backend and its REST/OpenAPI contract were built by the client's backend team. I integrated against it.

The project started in April 2026, and the App Store release came in July 2026. The current version at the time of writing is 1.0.3.

## The challenge

- **Three products in one app.** Students, teachers and parents have different home screens, tabs and permissions. A teacher can also be pending review, and users can switch roles.
- **Real money, real compliance.** Lessons, plans, wallet top-ups and program purchases are paid by card through a hosted payment page. 3-D Secure and Google Pay don't run inside a WebView, and money must never be a floating-point number.
- **Timed exams on untrusted clocks.** A student's phone clock can be wrong. Exam countdowns have to follow the server.
- **Store rules.** There is no login wall (Apple 5.1.1(v)), Sign in with Apple must sit next to Google, in-app account deletion is required, and guests must be able to browse.

## Highlights

**🎓 Students**
- Browse and filter teachers by stage (primary, middle, secondary). Profiles show prices, experience, location and reviews.
- Book private sessions or join group sessions. Online sessions carry a per-session meeting link.
- A store of study programs (playlists) with purchases, a "my purchases" section, and a timeline of sessions.
- Exams: sit timed papers, review attempts and see published results.

**👩‍🏫 Teachers**
- An activation wizard with a progress card. The app has pending, awaiting-review, approved and rejected states.
- Subjects, sessions, files (PDF and video viewers), exams and a **grading queue**. Objective questions are scored automatically. Results stay hidden until the teacher publishes them.
- Earnings, bank account and payout day, a recurring subscription panel, and a **wallet** for cash-commission top-ups.

**👨‍👩‍👧 Parents**
- Link children and see each child's sessions and progress, with AI help to interpret reports and draft messages to teachers.

**🤖 AI and content**
- **27 AI tools** across the three roles:
  - photo question solver and step-by-step solver
  - summaries and flashcards
  - study plans and a Tawjihi review
  - essay grading and a writing trainer
  - exam, quiz and question-bank generators
  - lesson plans
  - report interpreter and parent/teacher messages
  - and more
- An educational **YouTube stories** row. Stream URLs are resolved natively, so playback needs no WebView.

**🔔 Platform**
- FCM push plus a REST notification feed with deep-link routing. Device tokens are registered on login and removed on logout.
- Guest mode with contextual sign-up sheets, Google and Apple native sign-in, and in-app account deletion.
- Remote Config force update, Crashlytics, Analytics with 25+ events, and Firestore logging of critical errors.

## Screens

<p align="center"><img src="assets/screens-app.png" alt="Inside the app" width="100%"></p>
<p align="center"><sub>Welcome with Google/Apple sign-in, teacher list, teacher profile with prices, and the guest account screen.</sub></p>

<p align="center"><img src="assets/screens-store.png" alt="Store listing" width="100%"></p>
<p align="center"><sub>Google Play marketing screenshots: profile, daily schedule, sessions, teachers, home and splash.</sub></p>

<p align="center"><img src="assets/screens-ipad.png" alt="iPad" width="100%"></p>
<p align="center"><sub>The same app on iPad (App Store iPad screenshots).</sub></p>

> All screenshots come from the public Google Play and App Store listings. The teacher names shown are public teacher profiles on the platform.

## Signature feature: an AI assistant for every role

<p align="center"><img src="assets/feature-ai-tutor.png" alt="AI assistant" width="100%"></p>

The assistant tab is the same entry point for everyone, but the tools change with the role:

- **Students:** solve a question from a photo, step-by-step solutions, explain a curriculum topic, summarise a lesson, flashcards, a study plan, a quick self-assessment, a Tawjihi review and a writing trainer.
- **Teachers:** generate an exam, a quiz or a question bank, build a lesson plan, grade essays, design adaptive activities, improve an explanation, compare concepts and issue a certificate.
- **Parents:** interpret a report, get home-support ideas and motivation tips, and draft a message to the teacher.

Each tool is its own page in the `users_ai` feature, with data, domain and presentation layers. Above the tools sits a **StudyGo channel**: YouTube stories played in-app through `youtube_explode_dart` and `video_player`.

## Payments: hosted checkout with no card data in the app

<p align="center"><img src="assets/feature-checkout.png" alt="Checkout flow" width="100%"></p>

Every paid product (plan upgrade, lesson booking, group-session pay-now, wallet top-up, program purchase) has its own "start" endpoint, but all of them return the same shape: `{ mode, redirectUrl?, purchaseId? }`.

- `mode: checkout` → the app opens the URL in a **real browser tab** (`flutter_custom_tabs`). Then it waits for the deep link `studygo://payments/result?status=…&purchaseId=…`, which `app_links` catches. A `PaymentFlowService` parses it and shows the right result screen.
- `mode: auto_confirm` → the server has already activated the product, so the app goes straight to success.
- **Lost redirect?** If the user closes the tab, the app polls the purchase status as a fallback.
- All amounts are signed integers in **agorot** (1/100 ILS) on the wire and are formatted only at the edge.

## Architecture & stack

```mermaid
flowchart LR
  subgraph Presentation
    Screens["Role shells<br/>student · teacher · parent"] --> Cubits["Cubits (flutter_bloc)"]
  end
  subgraph Domain
    UseCases["Use cases<br/>UseCase&lt;Result, Params&gt;"] --> Entities["Entities + repository contracts"]
  end
  subgraph Data
    Repos["Repository impls"] --> Remote["Remote data sources<br/>(DioClient + interceptor)"]
    Repos --> Local["LocalStorageService"]
  end
  Cubits --> UseCases
  UseCases --> Repos
  Remote --> API["REST API (OpenAPI)"]
  DI["GetIt container"] -.wires.-> Cubits
  Router["GoRouter + AuthGuard<br/>+ notification deep links"] --> Screens
  Pay["PaymentFlowService<br/>Custom Tabs + app_links"] --> API
  Firebase["FCM · Crashlytics · Analytics<br/>Remote Config · Firestore logs"] -.-> Presentation
```

| Layer | Technology |
|---|---|
| Architecture | Clean Architecture, feature-first: `features/<x>/{data,domain,presentation}`, `core/`, `shared/`, `di/` |
| State | `flutter_bloc` 9 + `equatable`; sealed states |
| DI | `get_it` |
| Routing | `go_router` with an auth guard, role-based shells and notification deep links |
| Errors | `dartz` `Either` and typed `Failure` classes mapped from HTTP codes (Arabic messages) |
| Networking | Single `DioClient`, a bearer-token interceptor and `connectivity_plus` |
| Payments | `flutter_custom_tabs` + `app_links` (deep link `studygo://`) |
| Media | `video_player` + `chewie`, `flutter_pdfview`, `youtube_explode_dart`, `cached_network_image` |
| Maps | `flutter_map` + OpenStreetMap, `geolocator` |
| Auth | `google_sign_in`, `sign_in_with_apple` (native ID-token flow) |
| Firebase | Messaging, Crashlytics, Analytics, Remote Config, Cloud Firestore; `flutter_local_notifications` |
| Design | "Academic Curator" design system: Academic Blue `#0C4381` and Progress Orange, Manrope / Cairo / Inter fonts |

## Engineering notes

- **The exam clock never trusts the device.** The countdown reads the offset the server returns with the attempt and re-anchors on every save. A phone that is ten minutes fast still shows the true remaining time. Saving an answer rebuilds the paper, but the question being typed into does not re-animate. The pinned header turns red in the last five minutes.
- **Grading that respects the teacher.** Objective answers arrive pre-scored. The grading queue only holds human-marked questions and the final confirmation. Students and parents see nothing until the teacher publishes.
- **A browser tab, not a WebView, for payments.** 3-D Secure challenges and Google Pay refuse to run inside WebViews. A Custom Tab (Android) or Safari view (iOS) plus a deep-link return keeps card data out of the app and passes store review.
- **Guest mode by design.** A guest is just a logged-out app: no token and no server state. Public endpoints are called without an `Authorization` header. Any gated action ("book with…", "save this list", "link your child") opens a contextual Google/Apple sign-up sheet over the current screen.
- **Notifications that land in the right place.** Each notification kind maps to a deep link. One router handles taps from both FCM pushes and the in-app feed. Token refreshes are re-registered automatically.
- **Fault-tolerant startup.** Firebase initialisation is wrapped so a misconfigured service can't crash launch. Analytics calls go through a `_safe()` guard. Critical errors are also written to Firestore for triage.

## Results

- **Live on Google Play** ([com.study.go](https://play.google.com/store/apps/details?id=com.study.go)) with **100+ installs**.
- **Live on the App Store** ([Study Go](https://apps.apple.com/sa/app/study-go/id6777997182)) in the Education category, version 1.0.3, for iPhone and iPad.

## بالعربي

**ستادي جو** منصة تعليمية تربط طلاب القدس والمدن الفلسطينية بمعلمين متخصصين في التوجيهي والبجروت، داخل تطبيق واحد بثلاث واجهات: طالب ومعلم وولي أمر.
فيها حجز حصص خاصة وجماعية، ومتجر برامج دراسية، وامتحانات بمؤقت مرتبط بالخادم مع تصحيح يعتمده المعلم، ومحفظة وأرباح للمعلم.
وفيها 27 أداة ذكاء اصطناعي حسب الدور، وقصص يوتيوب تعليمية داخل التطبيق.
الدفع يتم عبر صفحة دفع مستضافة في متصفح حقيقي، ثم يعود المستخدم للتطبيق عبر رابط عميق. التطبيق مبني بـ Clean Architecture وBloc وGetIt وGoRouter، ومنشور على Google Play وApp Store.

## Links

- ▶️ [Google Play](https://play.google.com/store/apps/details?id=com.study.go)
- 🍎 [App Store](https://apps.apple.com/sa/app/study-go/id6777997182)

---
<h3 align="center">Built by Mahmoud Shaker — Senior Flutter Engineer</h3>
<p align="center">
  <a href="https://wa.me/201140279111"><img src="https://img.shields.io/badge/WhatsApp-Chat-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" alt="WhatsApp"></a>
  <a href="https://www.linkedin.com/in/anashaker/"><img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <a href="https://anashaker.github.io/shaker_portfolio/"><img src="https://img.shields.io/badge/Portfolio-Visit-111827?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Portfolio"></a>
  <a href="mailto:mahmoudshaker382001@gmail.com"><img src="https://img.shields.io/badge/Email-Hire%20me-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"></a>
  <a href="https://khamsat.com/user/sheko201566"><img src="https://img.shields.io/badge/Khamsat-Services-F39C12?style=for-the-badge" alt="Khamsat"></a>
  <a href="https://mostaql.com/u/ShekoShaker"><img src="https://img.shields.io/badge/Mostaql-Freelance-2386C8?style=for-the-badge" alt="Mostaql"></a>
  <a href="https://github.com/ANaSHaker"><img src="https://img.shields.io/badge/GitHub-More%20work-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"></a>
</p>
