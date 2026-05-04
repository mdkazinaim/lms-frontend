# EduCore — Learning Management Platform
## Frontend Planning Document

---

## 1. Platform Overview

EduCore is an open-access educational platform where **institutes**, **teachers**, and **students** can register and interact. The platform does not require a subscription to register — content access may optionally be free or paid based on the provider's choice.

The platform serves three main user types:
- **Institutes** — Organizations that can hire teachers, manage students, and publish institutional content.
- **Teachers** — Can be affiliated with an institute or operate independently as individuals.
- **Students** — Can enroll under institutes and/or individual teachers, or remain independent and access platform-wide content.

---

## 2. User Roles & Registration

### 2.1 Registration Flow
All users (institute, teacher, student) register through a single unified registration page. During registration, users select their **initial role**:
- Student
- Teacher (Individual)
- Institute

> **Clarification on Roles:** A user account can hold multiple role capabilities over time. A student can upgrade to individual teacher. An individual teacher, however, cannot "become" an institute — institutes must register independently as an organization.

---

### 2.2 Institute Account
- Registers as an organization with institute name, address, contact, and verification info.
- Can **hire teachers** from the platform by sending invitations, or teachers can apply to join.
- Every institute gets a **public Landing Page** that showcases:
  - Hero/Banner section (custom image, tagline, call-to-action).
  - About the institution (description, history, mission).
  - Notice board (upcoming exams, events, announcements).
  - Teachers section (featured teachers with photo and subject).
  - Student testimonials or student highlights section.
  - The institute admin can configure and customize this landing page from the dashboard.
- Upon a teacher or student **joining the institute**, after login they are redirected to the **Institute Home Page** (landing page) first, then forwarded to their own dashboard. This gives a sense of belonging to the institution.
- When a teacher joins an institute:
  - The teacher works **under the institute's authority**.
  - Materials created by the teacher under the institute are **co-owned** — institute retains access even if teacher leaves.
  - The teacher retains their **personal copies** of their own original materials and can take them when they leave.
- Can publish: Courses, Batches, Tests, Practice Sets, Quizzes, Free Resources, Guidelines, News.
- **Pricing and content publishing** require approval from the institute admin. Affiliated teachers cannot set pricing or go live without explicit admin authorization.
- Has a dedicated **Institute Dashboard** with student management, teacher management, analytics, content management, and landing page configuration.

---

### 2.3 Teacher Account

#### Affiliated Teacher (Under an Institute)
- Joins an institute via invitation or application.
- Can create and publish all content types **on behalf of the institute**.
- Has dual access: institute-scoped and personal.
- Can **leave the institute** at any time:
  - Personal materials follow the teacher.
  - Institute-created materials (funded/hosted by institute) remain with the institute.
  - The institute may allow the teacher to take certain materials — a configurable permission.
- **Pricing and publishing rights** must be granted explicitly by the institute admin. By default, new affiliated teachers require admin approval before content goes live.

#### Individual Teacher
- Operates independently with no institute affiliation.
- Can publish all content types on their own profile.
- Can set pricing, subscription plans, or make content free — full authority.
- Has a **Teacher Dashboard** with student management, content management, earnings, and analytics.
- Students can **follow** or **enroll** under an individual teacher.
- Can create and manage **Batches** for organized course delivery.

---

### 2.4 Student Account
- Registers as a student.
- Can **enroll** under one or more institutes and/or individual teachers simultaneously.
- Can remain as an **independent student** and access platform-provided free/subscribed content.
- Can **upgrade role** to Individual Teacher without losing student history — they retain both capabilities.
- Can interact with content: take tests, join quizzes, practice, access resources, share news and resources.
- Students can follow other students.
- A student who shares educational resources publicly can build a **contributor profile** visible on the platform.

---

## 3. Core Features

### 3.1 Courses
- Formats: Video lessons, Audio lessons, or mixed media.
- Structure: Course → Modules → Lessons.
- Attachment support: PDFs, slides, notes per lesson.
- Progress tracking: completion percentage, bookmarks, resume.
- Instructors can add quizzes at the end of lessons.
- Courses can be free, one-time paid, or subscription-gated.
- Courses can be assigned to a **Batch** (see Section 3.8) for structured, time-bound delivery.

