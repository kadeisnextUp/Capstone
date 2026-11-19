**Team members:** Kaden Sawyer, Laila Raines
**Version:** 1.0 | **Date:** 2026-11-18
**Traces to:** [Requirements_Document.md](Requirements_Document.md) 1.1 and [Technical_Specification.md](Technical_Specification.md) 1.1. Test case IDs TC-001 to TC-069 are the ones in the Test Case column of the traceability matrix.
 
## 1. Scope
 
**In scope this cycle.** The 23 Must requirements (MoSCoW): REQ-01 to REQ-04, REQ-06 to REQ-08, REQ-10 to REQ-12, REQ-15 to REQ-18, REQ-20, NFR-01 to NFR-05, NFR-07, NFR-09, and NFR-10, with 69 test cases. Components: C1 to C7, G1 to G4, A1 and A2, and the interfaces I1a to I14 between them. The CON-01 to CON-05 constraints are covered through the requirements that carry them.
 
**Out of scope.**
- The 8 Should requirements (REQ-05, REQ-09, REQ-14, REQ-19, NFR-06, NFR-08, NFR-11, NFR-12) and the Could requirement REQ-13: they get no test cases this cycle and are checked by the demos and by the unit tests that already cover their code. Cases for them can be added if time allows.
- Won't features (Requirements section 7): editing events, saving events to a schedule, holidays, hills, stairs and weather, the orientation flow, study spaces and dining, professor ratings, following a single club, software guessing of categories, Expo delivery receipts (GAP-1), non-PDF schedule import, and any connection to official UC maps or student records.
- Known limitations we accept: no timing budget beyond those in section 3 (GAP-5), notices only reach online phones that allow notifications (GAP-6), cancel notices after an interest change (GAP-7).
- Third-party components we do not retest: Supabase Auth and PostgreSQL internals, Expo's delivery to Apple and Google, OpenFreeMap uptime (GAP-10), OpenStreetMap data accuracy beyond the 10 sample routes, and the internals of pdfplumber, OSMnx, NetworkX, MapLibre, FastAPI, and Pydantic. We test how we use them.
- Load testing: the 1,200-phone worst case in D2 2.9 is an estimate, not a test target.


## 2. Verification and Validation Strategy
 
**Verification** (did we build it right) is done by the team against the specification: every case in section 4 checks a measurable condition in a Week 9 requirement, using automated tests where a computer can decide (unit and integration) and a written script where a person must (VoiceOver, TalkBack, phone arrival times). The test lead checks that every Must requirement has at least 3 cases, including one edge case or failure.
 
**Validation** (did we build the right thing) is done by people who are not the authors. Five first-year or transfer student volunteers import their own schedule (with consent), read the routes, and rate whether the suggested events were useful; two club heads post and cancel an event; the advisor, Professor Zhao, watches the backend demo (IM-05) and the feature freeze demo (IM-08) and records feedback in the notebook. This is also where the real-schedule accuracy of C1 is measured (GAP-9). Two TC cases are traded with another team before the freeze to check that the steps can be followed without questions.
 
This project has no sensors or machine learning. Its two measured algorithms are the walk time (C2) and the event fit check (C3); their variables, metrics, and tolerances are in section 3.
 
| Layer | Approx. count | Coverage focus |
| ----- | ------------- | -------------- |
| Unit (pytest, run on every commit) | 45 | parse_schedule(), match_building(), build_walk_table(), suggest_events(), resolve_event_categories(), build_notice(), log redaction, load_settings() |
| Integration (API against a test Supabase project) | 30 | Endpoints and error codes, row-level security, the SQL functions (replace_courses, publish_event, cancel_event, claim_notification_jobs), the worker with a stubbed Expo |
| System (apps and server on staging) | 69 | The cases in section 4, run on real phones, the web portal, and the staging server |
| Acceptance (users and advisor) | 8 | The volunteer sessions, the club head sessions, and the two advisor demos |
 
## 3. Measurable Success Criteria
 
