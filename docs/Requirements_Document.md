# Campus Companion: Requirements Document
- **Team Members:** Kaden Sawyer, Laila
- **Version:** 1.1 | **Date:** 2026-11-18
- **Related Files:** User_stories.md | Design_Diagrams| Technical_Specification.md

## 1. Version and Change Log
 
| Version | Date | Change | Author |
| ------- | ---- | ------ | ------ |
| 1.0 | 2026-11-11 | Draft written from the Week 3 essay, the Week 4 stories, and D0, before D1 and D2 were finished. | Laila Raines, Kaden Sawyer |
| 1.1 | 2026-11-18 | Rebuilt from the finished D1 and D2. Functional requirements are now REQ-01 to REQ-20, each tied to a Week 4 story or acceptance criterion (AC). The NFR numbers NFR-01 to NFR-09 are the ones defined in D2 Part 4, and NFR-10 to NFR-12 are new (usability and reliability). Priorities follow D2 Part 4. NFR-11 and NFR-12 are our additions (Should), and all are proposals until the team confirms them (Q5). | Laila Raines, Kaden Sawyer |
 
IDs are stable and never reused. A dropped requirement stays in its row marked WITHDRAWN, and the change is logged here. Numbers marked † are proposals that the team has not yet confirmed.
 
## 2. Purpose and Scope
 
**Purpose.** This document states what Campus Companion must do and for whom. It does not say how the system is built; the [Technical Specification](Technical_Specification.md) does that and traces to this version.
 
**Scope.** Campus Companion is a mobile app, a web app with an admin portal, and one server for first-year and transfer students at the University of Cincinnati's Uptown West campus. This release covers:
 
- Importing a class schedule from a PDF, reviewing it, and entering courses by hand, only after the student consents.
- Picking interest categories, showing the walk between back-to-back classes with its time and a route on a map, and suggesting events that fit between classes.
- Club verification by an admin, event posting and cancelling by a verified club head, and push notices to students with matching interests.
- Screen-reader access to every schedule and event field.
Items left out of this release are in the Won't list (section 7).
 
## 3. Stakeholders and User Goals (Week 4)
 
| Stakeholder | Category | Goal | Stories |
| ----------- | -------- | ---- | ------- |
| First-year or transfer student | Primary | Upload a schedule with little friction, get between classes on time on an unfamiliar campus, and find events that do not conflict with classes | US-01, US-02, US-05, US-05-M |
| Head of a student club or organization | Secondary | Post and cancel events and reach interested students without messaging each one | US-03 |
| Student who needs assistive technology | Hidden | Read or listen to their schedule and campus events with accessibility tools built into the app | US-04 |
| University student-records privacy office (FERPA) | Hidden | Schedule data is stored only with the student's consent, kept to the minimum needed, and fully deleted when the student removes it | UC-01-P; NFR-01 to NFR-04 |
| Admin (added in D2) | Secondary | Confirm that each club request comes from a real officer before the club can post | UC-02-P |
 
## 4. Glossary
 
