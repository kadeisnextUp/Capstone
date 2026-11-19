# Project Plan
**Team members:** Kaden Sawyer, Laila Raines
**Plan date:** 2026-11-18 (draft)
 
**Planning basis**
 
- Implementation window: Monday 2027-01-11 to the showcase on Tuesday 2027-04-13. Winter-break pre-work is planned from 2026-12-07.
- Capacity (to be confirmed by both members): 10 hours per week each during the semester, 4 hours in the week of 2027-02-22 (midterms), 0 in spring break (2027-03-15 to 2027-03-21), and 3 hours per week each in the five weeks of winter break (2026-12-07 to 2027-01-10). MLK Day (2027-01-18) is a day off, but the weekly meeting still happens that week.
- Hours available for tasks: 106.0 in the semester and 15.0 in winter break, 121.0 per member. Protected blocks take 4 (PB1), 4 (PB2), and 6 (PB3) hours per member on top of that.
- Scheduling: tasks were placed week by week, earliest dependency first, with 15% of each member's hours held back for pairing and review on shared tasks. A task may start in the week its predecessor finishes. Start and completion dates are the first and last day of those weeks.



## 1. Task List
1. Specify the schedule PDF reading rules and collect sample UC schedule PDFs for the C1 Information Check module (I1). Owner: Kaden
2. Develop the schedule PDF reader and the POST /v1/schedule/import endpoint for the C1 Information Check module. Owner: Kaden
3. Develop the schedule save endpoint, the building search, and manual course entry for the C1 Information Check module. Owner: Kaden
4. Research open source mapping and routing options and select one for the E1 Open Map Data external system. Owner: Kaden
5. Obtain and validate UC campus path data from E1 Open Map Data and build the walk table for the C2 Route Planner module. Owner: Kaden
6. Develop the route calculation and the GET /v1/schedule/routes endpoint for the C2 Route Planner module. Owner: Kaden
7. Develop the matching of events against the free time in a student's schedule and the GET /v1/events/suggested endpoint for the C3 Event Check module. Owner: Kaden
8. Develop student sign-in, the consent screen, and account deletion with Supabase Auth for the G1 Authentication component. Owner: Kaden
9. Develop configuration management and secrets handling, keeping secrets out of source control, for the G3 Configuration component. Owner: Laila
10. Develop the schedule import, review, and manual entry screens of the A1 Mobile App. Owner: Kaden
11. Develop the route map and suggested event screens of the A1 Mobile App. Owner: Kaden
12. Test the schedule, map, and event screens of the A1 Mobile App with VoiceOver and TalkBack. Owner: Kaden
13. Develop the Supabase PostgreSQL tables for the student side (student, course, building, walk_time, and student_interest) and enable row level security for the D1 Data Model. Owner: Kaden
14. Develop the Supabase PostgreSQL tables for the club side (club, event, category, event_category, club_request, and notification_job) and enable row level security for the D1 Data Model. Owner: Laila
15. Develop club request submission and the existence check against E2 Campus Links for the C4 Club Check module. Owner: Laila
16. Design the 23 GetInvolvedUC category list and develop the categorization of clubs and events for the C5 Club Categories module. Owner: Laila
17. Develop the classification of submitted events and the event posting and cancelling endpoints (save_event) for the C6 Event Classifier module. Owner: Laila
18. Develop the matching of posted events to interested students and the creation of notification jobs for the C7 Interest Finder module. Owner: Laila
19. Develop the notification worker and Expo push delivery of published and cancelled events for the G2 Notifications component. Owner: Laila
20. Develop logging that keeps course names, emails, and push tokens out of log lines for the G4 Logging component. Owner: Laila
21. Develop the React and Vite admin portal for approving, denying, and revoking club requests in the A2 Web App and Admin Portal. Owner: Laila
22. Develop the club head pages for requesting a club and for posting and cancelling events in the A2 Web App and Admin Portal. Owner: Laila
23. Document and deploy the FastAPI server and Supabase project to a hosting provider for the D1 Deployment. Owner: Laila
24. Validate the student flow from I1 through I4 and the I6 interface between the student view and club view with end to end testing. Owner: Laila
25. Test the D2 edge cases against the edge-case table and the traceability matrix. Owner: Laila
 