---

### 3.2 Tests
- Designed to mimic formal school/exam-style question papers.
- Question types: MCQ, Short Answer, Long Answer, Fill in the Blank, True/False.
- Tests can be timed or untimed.
- Tests can be set by: Institute, Affiliated Teacher, Individual Teacher, or Platform (pre-made).
- Students can be assigned a test (mandatory) or browse and self-attempt (optional).
- After submission: instant results for objective questions; descriptive answers go for grading.
- **Grading authority follows the test context:**
  - Test under an **institute** → graded by the institute admin or assigned teacher.
  - Test under an **individual teacher** → graded by that teacher.
  - **Self-attempted test** (platform or pre-made) → auto-graded with personal rating and accuracy score.
- Result reports visible to the student and the test creator (and institute admin if applicable).

---

### 3.3 Practice Module
- Structured practice sessions with subject/topic filters.
- Practice sets can be created by: Institute, Teacher (affiliated/individual), Platform, or AI.
- Shared practice sets: teachers or students can share sets publicly.
- AI-generated sets: students can enter a topic, difficulty, and number of questions to auto-generate a practice session.
- Supports flashcard mode and question-drill mode.
- Tracks time spent and accuracy per session.

---

### 3.4 Quizzes
- Formats: MCQ (multiple choice), Short Answer.
- Designed for quick, interactive assessment.
- Can be embedded inside courses or standalone.
- Supports live quiz sessions (teacher hosts, students join with a code).
- Leaderboard for live quiz sessions.
- AI-generated quizzes: students or teachers can generate based on topic input.

---

### 3.5 Free Resources
- Uploaded files: PDFs, Docs, Images, Slides, Links.
- Tagged by subject, grade, or topic.
- Published by: Platform, Institute, Teacher, or Student (students can contribute).
- Students can like, save, and share resources.
- **Moderation rules apply** (see Section 8.1).

---

### 3.6 Guidelines
- Structured written guides, study tips, exam strategies, roadmaps.
- Created by: Platform, Institute, or Teacher.
- Formatted with headings, lists, callouts — not just plain text.
- Students can bookmark and rate guidelines.

---

### 3.7 News & Announcements
- All registered users can see **platform-level news** (system updates, academic news, platform announcements).
- **Institutes and individual teachers** have an option to set visibility when publishing news:
  - **Organization only** — visible only to their enrolled students and affiliated teachers.
  - **Public** — visible to all platform users.
- **Students** can write short news posts or share external links. These go to the community news feed after moderation review (see Section 8.1).
- News feed is filterable: All / My Institute / My Teachers / Platform / Community.

---

### 3.8 Batches
A **Batch** is a structured, time-bound group delivery mechanism for courses and learning programs. It bridges the gap between open self-paced learning and a live scheduled classroom experience.

#### What is a Batch?
- A Batch groups a set of students together to go through a course or learning program on a **fixed schedule** (start date, end date, weekly sessions).
- Think of it as a "class" or "cohort" — multiple batches can run for the same course at different times.

#### Who can create Batches?
- **Institute:** Can create batches for any of their courses. Students enrolled in the institute can be added to a batch.
- **Individual Teacher:** Can create batches for their own courses. Their enrolled students can join.
- **Affiliated Teacher:** Can create batches under the institute with admin approval.

#### Batch Structure
- **Batch Name** — e.g., "Morning Batch – June 2026", "Batch A – Physics Class 12".
- **Course assigned** — One or more courses linked to this batch.
- **Schedule** — Start date, end date, class days and times (Mon/Wed/Fri 10:00am, etc.).
- **Seat limit** — Optional cap on how many students can join.
- **Enrolled students** — Managed by the teacher/institute. Students can be manually added or can request to join.
- **Batch-specific tests and assignments** — Tests assigned to this batch only; only batch students see and attempt them.
- **Batch announcements** — Private notice board visible only to batch students.
- **Progress per student** — Track each student's lesson completion and test scores within the batch.

#### Batch Lifecycle
1. **Draft** — Batch created but not open for enrollment.
2. **Open** — Accepting student enrollment requests (or teacher adds students directly).
3. **Active** — Batch has started. Content releases on schedule. Students can no longer join.
4. **Completed** — End date passed. All content locked. Certificates can be issued.
5. **Archived** — Batch is closed and visible only in history.