| Term | Definition |
| ---- | ---------- |
| Admin | A signed-in user whose Supabase Auth role is admin (the two team members during the capstone). Approves, denies, and revokes club requests in the admin portal. |
| Assistive-technology user | A student who uses a screen reader such as VoiceOver (iOS) or TalkBack (Android). |
| Category | One of the 23 organization categories copied from GetInvolvedUC. Students pick categories as interests, and clubs and events carry categories. |
| Club | A student organization at the University of Cincinnati. A club exists in the system only after an admin approves it. |
| Club head | The student who leads a verified club. Only the head can post and cancel the club's events. |
| Club request | A student's request for a club to be verified: name, description, 1 to 5 categories, and an optional GetInvolvedUC link. It is Pending, Approved, or Denied. |
| Consent | The student's agreement on the consent screen to let the system store their schedule and interests. One consent covers both. |
| Course | One saved class meeting: name, professor, building, room, days, times, and start and end dates. A lecture and a lab are two courses. |
| Acceptance criterion (AC) | A numbered, testable condition from the Week 4 user stories (for example AC-01.1) that a story must meet. |
| Course draft | A course read from a PDF that the student has not yet confirmed. It exists only in the app until confirmed. |
| Edge case (EC) | A numbered unusual situation (for example EC-41) from the Week 4 edge-case table that a requirement must handle. |
| Event | A campus activity posted by a club head: name, building, room, description, start and end times, and categories. It is published or cancelled. |
| FERPA | The U.S. Family Educational Rights and Privacy Act, which governs student education records. |
| GetInvolvedUC | The University of Cincinnati's student-organization site that admins check by hand (D0 calls it E2 Campus Links). |
| Interest | A category a student picks so that events with that category are suggested to them. |
| Leg | The walk between two back-to-back classes on one date, with gap, walk minutes, distance, path, and a status. |
| MoSCoW | The priority scale used here: Must, Should, Could, Won't |
| Notice | A push notification about a published or cancelled event. It holds only public event information. |
| Notice job | A stored row that tells the server to send one publish or cancel notice. It is written in the same transaction as the event. |
| Privacy office | The University of Cincinnati office that reviews how student data is handled; its guidance sets the minimum-data rules in NFR-02 to NFR-04. |
| Push token | The Expo address of one phone (ExponentPushToken[...]) that the app registers so the server can send notices. |
| Review step | The screen after a PDF import where the student checks, edits, and confirms or cancels the course drafts. |
| Schedule | The set of a student's saved courses. |
| Schedule file | A PDF of the student's class schedule, 10 MB (10,485,760 bytes) or less. |
| Screen reader | Software that reads screen content aloud. |
| Student | A first-year or transfer student who uses the app with a confirmed UC email address. |
| Verified club | A club whose verified_at is set because an admin approved it. |
| Walk table | The stored walking time, distance, and path for every pair of campus buildings, built from open map data. |
| Walk time | The estimated minutes to walk between two buildings, in whole minutes. |
 
## 5. Functional Requirements
 