## 2. Milestones List
- Major milestones are marked MAJOR
 
| ID | Milestone | What it demonstrates | Date | How it is verified | Verified by | Status |
| -- | --------- | -------------------- | ---- | ------------------ | ----------- | ------ |
| C-01 | Design phase documents: User Stories, D0, D1, D2, constraints essay, peer review notes | The design that the task list and this plan are built from | Before 2026-11-16 (exact due dates to be added from the Week 1 calendar) | Graded submissions; files present in the Design_Diagrams folder of the repository | Instructor | Complete |
| C-02 | Assignment 8: Task List, Milestones, Timeline, and Effort Matrix (this package) | A dated, owned plan from the finished design to the showcase | Due date was 2026-10-28 | One PDF with the six components; Tasklist.md in the repository root; sources in the Planning folder | Laila and Kaden | Needs review |
| C-03 | Assignment 9: Requirements Document and Technical Specification | The REQ, NFR, and CON IDs that the test plan verifies, and the design they trace to (D0, D1, D2) | Due 2026-10-30 | Both PDFs submitted; both .md files committed; README section 3 updated; specification cites the requirements version | Laila and Kaden check each other's copy before submitting | Rebuilt 2026-11-18 from the finished D1 and D2; not yet submitted |
| C-04 | Assignment 10: Test and validation | A test plan with a test case for each requirement ID | Due 2026-11-04 | Test IDs match the traceability matrix | Laila and Kaden | Overdue, not yet submitted |
| C-05 | Assignment 11: Ethics and AI responsibility use | The project's privacy, security, licensing, attribution, and AI-use positions | 2026-11-11 | Submitted PDF; Ethics_Statement.md committed | Instructor | Complete |
| C-06 | Assignment 12: Presentation draft and advisor feedback | A presentable draft of the project that the advisor has reviewed | 2026-11-18 | Draft submitted; advisor comments recorded in the notebook | Professor Zhao (advisor); both members | Planned |
| C-07 | Assignment 13: Oral presentation and peer review | The team can explain and defend the design | 2026-11-18 | Presentation given; peer review forms received | Instructor and peers | Planned |
| C-08 | Assignment 15: Project advisor feedback | Advisor agreement with the design and this plan | 2026-11-25 | Advisor feedback recorded and any changes logged | Professor Zhao (advisor) | Planned |
| C-09 | Assignment 14: Design report and notebook | The full design, with Tables 1 and 2 reproduced | 2026-12-02 | PDF submitted; notebook up to date; tables match this plan | Laila and Kaden | Planned |
| C-10 | Assignment 16: Team survey | Each member's account of how the team worked | 2026-12-05 | Survey submitted by each member | Each member | Planned |
| C-11 | Implementation semester begins | Plan, Tasklist.md, and Planning folder are committed; winter-break pre-work (T13, T14, T1, T16, T20) is done | 2027-01-11 | Files present in the repository; migrations for both sides run on a fresh Supabase project | Kaden | Planned |
| IM-01 | Foundation ready | Database tables for both sides exist with row level security on; configuration and logging are in place; no secrets in the repository | 2027-01-17 | Migrations run on a fresh Supabase project; reading another student's row is refused; a search of the git history finds no secrets | Kaden checks the club tables, configuration, and logging; Laila checks the student tables | Planned |
| IM-02 | Sign-in, consent, and configuration working | A student cannot save a schedule before consenting; account deletion removes all student rows; campus paths are selected | 2027-01-31 | A new student is blocked from saving until consent is recorded (403 consent_required); a request without a token returns 401; deleting an account leaves 0 rows in the student tables | Laila verifies student sign-in and deletion; Kaden verifies the sign-in guard on club endpoints | Planned |
| IM-03 | Schedule import and club request working | A sample PDF with 5 courses reaches the review screen; a club request can be submitted and checked against Campus Links | 2027-02-07 | AC-01.1 and AC-01.4 pass on sample PDFs; a club request is stored with its existence check result | Laila verifies the import; Kaden verifies the club request | Planned |
| IM-04 | Event posting and categories working | The 23 categories are loaded; a verified club can post an event with 1 to 5 categories | 2027-02-14 | save_event rejects each error case in the D1 contract; a posted event shows its categories | Kaden verifies event posting; Laila verifies the categories | Planned |
| IM-05 | MAJOR: Backend demo to the advisor | Student and club flows work through the API: routes, event matching, and notifications delivered within 60 seconds | 2027-02-28 | Live demo with sample data; AC-02.1 timing measured on a physical phone; advisor feedback recorded | Professor Zhao (advisor); both members | Planned |
| IM-06 | Deployed system reachable over HTTPS | The hosted API and database answer from a phone over HTTPS, with secrets kept outside source control | 2027-03-07 | A request from a phone succeeds; TLS check passes; configuration review; the server does not sleep (AC-01.1) | Kaden verifies Laila's deployment | Planned |
| IM-07 | Student app and club web app feature complete; I6 validated | The mobile app and the web and admin portal are feature complete; the student and club views connect through I6 | 2027-04-04 | Every schedule and event field is read in order by VoiceOver and TalkBack; the I6 end-to-end test passes; an admin approves a request in the portal | Laila checks the app; Kaden checks the portal and the I6 test | Planned |
| IM-08 | MAJOR: Feature freeze with all tests passing | All 25 tasks are complete; edge-case tests pass; traceability matrix statuses and test IDs are updated; no open must-fix defects | 2027-04-11 | Test report; Requirements_Document.md matrix rows move from Designed to Tested | Laila and Kaden; advisor informed | Planned |
| IM-09 | MAJOR: Showcase | The working system shown live to the course audience | 2027-04-13 | Live demo; project deployment reachable | Laila and Kaden | Planned |
 