#### Student Batch Experience
- When a student joins a batch, their dashboard shows a **Batch tab** with:
  - Upcoming class schedule.
  - Lessons unlocked so far (schedule-gated or all-at-once, configurable).
  - Batch-assigned tests and their deadlines.
  - Batch announcements.
  - Classmates list (optional, can be hidden).

---

## 4. Dashboards

### 4.1 Institute Dashboard
- **Overview:** Enrollment stats, active teachers, student count, content published count.
- **Teacher Management:** View, invite, remove teachers. Configure their authority level (publish rights, pricing rights).
- **Student Management:** View enrolled students with full profile — personal details, academic info, platform activity, behavior, progress per course/test.
- **Batch Management:** Create and manage batches, assign courses, view per-batch analytics.
- **Content Management:** All published courses, tests, practices, quizzes, resources. Approve or reject teacher submissions.
- **Analytics:** Content engagement, test performance averages, student activity trends.
- **Landing Page Configuration:** Live preview editor for the institute's public landing page — banner, about, notice, teachers section, student highlights.
- **Settings:** Institute profile, branding, permissions, roles.

---

### 4.2 Teacher Dashboard (Individual & Affiliated)
- **Overview:** Student count, content stats, recent activity.
- **My Students:** Enrolled students with full profile — personal details, academic details, platform behavior, and per-content progress.
- **Content:** Create and manage courses, tests, practice sets, quizzes, resources.
- **Batches:** Create and manage batches; view batch-level student progress.
- **Live Quiz:** Host a live quiz session.
- **Analytics:** Which content is performing well, engagement rates.
- **Earnings (Individual only):** Revenue from paid content, payout history.
- **My Public Profile (Individual only):** Configure the teacher's public-facing profile page — bio, featured courses, testimonials, social links. This is the teacher's personal landing page visible to anyone on the platform.
- **Institute Panel (Affiliated only):** Switch context between personal and institute-scoped work. Submissions pending institute admin approval are flagged here.

---

### 4.3 Student Dashboard
- **Home Feed:** Aggregated announcements, upcoming events, recent content from all enrolled providers — the default landing view for multi-enrolled students.
- **My Providers:** Quick-access panel listing all enrolled institutes and teachers. One-click jump to any provider's page.
- **My Learning:** Courses in progress, completed courses. Active batch schedule.
- **My Batches:** Batch-specific content, schedule, classmates, announcements.
- **My Tests:** Assigned tests (from institute, teacher, or batch), completed tests, result history.
- **Practice:** Recent sessions, saved sets, AI-generated sets.
- **Explore:** Browse platform content, trending resources.
- **Following:** Following students, their shared content.
- **Saved:** Bookmarked resources, guidelines, courses.
- **My Public Profile:** Configure the student's own portfolio page — achievements, bio, goals, shared resources.
- **Settings:** Profile, enrolled providers, notification preferences.

---

## 5. Content Ownership & Authority Matrix

| Content Type | Institute | Affiliated Teacher | Individual Teacher | Platform |
|---|---|---|---|---|
| Courses | ✅ | ✅ (requires admin approval) | ✅ (personal) | ✅ |
| Batches | ✅ | ✅ (requires admin approval) | ✅ (personal) | ❌ |
| Tests | ✅ | ✅ (requires admin approval) | ✅ | ✅ |
| Practice Sets | ✅ | ✅ | ✅ | ✅ |
| Quizzes | ✅ | ✅ | ✅ | ✅ |
| Resources | ✅ | ✅ | ✅ | ✅ |
| Guidelines | ✅ | ✅ | ✅ | ✅ |
| News | ✅ | ✅ | ✅ | ✅ |

**Student Contribution:**
| Content Type | Student | Moderation |
|---|---|---|
| Resources | ✅ | Platform admin review |
| News/Posts | ✅ | Platform admin review |
| Guidelines | ✅ | Platform admin review |
| Practice Sets (shared) | ✅ | Platform admin review |

---

## 6. Enrollment & Access Model