| ID | Metric | Target threshold | How it is measured | Requirement |
| -- | ------ | ---------------- | ------------------ | ----------- |
| M-01 | Schedule import time (5-course PDF) | 10 s or less | Stopwatch and server log duration_ms, 10 runs on staging | REQ-02, REQ-04 |
| M-02 | Field accuracy of the PDF reader | 90% or more of fields match, on 5 real UC schedules | Compare each draft field with the printed schedule | REQ-02 |
| M-03 | Confirm-to-schedule time | 5 s or less | Stopwatch, 10 runs | REQ-03 |
| M-04 | Walk time error | Within 2 minutes of a timed walk, on 10 building pairs | Walk each pair with a timer | REQ-08 |
| M-05 | Overlapping suggestions | 0 of the suggested events overlap a class | Script over 200 seeded events and 3 student schedules | REQ-10 |
| M-06 | Event listed after posting | 5 s or less | Stopwatch, 10 posts | REQ-15 |
| M-07 | Notice delivery time | 60 s or less, 100% of active phones, over 10 events | Phone clocks and the job row's created_at and finished_at | REQ-17, REQ-18 |
| M-08 | Partial writes after a forced failure | 0 rows | Row counts after failure injection on each SQL function | NFR-01 |
| M-09 | Rows left after account deletion | 0 in 5 tables | Row counts after DELETE /v1/me | NFR-03 |
| M-10 | Secrets found in the repository and git history | 0 | Secret scanner [pick tool] | NFR-04 |
| M-11 | Requests without a valid token that get 401, and non-UC emails that get 403 | 100% | Script of 20 requests each | NFR-04, NFR-09 |
| M-12 | Personal data in log lines | 0 matches | Search logs for the seeded names, emails, and tokens | NFR-02 |
| M-13 | Screen fields with a label, read in order | 100% on every schedule and event screen, buttons 44 by 44 points or more | Manual VoiceOver and TalkBack run | NFR-10 |
| M-14 | Student volunteers who find at least one suggested event useful | 4 of 5 | Short survey after the volunteer session | US-02 (validation) |
| M-15 | Monthly cost | $7 or less | Cost inventory | NFR-07 |
 
## 4. Test Cases
 
Status values are Pass, Fail, Blocked, or Not Run. Nothing has been run yet, so every case is Not Run. The sample files (S1 to S3 and the answer key) and the 200 seeded events live in tests/data.
 