## 3. Timeline
 
| ID | Task | Owner | Start | Completion | Depends on | PERT h | Buffer h (17%) | Total h |
| -- | ---- | ----- | ----- | ---------- | ---------- | ------ | -------------- | ------- |
| T1 | Specify the schedule PDF reading rules and collect sample UC schedule PDFs for the C1 Information Check module (I1). | Kaden | 2026-12-14 | 2026-12-27 | None | 2.1 | 0.4 | 2.4 |
| T2 | Develop the schedule PDF reader and the POST /v1/schedule/import endpoint for the C1 Information Check module. | Kaden | 2026-12-21 | 2027-01-17 | T1, T13 | 12.7 | 2.2 | 14.8 |
| T3 | Develop the schedule save endpoint, the building search, and manual course entry for the C1 Information Check module. | Kaden | 2027-01-25 | 2027-02-07 | T2 | 6.3 | 1.1 | 7.4 |
| T4 | Research open source mapping and routing options and select one for the E1 Open Map Data external system. | Kaden | 2027-01-25 | 2027-01-31 | None | 2.0 | 0.3 | 2.3 |
| T5 | Obtain and validate UC campus path data from E1 Open Map Data and build the walk table for the C2 Route Planner module. | Kaden | 2027-02-01 | 2027-02-14 | T4, T13 | 8.5 | 1.4 | 9.9 |
| T6 | Develop the route calculation and the GET /v1/schedule/routes endpoint for the C2 Route Planner module. | Kaden | 2027-02-08 | 2027-02-21 | T3, T5 | 5.2 | 0.9 | 6.0 |
| T7 | Develop the matching of events against the free time in a student's schedule and the GET /v1/events/suggested endpoint for the C3 Event Check module. | Kaden | 2027-02-15 | 2027-02-28 | T3, T13, T14 | 8.3 | 1.4 | 9.8 |
| T8 | Develop student sign-in, the consent screen, and account deletion with Supabase Auth for the G1 Authentication component. | Kaden | 2027-01-11 | 2027-01-31 | T13 | 8.2 | 1.4 | 9.6 |
| T9 | Develop configuration management and secrets handling, keeping secrets out of source control, for the G3 Configuration component. | Laila | 2027-01-11 | 2027-01-17 | T13 | 4.2 | 0.7 | 4.9 |
| T10 | Develop the schedule import, review, and manual entry screens of the A1 Mobile App. | Kaden | 2027-02-22 | 2027-03-14 | T2, T3, T8 | 10.5 | 1.8 | 12.3 |
| T11 | Develop the route map and suggested event screens of the A1 Mobile App. | Kaden | 2027-03-08 | 2027-03-28 | T6, T7, T10 | 9.5 | 1.6 | 11.1 |
| T12 | Test the schedule, map, and event screens of the A1 Mobile App with VoiceOver and TalkBack. | Kaden | 2027-03-22 | 2027-04-04 | T11 | 3.2 | 0.5 | 3.7 |
| T13 | Develop the Supabase PostgreSQL tables for the student side (student, course, building, walk_time, and student_interest) and enable row level security for the D1 Data Model. | Kaden | 2026-12-07 | 2026-12-20 | None | 4.0 | 0.7 | 4.7 |
| T14 | Develop the Supabase PostgreSQL tables for the club side (club, event, category, event_category, club_request, and notification_job) and enable row level security for the D1 Data Model. | Laila | 2026-12-07 | 2026-12-20 | None | 4.0 | 0.7 | 4.7 |
| T15 | Develop club request submission and the existence check against E2 Campus Links for the C4 Club Check module. | Laila | 2027-01-25 | 2027-02-07 | T14, T8 | 7.3 | 1.2 | 8.6 |
| T16 | Design the 23 GetInvolvedUC category list and develop the categorization of clubs and events for the C5 Club Categories module. | Laila | 2026-12-14 | 2027-01-03 | T14 | 4.2 | 0.7 | 4.9 |
| T17 | Develop the classification of submitted events and the event posting and cancelling endpoints (save_event) for the C6 Event Classifier module. | Laila | 2027-02-01 | 2027-02-14 | T15, T16 | 8.3 | 1.4 | 9.8 |
| T18 | Develop the matching of posted events to interested students and the creation of notification jobs for the C7 Interest Finder module. | Laila | 2027-02-08 | 2027-02-21 | T17, T13 | 6.3 | 1.1 | 7.4 |
| T19 | Develop the notification worker and Expo push delivery of published and cancelled events for the G2 Notifications component. | Laila | 2027-02-15 | 2027-02-28 | T18 | 8.5 | 1.4 | 9.9 |
| T20 | Develop logging that keeps course names, emails, and push tokens out of log lines for the G4 Logging component. | Laila | 2026-12-28 | 2027-01-17 | None | 3.1 | 0.5 | 3.6 |
| T21 | Develop the React and Vite admin portal for approving, denying, and revoking club requests in the A2 Web App and Admin Portal. | Laila | 2027-03-01 | 2027-03-14 | T15, T8 | 9.5 | 1.6 | 11.1 |
| T22 | Develop the club head pages for requesting a club and for posting and cancelling events in the A2 Web App and Admin Portal. | Laila | 2027-03-08 | 2027-04-04 | T17, T21 | 8.3 | 1.4 | 9.8 |
| T23 | Document and deploy the FastAPI server and Supabase project to a hosting provider for the D1 Deployment. | Laila | 2027-02-22 | 2027-03-07 | T9, T3, T17 | 5.2 | 0.9 | 6.0 |
| T24 | Validate the student flow from I1 through I4 and the I6 interface between the student view and club view with end to end testing. | Laila | 2027-03-29 | 2027-04-04 | T12, T22, T19, T7, T6 | 4.0 | 0.7 | 4.7 |
| T25 | Test the D2 edge cases against the edge-case table and the traceability matrix. | Laila | 2027-03-29 | 2027-04-11 | T24, T19, T22 | 6.3 | 1.1 | 7.4 |
| | **Total (25 tasks)** | | | | | **159.7** | **27.1** | **186.8** |
 