- Students can enroll under **multiple institutes and teachers** simultaneously.
- Enrollment gives access to all published content of that provider (per visibility/pricing settings).
- Students can also access **platform-wide** free content without any enrollment.
- Paid content requires purchase or subscription — this is per-provider, not platform-wide.
- Platform content may have its own free tier and optional premium subscription.

---

## 7. Role Transition Rules

| From | To | Allowed? | Notes |
|---|---|---|---|
| Student | Individual Teacher | ✅ | Gains teacher features; student history preserved |
| Individual Teacher | Back to Student only | ✅ | Can toggle view mode |
| Individual Teacher | Institute | ❌ | Institutes are separate organizational accounts |
| Student | Institute | ❌ | Not applicable |

---

## 8. Content Moderation & Publishing Rules

### 8.1 Moderation by Role

| Submitted by | Reviewed by |
|---|---|
| Student (resources, news, guides) | **Platform admin** |
| Affiliated Teacher (content under institute) | **Institute admin** — must approve before going live |
| Individual Teacher | Self-published — no review required |
| Institute | Self-published — no review required |

- Platform moderators can **flag, remove, or suspend** any content at any time for policy violations.
- Reported content (flagged by users) enters a review queue automatically.

### 8.2 Affiliated Teacher Submission Workflow
1. Teacher creates content and submits for review.
2. Institute admin is notified.
3. Admin reviews and either **Approves** (goes live), **Requests changes**, or **Rejects**.
4. Teacher is notified of the decision with optional feedback.

---

## 9. Content Sharing & Community

- Teachers can **share practice sets** publicly for any student to use.
- Students can **share resources** they find useful.
- Any shared content is attributed to the original creator.
- Platform moderates community content with flagging and review workflows.
- Student contributors with high engagement get a **"Top Contributor" badge** on their profile.

---

## 10. Platform Content

- The platform maintains its own library of free and premium content.
- Platform content is accessible to all registered users (free tier) or behind a platform subscription.
- Platform content covers common subjects, competitive exams, general learning.
- AI features (quiz/practice generation) are part of the platform's value proposition.

---

## 11. Key User Journeys

### Student: First-Time Login (Single Enrollment)
1. Registers → selects "Student" role.
2. Browses the explore page.
3. Enrolls under **one** institute.
4. On next login → shown that **institute's Landing Page** once as a welcome screen → then forwarded to own dashboard.
5. Accesses course content, joins a batch, takes assigned tests, joins live quizzes.
6. Practices using pre-made or AI-generated sets.

### Student: Multi-Enrollment Login Flow
When a student is enrolled under **multiple institutes and/or individual teachers**, showing a single institution's landing page would be unfair and confusing. The flow handles this as follows:

1. After login, the student lands on their **Personalized Home Feed** — not any single provider's page.
2. The Home Feed is an aggregated view showing:
   - Announcements and notices from all enrolled institutes and teachers, sorted by recency.
   - Upcoming tests and batch sessions across all providers.
   - Recent content published by any enrolled provider.
   - A **"My Providers" sidebar or section** listing all enrolled institutes and teachers with quick-jump links to each of their pages.
3. If the student clicks on a specific institute or teacher from the sidebar, they are taken to that provider's **Landing/Profile Page**, where they see the branded experience for that provider.
4. The first time a student enrolls in a new institute (even if already enrolled elsewhere), they are shown that institute's landing page **once** as a welcome, then redirected back to their Home Feed.
5. The student can visit any institute's or teacher's page at any time from the **"My Providers"** section in the dashboard sidebar.

> **Design Principle:** The student's dashboard is their home — provider landing pages are a contextual welcome layer, not a persistent lock-in.

### Teacher: Setting Up
1. Registers as Individual Teacher.
2. Creates profile with subjects, experience, and configures their **public profile page**.
3. Uploads content (courses, tests, resources).
4. Creates a batch for a course and opens enrollment.
5. Sets pricing or makes content free.
6. Students discover the teacher via the Explore page, their public profile, or referral links and enroll or join the batch.

### Institute: Onboarding
1. Registers as Institute.
2. Sets up institute profile and configures the **public landing page**.
3. Invites or hires teachers. Assigns their authority level.
4. Teachers create content — submitted for admin approval before going live.
5. Institute creates batches and assigns students.
6. Students enroll and are greeted by the institute landing page on their first login to that institute.