| ID | Description | Steps | Expected Result | Status |
| -- | ----------- | ----- | --------------- | ------ |
| TC-001 | REQ-01 (C1; Kaden): Boundary: a PDF of exactly 10,485,760 bytes is accepted | 1. Make a valid schedule PDF padded to 10,485,760 bytes. 2. Sign in as a consented test student. 3. POST it to /v1/schedule/import. | 200 with drafts; no 413. | Not Run |
| TC-002 | REQ-01 (C1; Kaden): Boundary: a file of 10,485,761 bytes is rejected | 1. Pad the same PDF by 1 byte. 2. POST it. 3. GET /v1/schedule/courses. | 413 file_too_large; the saved course count is unchanged. | Not Run |
| TC-003 | REQ-01 (C1; Kaden): Failure: a JPEG renamed schedule.pdf | 1. Rename a JPEG to schedule.pdf. 2. POST it. 3. GET /v1/schedule/courses. | 415 not_a_pdf; 0 courses saved. | Not Run |
| TC-004 | REQ-02 (C1; Kaden): Happy path: 5-course sample PDF (AC-01.1) | 1. Upload sample PDF S1 (5 courses, answer key in tests/data). 2. Time the response. 3. Compare each field with the key. | 5 drafts; name, professor, building, room, and time match the key; response in 10 s or less. | Not Run |
| TC-005 | REQ-02 (C1; Kaden): Edge: a course whose building text matches nothing (EC-07) | 1. Upload PDF S2 with 'Online' as the room. 2. Read missing_fields. 3. Open the review screen. | building_id is null and listed in missing_fields; the app highlights it. | Not Run |
| TC-006 | REQ-02 (C1; Kaden): Accuracy: 5 real UC schedules (GAP-9) | 1. Collect 5 schedule PDFs from team members and volunteers with consent. 2. Import each. 3. Count fields that match the printed schedule. | At least 90% of fields match; every miss is in missing_fields. | Not Run |
| TC-007 | REQ-03 (C1; Kaden): Happy path: confirm 5 drafts (AC-01.2) | 1. Import S1. 2. Edit one room. 3. Tap Confirm. 4. Time it. | PUT returns saved_count 5; success message and the schedule show in 5 s or less; the edited room is saved. | Not Run |
| TC-008 | REQ-03 (C1; Kaden): Cancel: drafts are dropped (AC-01.3) | 1. Import S1. 2. Tap Cancel. 3. GET /v1/schedule/courses. | 0 of the 5 drafts saved. | Not Run |
| TC-009 | REQ-03 (C1; Kaden): Failure: end time before start time | 1. Import S1. 2. Set one end time before its start. 3. Tap Confirm. | 422 invalid_course with field courses[n].end_time; the field is highlighted; nothing saved. | Not Run |
| TC-010 | REQ-04 (C1; Kaden): No readable course (AC-01.4) | 1. Upload a valid PDF with no schedule (S3). 2. Time the response. | 200 with no_courses_found true in 10 s or less; the app offers manual entry; 0 saved. | Not Run |
| TC-011 | REQ-04 (C1; Kaden): Failure: password-protected PDF (EC-06) | 1. Upload an encrypted PDF. | 422 unreadable_pdf; manual entry offered; 0 saved; no PDF text in the log. | Not Run |
| TC-012 | REQ-04 (C1; Kaden): Timing: reading takes over 8 s (EC-03) | 1. Set the test hook that delays parse_schedule() by 9 s. 2. Upload S1. 3. Time the response. | 504 extraction_timeout at about 8 s; manual entry offered; total 10 s or less. | Not Run |
| TC-013 | REQ-06 (C1, G1; Kaden): Unconsented student cannot save | 1. Create a student with consented_at empty. 2. PUT /v1/schedule/courses with 1 valid course. | 403 consent_required; 0 course rows. | Not Run |
| TC-014 | REQ-06 (C1, G1; Kaden): Every schedule endpoint checks consent | 1. With the same student call import, GET and PUT courses, routes, suggested, and PUT interests. | Each returns 403 consent_required; 0 rows written. | Not Run |
| TC-015 | REQ-06 (C1, G1; Kaden): Timing: consent withdrawn on device B during a save on device A (EC-04) | 1. Sign in on two devices. 2. Import on A. 3. Withdraw consent on B. 4. Confirm on A. | 403 consent_required from replace_courses; 0 courses saved. | Not Run |
| TC-016 | REQ-07 (C1, C5; Kaden): Boundary: 0 and 20 interests | 1. PUT category_ids [] then 20 different valid IDs. 2. GET /v1/me. | Both return 200; the saved list matches. | Not Run |
| TC-017 | REQ-07 (C1, C5; Kaden): Boundary: 21 interests | 1. PUT 21 different valid IDs. | 422; the saved list is unchanged. | Not Run |
| TC-018 | REQ-07 (C1, C5; Kaden): Failure: unknown category | 1. PUT [1, 9999]. | 422 unknown_category; the saved list is unchanged. | Not Run |
| TC-019 | REQ-08 (C2; Kaden): Happy path: one leg between two buildings (AC US-05) | 1. Save 2 back-to-back classes in different buildings. 2. GET /v1/schedule/routes for that date. 3. Walk the route with a timer. | One leg with integer walk_minutes and distance_m; the walk is within 2 minutes of the timed walk. | Not Run |
| TC-020 | REQ-08 (C2; Kaden): Statuses: tight and same_building | 1. Save classes with a 3-minute gap and a 12-minute walk. 2. Save two classes in one building. 3. GET routes. | tight for the first; same_building with walk 0 for the second. | Not Run |
| TC-021 | REQ-08 (C2; Kaden): Failure: walk table not loaded (EC-14) | 1. Start the server with an empty walk_time table. 2. GET routes. 3. Open the schedule in the app. | 503 routes_unavailable; the schedule still shows without walks. A course with no building returns unknown_building. | Not Run |
| TC-022 | REQ-10 (C3; Kaden): Happy path: matching, non-conflicting events | 1. Seed 200 events and 1 student with 30 class meetings and 3 interests. 2. GET /v1/events/suggested. 3. Check each result with a script. | Every event shares a category; 0 overlap a class; results ranked by matches, start time, event_id. | Not Run |
| TC-023 | REQ-10 (C3; Kaden): Boundary: margin of 5 minutes | 1. Event starts 3 minutes after a class ends in the same building. 2. Event starts walk + 5 minutes after a class in another building. | The first is not suggested; the second is. | Not Run |
| TC-024 | REQ-10 (C3; Kaden): Fallback and failure (EC-17, EC-20) | 1. Remove one walk row and rerun. 2. Request a 15-day window. 3. Use a student with no interests. | 15 minutes replaces walk + 5; 422 invalid_window; empty_reason no_interests. | Not Run |
| TC-025 | REQ-11 (C6; Laila): Happy path: list and paging | 1. Seed 120 published events. 2. GET /v1/events with no parameters. 3. Follow next_offset. | 50 per page, sorted by start time then event_id; only published events. | Not Run |
| TC-026 | REQ-11 (C6; Laila): Cancelled events (EC-19) | 1. Cancel one event. 2. GET /v1/events. 3. GET /v1/events/{id}. | Absent from the list; the detail call returns status cancelled. | Not Run |
| TC-027 | REQ-11 (C6; Laila): Failure: bad filter and window | 1. GET with category_id 9999. 2. GET with a 15-day window. 3. GET with category_id of Religious. | 422 unknown_category; 422 invalid_window; only Religious events. | Not Run |
| TC-028 | REQ-12 (C4, A2; Laila): Happy path: request, then approve | 1. Signed-in student POSTs a valid request. 2. Admin opens the portal list. 3. Admin approves. | 201 pending; the request is listed; after approval the club is verified, the student is head, and categories are copied. | Not Run |
| TC-029 | REQ-12 (C4, A2; Laila): Boundary and duplicate (EC-23) | 1. POST a request with a 19-character description. 2. POST the same club name twice. | 422 invalid_request; second request 409 request_pending. | Not Run |
| TC-030 | REQ-12 (C4, A2; Laila): Timing: two admins decide at once (EC-24); deny needs a note | 1. Two admins approve the same request together. 2. Deny another request with no note. | One 200 and one 409 already_decided; 422 note_required. | Not Run |
| TC-031 | REQ-15 (C6; Laila): Happy path: post and list (AC-02.1 list) | 1. Verified head POSTs a valid event. 2. Time until GET /v1/events shows it. | 201 published; listed in 5 s or less. | Not Run |
| TC-032 | REQ-15 (C6; Laila): Duplicate post (EC-35) | 1. POST the same name and start time twice in a row. | One 201 and one 409 duplicate_event; 1 event row and 1 notification_job. | Not Run |
| TC-033 | REQ-15 (C6; Laila): Boundary: end time (EC-33) | 1. POST with ends_at exactly 24 hours after starts_at. 2. Repeat with 24 hours + 1 minute. 3. Repeat with starts_at equal to now. | Accepted; 422 invalid_time_range; 422 start_in_past. | Not Run |
| TC-034 | REQ-16 (C6; Laila): Missing location (AC-02.3, EC-36) | 1. POST an event without building_id. 2. Count events and jobs. 3. Check the form. | 422 missing_field with field event.building_id; 0 events; 0 jobs; the field is highlighted. | Not Run |
| TC-035 | REQ-16 (C6; Laila): Missing name and description | 1. POST with an empty name and description. | 422 naming each field; 0 events; 0 notices. | Not Run |
| TC-036 | REQ-16 (C6; Laila): Failure: unknown building and database down (EC-37) | 1. POST with building_id 9999. 2. Stop the test database and POST a valid event. | 422 unknown_building; 503 database_unavailable; 0 events and 0 jobs. | Not Run |
| TC-037 | REQ-17 (C7, G2; Laila): Happy path: notice within 60 s (AC-02.1) | 1. Three students with the event's category each register a phone (1 iPhone, 1 Android, 1 simulator). 2. Post the event. 3. Time each arrival. | Every phone gets the notice in 60 s or less; the text holds only the event name, club, time, and place. | Not Run |
| TC-038 | REQ-17 (C7, G2; Laila): Retry (EC-52) | 1. Stub Expo to answer 429 twice, then 200. 2. Post an event. 3. Read the job row. | Attempts at about 5 s and 10 s, then sent; delivered in 60 s or less. | Not Run |
| TC-039 | REQ-17 (C7, G2; Laila): Non-recipients (EC-38, EC-55) | 1. Post an event no student's interests match. 2. Post one whose only matching student has a disabled token. | 0 notices; the job ends skipped. | Not Run |
| TC-040 | REQ-18 (C6, C7; Laila): Happy path: cancel and notify (AC-02.2) | 1. Interested students hold a published event. 2. Head confirms Cancel. 3. Check the lists and phones. | Status cancelled; gone from the list and suggestions; cancel notice on each phone in 60 s or less. | Not Run |
| TC-041 | REQ-18 (C6, C7; Laila): Failure: after the start, or twice (EC-34) | 1. Cancel an event that has started. 2. Cancel an event twice. | 409 event_already_started; 409 already_cancelled. | Not Run |
| TC-042 | REQ-18 (C6, C7; Laila): Timing: cancel before the publish notice is sent (EC-40) | 1. Stop the worker. 2. Post, then cancel. 3. Start the worker. | Both jobs end skipped; 0 notices. | Not Run |
| TC-043 | REQ-20 (G2; Laila): Register a token | 1. POST /v1/devices with a valid token. 2. POST one in the wrong format. | 204; 422 invalid_push_token. | Not Run |
| TC-044 | REQ-20 (G2; Laila): Sign-out removes the token | 1. Sign out in the app. 2. Post a matching event. | DELETE /v1/devices returns 204; that phone gets 0 notices; an unknown token also returns 204. | Not Run |
| TC-045 | REQ-20 (G2; Laila): Token moves; dead phone (EC-53, EC-55) | 1. Student B signs in on student A's phone. 2. Stub Expo to answer DeviceNotRegistered. | The token belongs to B and A gets 0 notices; disabled_at is set on the dead token. | Not Run |
| TC-046 | NFR-01 (C1; Kaden): Withdraw consent deletes data | 1. Student with 5 courses and 3 interests. 2. DELETE /v1/me/consent. 3. Count rows. | 204; 0 courses and 0 interests remain. | Not Run |
| TC-047 | NFR-01 (C1; Kaden): consented_at is recorded before any write | 1. Inspect replace_courses. 2. Try saving as a student with no consent. | The function checks consented_at inside its transaction; no write occurs without it. | Not Run |
| TC-048 | NFR-01 (C1; Kaden): Failure: database error during withdrawal (EC-09) | 1. Force an error mid-function. 2. Check rows. 3. Retry. | 0 partial changes; the retry succeeds. | Not Run |
| TC-049 | NFR-02 (G4, A1, G2; Laila): Logs hold no personal data (EC-63) | 1. Import a PDF, post an event, and register a token. 2. Search the log for course names, emails, and tokens. | 0 matches; the unit test fails if a schedule field is logged. | Not Run |
| TC-050 | NFR-02 (G4, A1, G2; Laila): The phone stores no schedule | 1. Use the app, then inspect device storage and cache. | Only the login session, push token, and public map tiles are found. | Not Run |
| TC-051 | NFR-02 (G4, A1, G2; Laila): Jobs and notices (EC-42) | 1. Inspect a notification_job row and a delivered notice. 2. Age a finished job to 31 days and run the daily cleanup. | No recipient list; the notice holds only public event information; the old job is deleted. | Not Run |
| TC-052 | NFR-03 (C1; Kaden): Delete an account with data | 1. Student with courses, interests, a device, and a club request. 2. DELETE /v1/me. 3. Count rows and try to sign in. | 204; 0 rows in student, course, student_interest, push_device, and club_request; sign-in fails. | Not Run |
| TC-053 | NFR-03 (C1; Kaden): Delete a club head | 1. Head of a club with 2 upcoming events. 2. DELETE /v1/me. | Both events cancelled and cancel jobs queued before the rows are removed. | Not Run |
| TC-054 | NFR-03 (C1; Kaden): Failure: login deletion fails once (EC-10) | 1. Stub Supabase Auth to fail once. 2. DELETE /v1/me twice. | 503 then 204; no orphan rows. | Not Run |
| TC-055 | NFR-04 (G1, G3; Kaden): No secrets in the repository | 1. Run a secret scanner on the repository and its full git history. 2. Check the host settings. | 0 findings; the secret key exists only in the host settings. | Not Run |
| TC-056 | NFR-04 (G1, G3; Kaden): Row-level security | 1. For each table, query it with the public key from a script. 2. Query a student's course with another student's token. | Every direct read or write is refused; no other student's data returns. | Not Run |
| TC-057 | NFR-04 (G1, G3; Kaden): Access checks | 1. Call endpoints with no token. 2. A student calls /v1/admin/.... 3. A non-head calls a club's endpoints. 4. Call over plain HTTP. | 401 not_signed_in; 403 not_admin; 403 not_club_head; HTTP refused. | Not Run |
| TC-058 | NFR-05 (E1, C2, A1; Kaden): No third-party routing | 1. Capture network traffic while importing a schedule and opening routes. | 0 requests carry class locations to an outside routing service. | Not Run |
| TC-059 | NFR-05 (E1, C2, A1; Kaden): Credit on every map | 1. Open the route map in A1 and A2. | '© OpenStreetMap contributors' is visible on both. | Not Run |
| TC-060 | NFR-05 (E1, C2, A1; Kaden): Open data source | 1. Run import_map(). 2. Read the ImportReport and the source of the data. | Buildings and paths come from OpenStreetMap; 62 buildings loaded. | Not Run |
| TC-061 | NFR-07 (All; Laila): Cost inventory | 1. List every service with its plan and monthly price. | Every service is free or already paid for; the host is $7 a month or less. | Not Run |
| TC-062 | NFR-07 (All; Laila): Apple and Expo | 1. Confirm the Apple Developer Program membership is active. 2. Confirm Expo sending is free. | No new fee. | Not Run |
| TC-063 | NFR-07 (All; Laila): Host does not sleep (Q1) | 1. Leave the staging host idle for 30 minutes. 2. Send a request. | Reply in 2 s or less, with no 30 to 60 s wake-up. | Not Run |
| TC-064 | NFR-09 (G1, A1, A2; Kaden): App refuses other domains | 1. Try to sign up with a gmail.com address in A1 and A2. | Refused before calling Supabase. | Not Run |
| TC-065 | NFR-09 (G1, A1, A2; Kaden): Unconfirmed email | 1. Sign up with a UC address and do not confirm. 2. Call GET /v1/me with its token. | 403 uc_email_required. | Not Run |
| TC-066 | NFR-09 (G1, A1, A2; Kaden): Domain check is exact (EC-50) | 1. Use a confirmed token for x@mail.uc.edu and one for x@mail.uc.edu.evil.com. | The first is allowed; the second gets 403 uc_email_required. | Not Run |
| TC-067 | NFR-10 (A1; Kaden): VoiceOver on iPhone | 1. Turn on VoiceOver. 2. Walk through sign-in, consent, review, Today, For you, All events, and Settings. | Every schedule and event field has a label and is read in visual order. | Not Run |
| TC-068 | NFR-10 (A1; Kaden): TalkBack on Android | 1. Repeat on an Android phone with TalkBack. | Same result as the iPhone. | Not Run |
| TC-069 | NFR-10 (A1; Kaden): Touch targets, times, and color | 1. Measure each button. 2. Listen to the times. 3. View each screen in grayscale. | Buttons at least 44 by 44 points; times read as words ('6 PM to 8 PM'); nothing is shown by color alone. | Not Run |

 
## 5. Test Environment
 