**Protected blocks and calendar constraints**
 
| Item | Dates | Hours per member | Purpose |
| ---- | ----- | ---------------- | ------- |
| Winter break | 2026-12-06 to 2027-01-10 | 3 a week | Pre-work on T13, T14, T1, T16, T20, and the start of T2; the plan falls behind if this is not done |
| Midterm week | 2027-02-22 to 2027-02-28 | 4 (instead of 10) | T7 and T19 finish in this week; a lost week moves both |
| MLK Day | 2027-01-18 | 10 (meeting moves within the week) | Day off |
| Protected block PB1 | 2027-03-01 to 2027-03-14 | 4 | Fix and rehearse before spring break |
| Spring break | 2027-03-15 to 2027-03-21 | 0 | No planned work |
| Protected block PB2 | 2027-04-05 to 2027-04-11 | 4 | Fix defects before the feature freeze (IM-08) |
| Protected block PB3 | 2027-04-12 to 2027-04-13 | 6 | Rehearse and fix before the showcase (IM-09) |
 
**Milestone planned completion dates**
 
| Milestone | Planned completion |
| --------- | ------------------ |
| IM-01 Foundation ready | 2027-01-17 |
| IM-02 Sign-in, consent, and configuration working | 2027-01-31 |
| IM-03 Schedule import and club request working | 2027-02-07 |
| IM-04 Event posting and categories working | 2027-02-14 |
| IM-05 MAJOR: Backend demo to the advisor | 2027-02-28 |
| IM-06 Deployed system reachable over HTTPS | 2027-03-07 |
| IM-07 Student app and club web app feature complete; I6 validated | 2027-04-04 |
| IM-08 MAJOR: Feature freeze with all tests passing | 2027-04-11 |
| IM-09 MAJOR: Showcase | 2027-04-13 |
 
 
## 4. Effort Matrix 
- The task owner's column is marked with an *.
 