| ID | Requirement | Priority | Source story | Verification |
| -- | ----------- | -------- | ------------ | ------------ |
| REQ-01 | The system shall accept a schedule file only if it begins with "%PDF-" and is 10,485,760 bytes or less, and for any other file shall return 415 not_a_pdf or 413 file_too_large and save 0 courses; a 10,485,760-byte PDF is accepted and a 10,485,761-byte file is rejected. | Must | US-01 (AC-01.6; EC-01, EC-05) | Test |
| REQ-02 | When a signed-in student who has consented selects a valid PDF with 5 courses, the system shall return all 5 courses with name, professor, building, room, and time matching the file within 10 seconds, and shall list each field it could not read so the app highlights it. | Must | US-01 (AC-01.1) | Test |
| REQ-03 | The system shall show the course drafts for review before saving anything, let the student edit any field, and save courses only on confirmation; on confirmation it shall show a success message and the saved schedule within 5 seconds, and on cancellation it shall save 0 of the drafts. | Must | US-01 (AC-01.2, AC-01.3) | Test |
| REQ-04 | When the file has no readable course, is damaged or password protected (422 unreadable_pdf), or takes more than 8 seconds to read (504 extraction_timeout), the system shall within 10 seconds tell the student, save 0 courses, and offer manual entry. | Must | US-01 (AC-01.4; EC-03, EC-06, EC-08) | Test |
| REQ-05 | After the student types the first 3 characters of a building name in manual entry, the system shall return 1 to 5 matching buildings within 2 seconds, with short-code matches first and then names from A to Z. | Should | US-01 (AC-01.5) | Test |
| REQ-06 | The system shall save 0 courses and 0 interests for a student who has not consented: every schedule endpoint shall return 403 consent_required until the student agrees on the consent screen, and the saving function shall check consent again when it saves. | Must | US-01 (UC-01 precondition; UC-01-P); CON-01 | Test, Inspection |
| REQ-07 | The system shall let a consenting student save 0 to 20 different interest categories from the 23 categories and shall reject any unknown category with 422 unknown_category. | Must | US-02 | Test |
| REQ-08 | For each pair of back-to-back classes on a requested date, the system shall return the gap in minutes, the walk in whole minutes, the distance in whole meters, and a status of ok, tight (walk longer than gap), same_building, unknown_building, or no_path; when the walk table is not loaded it shall return 503 routes_unavailable and the app shall still show the schedule. | Must | US-05 | Test |
| REQ-09 | The system shall, for each leg that has a path, draw the path on a map with markers at both buildings, show the minutes and distance as text above the map, and show "© OpenStreetMap contributors"; if the background map cannot load, the line and markers shall still draw. | Should | US-05-M (EC-71) | Test, Demonstration |
| REQ-10 | The system shall suggest only events that share at least one of the student's categories and fit between the student's classes after the real walk from the previous class and to the next class plus a 5-minute margin (15 minutes when a walk is missing), so that 0 suggested events overlap a class; the window is at most 14 days. | Must | US-02 | Test |
| REQ-11 | The system shall list published events in a window of at most 14 days, sorted by start time, 50 per page by default (at most 100), with an optional category filter, shall not list cancelled events, and shall return a cancelled event with status cancelled when it is opened by its ID. | Must | US-02, US-03 (AC-02.1 list); CON-04 | Test |
| REQ-12 | The system shall let any signed-in student request verification of a club (name 3 to 100 characters, description 20 to 2,000, 1 to 5 categories) and shall make a club verified only when an admin approves; an admin shall approve or deny each request (a denial needs a note of up to 500 characters), and a second pending request for the same club name from the same student shall return 409 request_pending. | Must | US-03 (UC-02 precondition; UC-02-P) | Test, Demonstration |
| REQ-13 | When an admin revokes a club (note of 1 to 500 characters), the system shall clear its verified status, cancel every upcoming event of that club, queue a cancel notice for each, and return the number of events cancelled. | Could | UC-02-P (no story) | Test |
| REQ-14 | When a club head posts an event, the system shall give it all of the club's categories if none are requested, and otherwise only categories that are the club's own, rejecting others with 422 category_not_allowed (or too_many_categories above 5). | Should | US-03 (AC-02.1); NFR-06 | Test |
| REQ-15 | When the verified head of a club submits an event with a name (1 to 100 characters), a building, a description (1 to 2,000), a start in the future, and an end after the start and at most 24 hours later, the system shall publish it and show it in the event list within 5 seconds; posting the same name and start time twice shall create 1 event and return 409 duplicate_event. | Must | US-03 (AC-02.1 list; EC-33, EC-35) | Test |
| REQ-16 | When a required event field is missing or invalid, the system shall return 422 naming the field (for example event.building_id), publish 0 events, and send 0 notices, and the app shall highlight the field. | Must | US-03 (AC-02.3; EC-36) | Test |
| REQ-17 | Within 60 seconds of an event being published, the system shall send a notice to every active registered phone of every student who has at least one of the event's categories, holding only public event information, and shall retry a failed send up to 6 attempts (after 5, 10, 20, 40, and 80 seconds) while the event has not started. | Must | US-03 (AC-02.1 notice) | Test |
| REQ-18 | When the head of an event's club confirms cancellation before the event starts, the system shall mark it cancelled, remove it from the event list and suggestions, and send a cancel notice to the same recipients within 60 seconds; it shall return 409 event_already_started or already_cancelled otherwise, and shall send no cancel notice if the publish notice was never sent. | Must | US-03 (AC-02.2; EC-34, EC-40) | Test |
| REQ-19 | Before the head confirms a cancellation, the system shall show the number of students with a matching interest, and never their names. | Should | US-03 (UC-02 alternate flow) | Test |
| REQ-20 | The system shall let a signed-in user register a phone's push token (the ExponentPushToken[...] format, at most 200 characters, platform ios or android) and remove it on sign-out; after removal, or after Expo reports the phone as not registered, that phone shall receive 0 further notices, and a token registered by a new student shall move to that student. | Must | US-03 (EC-53, EC-54, EC-55) | Test |
 