| | Development | Staging | Pilot |
| - | ----------- | ------- | ----- |
| Purpose | Unit and integration tests while building | System tests, demos, and the timing measurements | Volunteer and club head sessions |
| Server | Laptop, Python and FastAPI, local worker | The chosen host (Q1), always awake, in Supabase's region | Same host as staging |
| Database | A separate Supabase test project, reset before each run | A separate Supabase project with the seed data below | The staging project, with real volunteer data (consent first) |
| Devices | iOS simulator, Android emulator, Chrome [versions to confirm] | 1 iPhone and 1 Android phone (development build, not Expo Go), Chrome and Safari for the portal [models and versions to confirm] | Volunteers' own phones |
| Network | Office or home Wi-Fi | Wi-Fi, plus one run on cellular and one offline run | Their own networks |
| Expo | Stubbed (429, 5xx, DeviceNotRegistered) | The real Expo Push Service | The real Expo Push Service |
 
**Seed data.** 62 Uptown West buildings from uptown_west_buildings.csv and a walk table from import_map(); the 23 GetInvolvedUC categories; 5 sample schedule PDFs (S1 to S3 and 2 more) with answer keys; 200 published events across the categories; test accounts: 3 students, 2 club heads, 1 admin, each with a confirmed UC address [mailbox to confirm]. **Configuration.** Settings as in D2 2.12, with a test hook that delays parse_schedule() and a way to stop the worker. Real student data never goes into development or staging.
 