| ID | Task | Kaden (h) | Laila (h) | Task total (h) | Primary |
| -- | ---- | --------- | --------- | -------------- | ------- |
| T1 | Schedule PDF reading rules and sample PDFs (C1) | 2.1* | 0.0 | 2.1 | Kaden |
| T2 | Schedule PDF reader and import endpoint (C1) | 11.4* | 1.3 | 12.7 | Kaden |
| T3 | Schedule save, building search, manual entry (C1) | 6.3* | 0.0 | 6.3 | Kaden |
| T4 | Research and select open map data (E1, C2) | 2.0* | 0.0 | 2.0 | Kaden |
| T5 | Campus path data and walk table (E1, C2) | 8.5* | 0.0 | 8.5 | Kaden |
| T6 | Route calculation and route endpoint (C2) | 5.2* | 0.0 | 5.2 | Kaden |
| T7 | Event matching and suggested-events endpoint (C3) | 7.5* | 0.8 | 8.3 | Kaden |
| T8 | Sign-in, consent, and account deletion (G1) | 8.2* | 0.0 | 8.2 | Kaden |
| T9 | Configuration and secrets (G3) | 0.0 | 4.2* | 4.2 | Laila |
| T10 | Mobile import, review, and manual-entry screens (A1) | 10.5* | 0.0 | 10.5 | Kaden |
| T11 | Mobile route map and event list screens (A1) | 9.5* | 0.0 | 9.5 | Kaden |
| T12 | VoiceOver and TalkBack testing (A1) | 2.7* | 0.5 | 3.2 | Kaden |
| T13 | Student-side tables and row level security (D1 Data Model) | 4.0* | 0.0 | 4.0 | Kaden |
| T14 | Club-side tables and row level security (D1 Data Model) | 0.0 | 4.0* | 4.0 | Laila |
| T15 | Club request submission and existence check (C4, E2) | 0.0 | 7.3* | 7.3 | Laila |
| T16 | Category list and club categorization (C5) | 0.0 | 4.2* | 4.2 | Laila |
| T17 | Event classification and event posting endpoints (C6) | 0.0 | 8.3* | 8.3 | Laila |
| T18 | Interest matching and notification jobs (C7) | 0.0 | 6.3* | 6.3 | Laila |
| T19 | Notification worker and Expo push delivery (G2) | 0.0 | 8.5* | 8.5 | Laila |
| T20 | Logging without personal data (G4) | 0.0 | 3.1* | 3.1 | Laila |
| T21 | Admin portal for club requests (A2) | 1.0 | 8.6* | 9.5 | Laila |
| T22 | Club head web pages: request, post, cancel (A2) | 0.8 | 7.5* | 8.3 | Laila |
| T23 | Document and deploy the server and Supabase project (D1 Deployment) | 0.8 | 4.4* | 5.2 | Laila |
| T24 | Validate student flow I1 to I4 and the I6 interface end to end | 0.6 | 3.4* | 4.0 | Laila |
| T25 | Test D2 edge cases against the traceability matrix | 1.3 | 5.1* | 6.3 | Laila |
| | **Total (25 tasks)** | **82.3** | **77.4** | **159.7** | |
| | **Share of hours** | 52% | 48% | 100% | |
| | **Total with 17% buffer** | 96.3 | 90.6 | 186.8 | |
 