---

## 12. Open Loops — Resolved

| Loop | Resolution |
|---|---|
| Who owns content when a teacher leaves an institute? | Teacher takes personal materials; institute retains institute-labelled materials. Institute can optionally grant teacher copies. |
| Can a student be enrolled in multiple places at once? | Yes — multi-enrollment is fully supported. |
| Can a student share content to the wider platform? | Yes — resources, news, guidelines. Subject to platform admin moderation. |
| Can an affiliated teacher set their own pricing? | No — pricing requires institute admin approval. Teachers can suggest pricing but cannot publish paid content without authorization. |
| Can a teacher be affiliated and individual simultaneously? | Yes — their personal content remains personal. The institute context is separate. |
| Does a student lose history when upgrading to teacher? | No — student history is preserved; teacher features are added on top. |
| Who moderates community contributions? | Students' contributions → Platform admin. Affiliated teachers' content → Institute admin. |
| What happens to batch students if the institute/teacher account is suspended? | Batch is frozen; students retain all completed progress and certificates issued so far. |
| Can a batch run without a course attached? | Yes — a batch can be used purely for scheduling live sessions and assigning test sets without a formal course structure. |

---

## 13. Notification System

All users receive **in-app notifications** and **email notifications**. Notification types per role:

**Students receive:**
- New content published by enrolled institute or teacher.
- Upcoming test deadline reminders.
- Live quiz starting in X minutes.
- Batch class schedule reminders.
- Test graded — result available.
- Batch announcement from teacher/institute.
- Community engagement: someone liked or commented on shared content.

**Teachers receive:**
- New student enrolled.
- Test grading request (for descriptive answers).
- Institute admin approved or rejected a content submission.
- Live quiz session joined by students.
- Student completed a course or batch.

**Institute Admins receive:**
- Teacher submitted content for review.
- New teacher application or join request.
- New student enrollment.
- Teacher left the institute.

---

## 14. Certificates

- Certificates can be issued by: **Platform**, **Institute**, or **Individual Teacher**.
- Triggered when: a student completes a course, completes a batch, or passes a test above a defined score threshold.
- **Template system:** During course or batch creation, the creator selects or customizes a certificate template (logo, colors, signatory name, title).
- Certificate contains: student name, course/batch name, issuing authority, date, and a unique verification code.
- Generated as a downloadable PDF and stored in the student's profile under "My Certificates."
- Students can share certificate links publicly (for LinkedIn, etc.).

---

## 15. Additional Suggestions

> The following are recommendations that may strengthen the platform's value. Review and include if they align with the product vision.

**Teacher Verification Step**
Individual teachers should have a basic profile completion check — email verification, subject selection, and bio — before their content is discoverable. This prevents spam accounts from publishing to students without any credibility signal.

**Student Study Groups**
Students could form private study groups to share resources, discuss topics, and practice together — a lightweight community feature that increases retention.

**AI Study Assistant**
Beyond quiz/practice generation, a simple AI assistant ("Ask about this topic") within course pages and practice sessions would significantly differentiate the platform.

**Parent/Guardian View**
For school-age students, a read-only guardian account linked to a student profile can view test results, attendance, and course progress — particularly valuable for institute customers.

**Content Drafts & Scheduling**
Teachers and institutes should be able to save content as draft and schedule a publish date — critical for structured batch-based course releases.

**Audit Log for Institutes**
When a teacher leaves, an audit trail of what content was created, modified, or removed protects both parties from disputes. Also useful when a teacher challenges content ownership.

**Batch Waitlist**
When a batch hits its seat limit, students can join a waitlist and are notified if a seat opens up.

---

## 16. Public Profile & Landing Pages

Every user type on the platform has a **public-facing page** that serves as their identity on the platform. These pages are accessible to anyone (logged in or not), act as discovery surfaces, and are configurable by the owner.

---

### 16.1 Institute Landing Page

The institute's public page is a **branded, promotional landing page** designed to attract students and present the institution professionally.