## 6. Pass and Fail Criteria
- **A test passes** when every number or status in its Expected Result is met exactly. It fails when any value is wrong, a limit in section 3 is missed, or a step needs a workaround. It is Blocked when a prerequisite is missing, such as a host or a phone; a Blocked case is retried within a week or logged as a defect.
- **A requirement passes** when all its cases pass and every metric linked to it in section 3 meets its target. Timing metrics are measured over the run counts shown, and one miss fails that metric.
- **The plan passes** when: 100% of the 69 Must cases pass; no open defect of severity High (data loss, a schedule shown to another user, a missed notice budget, or a crash in the main flow); M-10, M-12, and M-09 are exactly 0; and M-14 is 4 of 5 or better.
- **The plan fails** if any Must case fails at the feature freeze (IM-08) with no accepted waiver from both members and the advisor.


## 7. Schedule and Evidence
 
| Milestone (Week 8 plan) | Date | Test activity | Owner | Evidence |
| ----------------------- | ---- | ------------- | ----- | -------- |
| IM-01 Foundation ready | 2027-01-17 | Unit tests for settings and logging; row-level security and migration checks | Kaden (tables, security), Laila (config, logging) | CI results; TC for NFR-02 and NFR-04 |
| IM-02 Sign-in and consent | 2027-01-31 | Integration: auth, consent, deletion, UC email | Kaden | Test log for NFR-01, NFR-03, NFR-09, REQ-06 |
| IM-03 Import and club request | 2027-02-07 | Unit and integration: PDF reader, import, club request | Kaden, Laila | REQ-01 to REQ-04, REQ-12 results; M-01, M-02 |
| IM-04 Event posting and categories | 2027-02-14 | Integration: save_event, publish_event, categories | Laila | REQ-15 and REQ-16 results; M-06 |
| IM-05 Backend demo (MAJOR) | 2027-02-28 | System: routes, suggestions, notices on real phones; advisor demo | Both | REQ-08, REQ-10, REQ-11, REQ-17, REQ-18, REQ-20 results; M-04 to M-07; advisor notes |
| IM-06 Deployed over HTTPS | 2027-03-07 | Staging: host does not sleep, TLS, secrets scan | Laila | NFR-04, NFR-07 results; M-10 |
| IM-07 Apps feature complete; I6 validated | 2027-04-04 | System: VoiceOver, TalkBack, portal, end-to-end I1 to I4 and I6; volunteer sessions | Kaden (apps), Laila (portal) | NFR-10 results; M-13, M-14 |
| IM-08 Feature freeze (MAJOR) | 2027-04-11 | Regression of all 69 cases; edge-case pass; TC swap with another team | Both | Completed Status column; defect list; matrix updated |
| IM-09 Showcase | 2027-04-13 | Smoke test of the 10 core cases on the demo phones | Both | Checklist signed |

 
## 8. Definition of Done
 
**A feature is done when:**
- [ ] Its requirement's TC cases pass (Status updated in section 4) and its unit tests run in CI.
- [ ] Every error code in D1 and D2 for that feature has a test.
- [ ] Another team member has reviewed the code and the steps in its TC cases.
- [ ] No log line holds a course name, email, or push token.
- [ ] The README, API description, and Technical Specification match the code.
- [ ] It has no open High defect.

**The project is done when:**
- [ ] All 69 cases are run, 100% of Must cases pass, and the matrix Status column says Tested.
- [ ] Every metric in section 3 has a measured value recorded.
- [ ] The secret scan, row-level security check, and deletion check are clean.
- [ ] The advisor has seen the demo and recorded feedback, and both members have signed off in the notebook.
- [ ] Open defects are listed with a decision for each (fix, accept, or move to Won't).
- [ ] The Requirements Document, Technical Specification, Test Plan, and README are at matching versions.