**Capacity check at 10 hours per week (plus 3 hours per week in winter break)**
 
| | Kaden | Laila |
| - | ----- | ----- |
| Task hours with buffer | 96.3 | 90.6 |
| Hours available for tasks | 121.0 | 121.0 |
| Unallocated | 24.7 | 30.4 |
| Protected blocks PB1 + PB2 + PB3 (on top) | 14 | 14 |
 
**Week-by-week check.** Each member's hours were placed into weeks with the dependency order of Table 1, after setting aside the protected blocks, the reduced midterm week, and spring break. Every task finishes by its Table 1 date. The unallocated hours are not freely usable: most of Laila's sit in the weeks of 2027-01-11 and 2027-01-18, when her tasks wait on Kaden's sign-in work (T8), and the rest sit in the pairing reserve. Kaden has less than 1 hour spare in the weeks of 2027-02-22, 2027-03-01, 2027-03-08, 2027-03-22. Laila has less than 1 hour spare in the weeks of 2027-02-15, 2027-02-22. A missed week in those weeks moves a task. The smallest weekly figure that still finished every task was 10 hours with the winter pre-work, or about 11.5 hours a week without it.
 
**If the team keeps 5 hours a week.** At 5 hours a week (2 in the week of 2027-02-22, none in spring break) each member has 53 hours for tasks, 106 for the team, against 186.8 needed, so about 57% of the work fits. Dropping the club category and classification tasks and both web portals (T16, T17, T21, T22: 35.5 hours with buffer) would still leave about 45 hours over. A 5-hour plan therefore needs a scope decision with the advisor, such as using the Supabase dashboard instead of the portals (as version 1 did) and deferring notifications. This plan does not make that cut on its own.
 
**Balance.** The columns differ by 4.9 hours of estimate. If Kaden falls behind, Laila takes over T12 first, since she already shares it, and Kaden's protected block PB3 is the first thing to shrink.
 

## 5. Work Breakdown Structure
- **1. Foundation**
  - 1.1 Data (D1 Data Model): T13 student-side tables, T14 club-side tables
  - 1.2 Configuration and logging (G3 Configuration, G4 Logging): T9 configuration and secrets, T20 logging
  - 1.3 Authentication (G1 Authentication): T8 sign-in, consent, and account deletion
- **2. Student side**
  - 2.1 Schedule import (C1 Information Check): T1 PDF reading rules, T2 PDF reader and import endpoint, T3 save, building search, and manual entry
  - 2.2 Routes and map (E1 Open Map Data, C2 Route Planner): T4 mapping research, T5 campus path data and walk table, T6 route calculation and endpoint
  - 2.3 Event matching (C3 Event Check): T7 matching and suggested-events endpoint
  - 2.4 Mobile app (A1): T10 import and review screens, T11 route map and event screens
- **3. Club side**
  - 3.1 Club requests (C4 Club Check, E2 Campus Links): T15 request submission and existence check
  - 3.2 Categories and classification (C5, C6): T16 category list and categorization, T17 event classification and posting endpoints
  - 3.3 Matching and notifications (C7 Interest Finder, G2 Notifications): T18 interest matching and notification jobs, T19 notification worker and Expo push
  - 3.4 Web app and admin portal (A2): T21 admin portal, T22 club head pages
- **4. Verification and release**
  - 4.1 Testing: T12 VoiceOver and TalkBack test, T24 student flow and I6 end-to-end test, T25 D2 edge-case test
  - 4.2 Deployment (D1 Deployment): T23 document and deploy


## 6. Risk Register
  