## 6. Non-Functional Requirements
 
| ID | Requirement | Priority | Source | Verification |
| -- | ----------- | -------- | ------ | ------------ |
| NFR-01 | Consent and withdrawal (security, privacy): when a student withdraws consent, the system shall delete all of that student's courses and interests in one database call so that 0 of them remain, and shall record consented_at before any schedule write. | Must | UC-01-P; privacy office; CON-01 | Test, Inspection |
| NFR-02 | Minimum data (privacy): the system shall store only the course fields that routing and conflict checks need, write 0 schedule data to the phone's storage, put only public event information in notices, keep 0 recipient lists in notice jobs, delete finished jobs after 30 days, and write 0 course names, emails, or push tokens to log lines (checked by a unit test that fails if a schedule field reaches a log line). | Must | D0 Part 6; privacy office | Inspection, Test (unit test fails if a schedule field reaches a log line) |
| NFR-03 | Real deletion (security, privacy): when a student deletes their account, the system shall remove the login and all data tied to the student so that 0 rows remain in the student, course, student_interest, push_device, and club_request tables, after first cancelling the upcoming events of any club the student heads. | Must | D0 Part 6; privacy office | Test |
| NFR-04 | Data isolation (security): the system shall keep the Supabase secret key only in the server's host settings (0 secrets found in the repository and its git history), turn row-level security on for every table with no policies, take the user ID for every query from the verified login token, carry all app traffic over HTTPS, answer 100% of requests without a valid token with 401, answer student calls to admin endpoints with 403 not_admin, and answer calls by a non-head to a club's endpoints with 403 not_club_head. | Must | D0 Part 6; privacy office | Test, Inspection |
| NFR-05 | Open map data (legal): the system shall run routing only on our server with OpenStreetMap data, send 0 class locations to an outside routing service, and show the OpenStreetMap credit on every map. | Must | CON-02; D-02 | Inspection, Test |
| NFR-06 | Identity-based categories (diversity): the system shall offer all 23 GetInvolvedUC categories, including Cultural and Religious, and let students filter the event list by any one of them. | Should | CON-04 | Test |
| NFR-07 | Cost: the system shall use only services on a free tier or already paid for (the team's Apple Developer Program membership), apart from the server host, which may cost up to $7 a month. | Must | D0 Part 6 | Analysis |
| NFR-08 | Performance: the system shall respond to GET /v1/events/suggested within 2 seconds for the default 7-day window. | Should | D1 Part 3.2 (proposed, Q5) | Test |
| NFR-09 | Account trust (security): the system shall return 403 uc_email_required to 100% of requests whose login email is unconfirmed or does not end in mail.uc.edu or uc.edu, and the apps shall refuse other domains at sign-up. | Must | UC-02-P; D2 decision 2026-11-11 | Test |
| NFR-10 | Usability (accessibility): the system shall give 100% of schedule and event fields on every screen an accessibility label and let VoiceOver (iOS) and TalkBack (Android) read them in visual order, make buttons at least 44 by 44 points, read times as words, and show no information by color alone. | Must | US-04 | Test (manual, before each demo) |
| NFR-11 | Reliability (atomic writes): when a database write fails, the system shall leave 0 partial changes, because every write that touches more than one row runs as one database function. | Should | D1 Part 5 (EC-09, EC-37); D2 R-05 | Test |
| NFR-12 | Reliability (notice durability): the system shall, after a server restart, send or mark skipped or failed 100% of notice jobs queued before the restart, with 0 lost, and make a repeated send replace the first copy on the phone. | Should | AC-02.1; D2 R-06 (EC-41, EC-43) | Test |
 
## 7. MoSCoW Summary
- **Must (23):** REQ-01, REQ-02, REQ-03, REQ-04, REQ-06, REQ-07, REQ-08, REQ-10, REQ-11, REQ-12, REQ-15, REQ-16, REQ-17, REQ-18, REQ-20, NFR-01, NFR-02, NFR-03, NFR-04, NFR-05, NFR-07, NFR-09, NFR-10.
- **Should (8):** REQ-05, REQ-09, REQ-14, REQ-19, NFR-06, NFR-08, NFR-11, NFR-12.
- **Could (1):** REQ-13 revoke a club.
- **Won't (this release):**
 
- Editing an event after it is posted: a mistake is fixed by cancelling and posting again (D2 GAP-2).
- Saving an event to a student's own schedule: notices and event lists are how students find events (D2 GAP-3).
- Holidays, breaks, hills, stairs, and weather in walking times (D2 GAP-4).
- The orientation flow, study spaces, and dining from the README (D2 GAP-8).
- Professor ratings, cut in Week 4 because they depended on an outside ratings source.
- Following a single club as an interest; interests are categories only (D2 R-03).
- Guessing a club's categories from its description by software; a person checks them at approval (D2 R-09).
- Reading Expo's delivery receipts (D2 GAP-1).
- Schedule import from any format other than PDF.
- Official university maps, logos, or any connection to a student-records system.

## 8. Inherited Constraints
 
| ID | Constraint | Area |
| -- | ---------- | ---- |
| CON-01 | Follow FERPA's consent principles: require explicit student permission before saving any data. | Legal |
| CON-02 | Use an open map source instead of copyrighted school maps or logos, to avoid intellectual property issues. | Legal |
| CON-03 | Consent screens add friction at sign-up, which pressures the social goal of reaching as many new students as possible, so the consent step must stay short and clear. | Legal, Social |
| CON-04 | Let students filter the event feed by interest and identity-based organizations instead of showing only the most popular events. | Diversity and Cultural |
| CON-05 | Put campus events next to the student's schedule so that taking part is easier. The design meets this with suggestions that fit between classes, not by saving events to the schedule (see Q6). | Social |
 
## 9. Assumptions and Open Questions
 
- (*) marks a date proposed in this draft. Q1 to Q5 are from D2.
 
| ID | Item | Owner | Date |
| -- | ---- | ----- | ---- |
| A-01 | The mobile app runs on iPhone and Android as Expo development builds, and the team already has an Apple Developer Program membership. | Kaden Sawyer | 2026-12-05† |
| A-02 | UC schedule PDFs follow the Catalyst layout that the reading rules assume (D2 GAP-9). | Kaden Sawyer | 2027-02-14* |
| A-03 | OpenStreetMap has enough campus paths for walks between most building pairs (D-02; EC-11). | Kaden Sawyer | 2027-01-31* |
| Q1 | Which host runs the server: Render, Railway, or Azure for Students? It must not sleep, or the 10 s and 60 s budgets are at risk. | Kaden Sawyer, Laila Raines | 2027-01-31* |
| Q2 | Do we build the mobile app with Expo development builds? Push notices and the map need them. | Kaden Sawyer | 2026-12-05* |
| Q3 | Who acts as admin after the capstone, and how does an admin confirm that a requester is a club officer? | Laila Raines | 2027-04-01* |
| Q4 | Do we set up our own sender for sign-up emails? Supabase's built-in sender allows only a few emails an hour. | Kaden Sawyer | 2027-01-31* |
| Q5 | Do we confirm the priorities in this document and the 2-second budget in NFR-08? | Kaden Sawyer, Laila Raines | 2026-11-25* |
| Q6 | CON-05 says events go on the student's schedule, but D2 decided not to save events there (GAP-3). Do we keep the decision and reword CON-05 in the Constraints Essay? | Laila Raines | 2026-11-25* |
| Q7 | The advisor has not yet reviewed these documents. The review is recorded in the Technical Specification, section 7. | Kaden Sawyer, Laila Raines | 2026-11-25* |
| Q8 | The tunable numbers (walking speed 1.2 m/s, 5-minute margin, 15-minute fallback, 8 s reading limit) are untested guesses (D2 GAP-5). | Kaden Sawyer | 2027-02-28* |