**Configurable Sections (drag-and-drop order in dashboard):**
- **Hero Banner** — Full-width image/video, headline, tagline, CTA button ("Enroll Now").
- **About the Institution** — Rich text description, founding year, mission, accreditations.
- **Notices & Announcements** — Public notice board showing upcoming exams, events, deadlines.
- **Our Teachers** — Showcase featured teachers with photo, name, subject, and link to their profile.
- **Student Highlights** — Featured student achievements, testimonials, success stories.
- **Courses & Batches** — Preview of published courses available for enrollment.
- **Contact & Location** — Address, phone, email, map embed.

**Behavior:**
- New students who enroll in this institute see this page **once as a welcome screen** on their next login, then land on their Home Feed thereafter.
- The page is always accessible via the **"My Providers"** section of the student dashboard.
- The institute can set sections to public (visible to everyone) or members-only (visible only to enrolled students and teachers).

---

### 16.2 Teacher Public Profile Page

Individual teachers get a **personal profile page** that serves as their public presence — think of it as a professional home page or a lightweight teaching portfolio. This is their primary tool for self-promotion and student acquisition.

**Configurable Sections:**
- **Profile Header** — Profile photo, name, title/tagline (e.g., "Physics Teacher | 8 Years Experience"), subject tags, verified badge (if applicable).
- **About Me** — Rich text bio. Teaching philosophy, qualifications, what students can expect.
- **Featured Courses & Batches** — Highlighted offerings with pricing, enrollment count, and a "Join" CTA.
- **Student Testimonials** — Reviews or endorsements from enrolled students (students can submit reviews, teacher can feature selected ones).
- **Stats Bar** — Total students, courses published, average rating, years active.
- **Free Resources** — Publicly shared resources for visitor discovery.
- **Social & Contact Links** — LinkedIn, YouTube, personal site, email (optional).

**Behavior:**
- The page is publicly accessible via a shareable URL: `platform.com/teachers/username`.
- Teachers affiliated with an institute also have their personal profile page — it is separate from their institute context.
- Affiliated teachers can optionally display a badge showing which institute they are currently working under.
- Students can **enroll directly** from the teacher's profile page.

**Dashboard Access:** Teacher Dashboard → **My Public Profile** tab → live preview editor with section toggles.

---

### 16.3 Student Portfolio Page

Students have an **optional public portfolio page** — a lightweight academic profile they can share externally (e.g., with employers, scholarship committees, or on social media). It is off by default and must be explicitly enabled by the student.

**Configurable Sections:**
- **Profile Header** — Profile photo, name, headline (e.g., "Commerce Student | Aspiring CA"), institution tags (showing enrolled institutes/teachers).
- **About Me** — Short bio, academic interests, career goals.
- **Achievements** — Earned certificates from courses, batches, or tests (student chooses which to display).
- **Learning Progress (optional)** — Courses completed count, test scores summary, badges earned ("Top Contributor", streak badges, etc.).
- **Study Plan / Goals (optional)** — A written section where the student can share their learning roadmap or upcoming exams they are preparing for.
- **Shared Resources** — Publicly shared resources and posts contributed to the platform community.
- **Following / Followers** — Other students following them (like a social academic profile).

**Privacy Controls (critical):**
Each section has an individual **visibility toggle**: Public / Followers only / Private. Students are in full control of what is visible and to whom.

**Shareable URL:** `platform.com/students/username` — can be shared like a LinkedIn profile link.

**Dashboard Access:** Student Dashboard → **My Public Profile** tab → section editor with per-section privacy controls.

---

### 16.4 Summary: Profile Page Comparison

| Feature | Institute Page | Teacher Page | Student Portfolio |
|---|---|---|---|
| Primary purpose | Institutional branding & enrollment | Self-promotion & student acquisition | Academic identity & achievement sharing |
| Public URL | ✅ | ✅ | ✅ (opt-in) |
| Configurable sections | ✅ | ✅ | ✅ |
| Per-section privacy | ❌ (public or members) | ❌ (fully public) | ✅ (per section) |
| Student can enroll from page | ✅ | ✅ | ❌ |
| Certificate display | ❌ | ❌ | ✅ |
| Shown on first login (welcome) | ✅ (once per institute) | ❌ | ❌ |
| Editable from dashboard | ✅ | ✅ | ✅ |

---

*Document Version: 1.2 — April 2026*
*Status: Planning Phase — For Developer Review*