| ID | Category | Risk | Likelihood | Impact | Strategy | Mitigation | Owner | Trigger | Where it shows in Table 1 |
| -- | -------- | ---- | ---------- | ------ | -------- | ---------- | ----- | ------- | ------------------------- |
| R-01 | Team | The work does not fit the available hours: the plan needs 186.8 hours with buffer (96.3 Kaden, 90.6 Laila) against 121.0 hours each only if each member works 10 hours a week and 3 in winter break; at 5 hours a week about 57% fits. | High | High | Reduce | Confirm the weekly hours now and each member logs hours weekly. If hours stay at 5, agree a scope cut with the advisor in the Assignment 15 feedback (portals first, then notifications). | Laila | A member logs fewer than 8 hours in a semester week outside 2027-02-22, or any task is more than 1 week behind Table 1 | Every row; protected blocks PB1 to PB3 |
| R-02 | Team | The winter-break pre-work (3 hours a week for five weeks) does not happen, which pushes T1, T2, T13, T14, T16, and T20 into January and delays everything that depends on them. | Medium | High | Reduce | Agree the pre-work at the Assignment 14 meeting; T13 and T14 are small and first; if skipped, drop T16 and T17 to the Supabase dashboard first. | Kaden | T13 and T14 are not done by 2027-01-10 | T13, T14, T1, T16, T20 (2026-12-07 to 2027-01-17) |
| R-03 | Team | Exams and other coursework cut into project weeks: the week of 2027-02-22 is planned at 4 hours each, and other exam weeks are not in the calendar. | Medium | Medium | Reduce | T7 and T19 finish in the week of 2027-02-22, so a lost week moves both; other exam weeks will be added once published and the 17% buffer absorbs short weeks. | Kaden | Either member has a week with fewer than 4 hours, or another exam week is announced | T7, T19 (week of 2027-02-22) buffer column |
| R-04 | Technical | UC schedule PDFs vary in layout, so the reader misses fields (D2 GAP-9: the rules are untested on real UC schedules) and T2 runs long. T3, T10, and T11 depend on T2. | Medium | High | Reduce | Collect at least 5 real UC schedule PDFs in T1; the review and edit screen covers missed fields; manual entry is always available (AC-01.4). | Kaden | The reader extracts fewer than 80% of fields on the sample PDFs by 2027-01-10 | T1, T2 (2026-12-14 to 2027-01-17) |
| R-05 | Technical | OpenStreetMap is missing campus paths, so walking times are wrong. | Medium | Medium | Reduce | T5 checks 10 building pairs against timed walks and adds missing paths by hand. | Kaden | More than 2 of the 10 sample routes are off by more than 2 minutes at the end of T5 | T5 (2027-02-01 to 2027-02-14) |
| R-06 | Technical | Push notifications do not reach a real phone within 60 seconds, or Expo delivery failures go unseen (D2 GAP-1: receipts are not read). | Medium | High | Reduce | Test on a physical phone at the start of T19 and read delivery receipts in the worker. Have an in-app list as the fallback. | Laila | No push reaches a physical phone by 2027-02-14 | T19 (2027-02-15 to 2027-02-28) |
| R-07 | External | The server host is not chosen (D2 Q1). A host that sleeps breaks the 10-second schedule budget, and free tiers or a Supabase pause could interrupt the demo. | Medium | Medium | Reduce | Choose the host at the start of T23 and keep the server awake (about $7 a month). Export the schema and sample data regularly. | Laila | No host chosen by 2027-02-21 | T23 |
| R-08 | Technical | The student side and club side disagree on the event data format, which shows up late at the I6 integration. | Medium | High | Reduce | Freeze the EventIn and SuggestedEvent schemas from the Technical Specification before T17 starts; T24 validates I6. | Kaden | Any change to the event schema after 2027-02-14 | T7, T17, T24 |
| R-09 | Team | One member is unavailable for a week or more because of illness or personal reasons. | Medium | High | Reduce | Pair review on shared tasks (T2, T7, T12, T21, T22, T23, T24, T25); each member can run the other's side from the README. | Laila | A member misses a week without notice, or is out for more than 1 week | Shared tasks and protected block PB2 |
| R-10 | External | Advisor or instructor feedback (Assignments 12, 13, and 15) changes the scope after this plan is set. | Medium | Medium | Accept | Log scope changes in the notebook and absorb them in the protected blocks first. | Kaden | Feedback adds or changes a requirement | Protected blocks PB1 and PB2 |