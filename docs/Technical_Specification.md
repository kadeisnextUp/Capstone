# Campus Companion: Requirements Document
- **Team Members:** Kaden Sawyer, Laila Raines
- **Version:** 1.1 | **Date:** 2026-11-18
- **Related Files:** User_stories.md | Design_Diagrams

## 1. Version and Change Log
 
| Version | Date | Change | Author |
| ------- | ---- | ------ | ------ |
| 1.0 | 2026-11-11 | Draft from D0, before D1 and D2 were finished; the stack and most interfaces were TBD. | Laila and Kaden |
| 1.1 | 2026-11-18 | Rebuilt from D0, D1, and D2. Interfaces use D2's revised IDs (I1a to I14), the endpoint table lists all 24 endpoints, the stack is filled in, and every matrix row names a design component. | Laila and Kaden |
 
- **Design changes since D0, and why.** D0 itself is unchanged. D2 Part 2.15 records how its interfaces work now.
 
| Change | Reason |
| ------ | ------ |
| D0 interfaces I1 to I12 revised and I13 (device tokens) and I14 (map tiles) added | Some carried too many parameters or exposed other components' internals (D2 2.15). |
| A person checks a club's categories at approval, instead of C5 reading the description (R-09) | The admin already reads every request, and a wrong guess would notify the wrong students. |
| Notices go through a notification_job table and a worker (R-06) | A job written with its event survives a restart, and posting never waits on Expo. |
| Multi-row writes run as PostgreSQL functions (R-05) | Each supabase-py call is its own transaction, so a series of calls cannot be all-or-nothing. |
| Walks are drawn on a map (US-05-M), sign-up is limited to UC email (NFR-09), and events are not saved to schedules (GAP-3). All decided 2026-11-11 | The map helps students find the buildings, UC email shows that club requests come from real accounts, and notices and lists already show every event. |
 
The peer design review is not complete, so any change it produces will be added here in version 1.2
 
## 2. Architecture Summary
 
Campus Companion uses a client-server architecture with a pipeline inside the server for each flow, as chosen in D0 Part 6. The apps are A1 (React Native mobile app) and A2 (React web app and admin portal). They call one Python (FastAPI) server over HTTPS, and sign in directly with Supabase Auth. The server runs the seven D0 components, C1 Information Check, C2 Route Planner, C3 Event Check, C4 Club Check, C5 Club Categories, C6 Event Classifier, and C7 Interest Finder, plus four glue components: G1 Authentication, G2 Notifications, G3 Configuration, and G4 Logging. Supabase holds the data (PostgreSQL). The external systems are E1 Open Map Data (OpenStreetMap, imported by hand), E2 Campus Links (GetInvolvedUC, checked by an admin), E3 OpenFreeMap (background map tiles), and the Expo Push Service.
 
The server runs on one host, not yet chosen (Q1), in the same region as the Supabase project. The Supabase secret key lives only in that host's settings, and the apps hold only a login session.
 
Data flows in two pipelines that meet at the event data. In the student flow, a schedule PDF goes to C1, which returns course drafts for review and saves them only after the student confirms (D-01). C2 looks up walks in a table built ahead of time from E1 (D-02), and C3 combines interests, courses, walks, and C6's candidate events into suggestions. In the club flow, a club request goes to C4, an admin approves it against E2 (D-04), C5 limits an event's categories to the club's, and C6 stores the event together with a notification_job row in one transaction. C7's worker reads the job, finds interested students, and G2 sends notices through Expo. The D0 diagram and D2 Figure 1 are in [Design_Diagrams](Design_Diagrams/) and are not copied here.
 

## 3. Interfaces and Data Formats

### 3.1 Conventions
 
Base path /v1, HTTPS, header `Authorization: Bearer <Supabase access token>`, JSON in UTF-8 (except the PDF upload), date-times in ISO 8601 with an offset, clock times HH:MM in America/New_York, durations in whole minutes, distances in whole meters. Every error has this body, where `field` is empty if no field is at fault and G4 adds `request_id` (also sent in the X-Request-ID header):
 
`{ "error": { "code": "invalid_course", "message": "End time must be after start time.", "field": "courses[2].end_time", "request_id": "..." } }`
 
Errors on every endpoint: 401 not_signed_in (missing or expired token), 403 uc_email_required (email unconfirmed or not a UC domain), 503 auth_unavailable, 503 database_unavailable (nothing partial is saved, and the app offers a retry), and 500 internal_error.
 
### 3.2 Component boundaries
 
| ID | Boundary | Inputs | Outputs and format | Protocol | Error behavior |
| -- | -------- | ------ | ------------------ | -------- | -------------- |
| I1a | Student to C1 | A PDF of 10,485,760 bytes or less; then the confirmed courses (1 to 15) | Course drafts, `no_courses_found`, `pages_skipped`; then the saved courses | HTTPS, multipart then JSON | 413, 415, 422 unreadable_pdf, 504 extraction_timeout; 422 invalid_course; manual entry offered |
| I1b | Student to C1 | `category_ids`, 0 to 20 | The saved list | HTTPS, JSON | 403 consent_required; 422 unknown_category |
| I2 | C1 to C2 and C3 | `get_courses(student_id)`, `get_interests(student_id)` | `list[Course]`, `set[int]` | In-process call | A failed read becomes 503 |
| I3 | C2 to C3 | `get_walk(from_building_id, to_building_id)` | A walk row, or none | In-process call | None or an unloaded table: C3 uses 15 minutes and logs a warning |
| I4a, I4b, I4c | C2, C3, and C6 to the student | Routes: a date. Suggestions: a window and limit. List: a window, category, and page | Legs; suggested events; event summaries (3.4) | HTTPS GET, JSON | 422 invalid_date, invalid_window; 503 routes_unavailable |
| I5 | E1 to C2 | `import_map(area, buildings_csv)`, run by hand | An ImportReport; the walk table is replaced in one call | Download over HTTPS, then a database call | Stops before writing if under 90% of building pairs connect; the last good table stays |
| I6 | C3 to C6 | `list_candidate_events(category_ids, window_start, window_end)` | Published events that share a category | In-process call | A failed read becomes 503 |
| I7a, I7b, I7c | Student or club head to C4 | A club request; an event; a cancel with `confirm: true` | Request ID; event ID and `interested_count`; cancelled status | HTTPS, JSON | 403, 404, 409, 422, 503 as in 3.3 |
| I8 | C4 to C5 | `resolve_event_categories(club_id, requested)` | The event's category IDs | In-process call | CategoryNotAllowedError (422); TooManyCategoriesError (422) |
| I9 | C5 to C6 | `save_event(club_id, category_ids, event)` | The new event ID | In-process call | MissingFieldError and others become 422, 403, 409, or 503 |
| I10 | C6 to C7 | A notification_job row written by `publish_event` or `cancel_event` in the event's transaction | A job with its own copy of the title, body, and category IDs | Database | A failed transaction leaves no event and no job; a worker crash is covered by a 60 s lease |
| I11 | C7 to G2 to phones | `send_push(messages)`, one message per phone | Tickets (ok or error per token) | HTTPS POST to Expo, batches of 100, at most 5 batches a second, 10 s timeout | 429, 5xx, or a timeout: retry after 5, 10, 20, 40, 80 s, at most 6 attempts; DeviceNotRegistered disables the token |
| I12 | E2 to C4 | An admin checks the club on GetInvolvedUC | The decision stored on the club request | Manual | If the site is down, the request stays Pending |
| I13 | A1 to G2 | `expo_push_token`, `platform` | 204 | HTTPS, JSON | 422 invalid_push_token |
| I14 | E3 to A1 and A2 | Map area on screen | Map tiles | HTTPS, by the map library | The line and markers draw on a plain background |
| G1 | Apps and server to Supabase Auth | Sign-up and sign-in (apps); `auth.get_user(token)` (server) | A login session; an AuthUser (user_id, email, is_admin) | HTTPS | 401, 403 uc_email_required, 503 auth_unavailable |
| DB | Server to Supabase | Table calls and `rpc()` functions, with the secret key | Rows | HTTPS (supabase-py) | 503 database_unavailable; functions roll back as a whole |
 
### 3.3 Endpoint table
 
"Consented" means a signed-in student who has consented; every endpoint also needs a confirmed UC email.
 
| Endpoint | Who may call | Input | Success | Errors | Req |
| -------- | ------------ | ----- | ------- | ------ | --- |
| GET /v1/me | Any signed-in user | None | 200 `student_id`, `consented_at`, `is_admin`, `category_ids`, `clubs[]` (creates the student on first call) | 401, 503 | REQ-06 |
| POST /v1/me/consent | Any | `agree: true` | 200 `consented_at` | 422 invalid_request | REQ-06 |
| DELETE /v1/me/consent | Any | None | 204; deletes courses and interests | 503 | NFR-01 |
| DELETE /v1/me | Any | None | 204; deletes the account and all data | 503 (the app retries) | NFR-03 |
| PUT /v1/me/interests | Consented | `category_ids`, 0 to 20 | 200 saved list | 403 consent_required; 422 unknown_category | REQ-07 |
| GET /v1/categories | Any | None | 200 `categories[]` by name | 401 | REQ-07, NFR-06 |
| GET /v1/buildings | Any | `q` 3 to 50 chars; `limit` 1 to 5 | 200 `buildings[]`, 0 to 5 | 422 query_too_short | REQ-05 |
| POST /v1/schedule/import | Consented | PDF file, 10,485,760 bytes or less | 200 drafts; saves nothing | 413 file_too_large; 415 not_a_pdf; 422 unreadable_pdf; 504 extraction_timeout | REQ-01, 02, 04 |
| PUT /v1/schedule/courses | Consented | `courses[]`, 1 to 15 | 200 `saved_count`, `courses[]` | 422 invalid_course, unknown_building; 403 consent_required | REQ-03 |
| GET /v1/schedule/courses | Consented | None | 200 `courses[]` | 403 consent_required | REQ-03 |
| GET /v1/schedule/routes | Consented | `date` | 200 `date`, `legs[]` | 422 invalid_date; 503 routes_unavailable | REQ-08, 09 |
| GET /v1/events/suggested | Consented | `from`, `to` (14 days at most), `limit` 1 to 50 | 200 `events[]`, `empty_reason` | 422 invalid_window | REQ-10, NFR-08 |
| GET /v1/events | Any | `from`, `to`, `category_id`, `limit` 1 to 100, `offset` | 200 `events[]`, `next_offset` | 422 invalid_window, unknown_category | REQ-11 |
| GET /v1/events/{event_id} | Any | None | 200 event summary plus `description`, `building_id`, `status` | 404 not_found | REQ-11 |
| POST /v1/clubs/requests | Any | `club_name`, `description`, `category_ids`, optional `campus_links_url` | 201 `request_id`, status pending | 422 invalid_request, unknown_category, too_many_categories; 409 request_pending | REQ-12 |
| GET /v1/clubs/mine | Any | None | 200 `requests[]`, `clubs[]` | 401, 503 | REQ-12 |
| POST /v1/clubs/{club_id}/events | Verified head | `name`, `building_id`, `room`, `description`, `starts_at`, `ends_at`, `category_ids` | 201 `event_id`, `status`, `category_ids`, `interested_count` | 403 not_club_head, club_not_verified; 404 club_not_found; 422 missing_field, start_in_past, invalid_time_range, unknown_building, category_not_allowed, too_many_categories; 409 duplicate_event | REQ-14 to 17 |
| GET /v1/clubs/{club_id}/events | Head | `include_past` | 200 `events[]` with `interested_count` | 403 not_club_head | REQ-19 |
| POST /v1/events/{event_id}/cancel | Head of the club | `confirm: true` | 200 `status` cancelled, `interested_count` | 403 not_club_head; 404 not_found; 409 already_cancelled, event_already_started; 422 invalid_request | REQ-18, 19 |
| GET /v1/admin/club-requests | Admin | `status` | 200 `requests[]` with `requester_email`, `existing_club` | 403 not_admin | REQ-12 |
| POST /v1/admin/club-requests/{request_id}/decision | Admin | `decision`, `category_ids`, `note` | 200 `status`, `club_id` | 403 not_admin; 404 not_found; 409 already_decided; 422 note_required | REQ-12 |
| POST /v1/admin/clubs/{club_id}/revoke | Admin | `note` | 200 `verified: false`, `cancelled_events` | 403 not_admin; 404 not_found | REQ-13 |
| POST /v1/devices | Any | `expo_push_token`, `platform` | 204 | 422 invalid_push_token | REQ-20 |
| DELETE /v1/devices/{expo_push_token} | Any | None | 204 (also for an unknown token) | 401 | REQ-20 |
 
### 3.4 Message payloads and schemas
 
Course input (PUT /v1/schedule/courses, each item). `end_time` must be after `start_time`, and `ends_on` must be on or after `starts_on` and at most 200 days later. A course draft has the same fields plus `building_text` and `missing_fields`, with the fields nullable; a saved course adds `course_id`.
 
```json
{ "type": "object",
  "required": ["name", "days", "start_time", "end_time", "starts_on", "ends_on"],
  "properties": {
    "name": {"type": "string", "minLength": 1, "maxLength": 100},
    "professor": {"type": ["string", "null"], "maxLength": 100},
    "building_id": {"type": ["integer", "null"]},
    "room": {"type": ["string", "null"], "maxLength": 20},
    "days": {"type": "array", "minItems": 1, "maxItems": 7, "uniqueItems": true,
             "items": {"enum": ["MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"]}},
    "start_time": {"type": "string", "pattern": "^[0-2][0-9]:[0-5][0-9]$"},
    "end_time": {"type": "string", "pattern": "^[0-2][0-9]:[0-5][0-9]$"},
    "starts_on": {"type": "string", "format": "date"},
    "ends_on": {"type": "string", "format": "date"} } }
```
 
Event input (POST /v1/clubs/{club_id}/events). `starts_at` must be in the future, and `ends_at` after it and at most 24 hours later.
 
```json
{ "type": "object",
  "required": ["name", "building_id", "description", "starts_at", "ends_at"],
  "properties": {
    "name": {"type": "string", "minLength": 1, "maxLength": 100},
    "building_id": {"type": "integer"},
    "room": {"type": ["string", "null"], "maxLength": 20},
    "description": {"type": "string", "minLength": 1, "maxLength": 2000},
    "starts_at": {"type": "string", "format": "date-time"},
    "ends_at": {"type": "string", "format": "date-time"},
    "category_ids": {"type": "array", "maxItems": 5, "items": {"type": "integer"}} } }
```
 
Response objects:
 
| Object | Fields |
| ------ | ------ |
| Leg | `from_course_id`, `to_course_id`, `gap_minutes`, `walk_minutes` or null, `distance_m` or null, `path` (list of [latitude, longitude]) or null, `status` (ok, tight, same_building, unknown_building, no_path) |
| SuggestedEvent | `event_id`, `name`, `club_name`, `building_name`, `room` or null, `starts_at`, `ends_at`, `matched_categories`, `walk_minutes_before` or null, `walk_minutes_after` or null |
| EventSummary | `event_id`, `name`, `club_name`, `building_name`, `room` or null, `starts_at`, `ends_at`, `categories` |
 
Push message to Expo, one per phone, with public event information only:
 
```json
{ "to": "ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]",
  "title": "New event: Fall Blitz Night",
  "body": "Chess Club · Thu Nov 12, 6:00 PM · Tangeman University Center 400A",
  "data": { "event_id": 4182, "kind": "published" },
  "collapseId": "event-4182-published", "tag": "event-4182-published",
  "priority": "high", "sound": "default" }
```
 
Log line (G4), one JSON object per request or job on standard output, with allow-listed fields only: `{ "ts": "2026-11-12T17:58:03.412-05:00", "level": "info", "event": "request", "request_id": "...", "method": "POST", "route": "/v1/clubs/{club_id}/events", "status": 201, "duration_ms": 184, "user_id": "..." }`
 
### 3.5 Database schema
 
Every table has row-level security on with no policies, so only the server's secret key can read or write. The first 9 tables are D1 Figure 2 (the login table, auth.users, belongs to Supabase Auth); the last 4 are new in D2 (the columns shown are the ones named in D2's text, and D2 Figure 2 has the full list).
 
| Table | Columns | Keys and rules |
| ----- | ------- | -------------- |
| student | student_id uuid, consented_at timestamptz (null) | PK and FK to auth.users.id, cascade |
| course | course_id bigint, student_id uuid, building_id int (null), name, professor, room text, days text[], start_time and end_time time, starts_on and ends_on date | PK course_id; FK student (cascade), building (restrict) |
| building | building_id int, osm_id bigint, name, short_code text, entrance lat and lon float8 | PK building_id; osm_id unique; never deleted |
| walk_time | from_building_id int, to_building_id int, walk_minutes int, distance_m int, path jsonb | PK both IDs; FK building |
| club | club_id int, head_student_id uuid, name text, verified_at timestamptz (null) | PK club_id; name unique; FK student (cascade) |
| event | event_id bigint, club_id int, building_id int, name, description text, room text (null), starts_at and ends_at timestamptz, status (published or cancelled) | PK event_id; unique (club_id, lower(name), starts_at); FK club (cascade), building (restrict) |
| category | category_id int, name text | PK category_id; name unique; never deleted |
| event_category | event_id bigint, category_id int | PK both; FK event (cascade) |
| student_interest | student_id uuid, category_id int | PK both; FK student (cascade) |
| club_category | club_id int, category_id int | PK both; FK club (cascade) |
| club_request | request_id, student_id, club_name, description, category_ids int[], campus_links_url, status (pending, approved, denied), decision_note, who decided and when, club_id (null), created_at | FK student (cascade); club_id set to empty if the club is deleted; unique partial index (student_id, lower(club_name)) where pending |
| push_device | student_id, expo_push_token, platform, last_seen_at, disabled_at | token unique; FK student (cascade); partial index on student_id where active |
| notification_job | event_id (null), kind (published or cancelled), status (queued, sending, retrying, sent, failed, skipped), title, body, category_ids, attempts, next_attempt_at, lease, created_at, finished_at | unique (event_id, kind); event_id set to empty if the event is deleted; partial index on next_attempt_at where queued or retrying |
 
Writes that touch more than one row are PostgreSQL functions called with `rpc()`: replace_courses, delete_account_data, decide_club_request, cancel_club_events, publish_event, cancel_event, and claim_notification_jobs (locks jobs with FOR UPDATE SKIP LOCKED). Expected sizes: about 5,000 courses, 3,844 walk times, 1,200 push tokens, and 400 jobs per two weeks.
 
## 4. Technology Stack
 
Items D1 and D2 mark "[Confirm]" (team skill fit with FastAPI, React, and push notifications, the type generator tool) are still open.
 
| Layer | Choice | Justification |
| ----- | ------ | ------------- |
| Database | Supabase (PostgreSQL), free plan | Joins and time ranges for REQ-10; cascading keys make deletion real (NFR-03); row-level security (NFR-04); free (NFR-07). Pro backups keep deleted rows up to 7 days, which the privacy notice must say. Firestore passed over because it has no joins. |
| Authentication | Supabase Auth, tokens last 1 hour, Confirm email on | NFR-09 and NFR-04; no hand-written login; the server checks each token with `auth.get_user`. |
| Backend | Python and FastAPI with Pydantic, Uvicorn, httpx, and supabase-py; one server runs C1 to C7 and G1 to G4 | The two hardest libraries are Python (REQ-02, REQ-08); asynchronous, so a slow upload does not block others; free (NFR-07). |
| PDF reading | pdfplumber, first 5 pages, 8 s limit | REQ-02 and REQ-04; PyMuPDF passed over because of its AGPL license. |
| Walking routes | OpenStreetMap data through OSMnx and NetworkX, stored as a walk table | CON-02, NFR-05, REQ-08; map data is ODbL and needs the credit. |
| Mobile app (A1) | React Native as an Expo development build, with expo-notifications | NFR-10 (native controls work with VoiceOver and TalkBack); REQ-20. |
| Web app and admin portal (A2) | React with Vite | REQ-12 (admin approvals); static files load quickly. |
| Map display | MapLibre (React Native in A1, GL JS in A2) with OpenFreeMap tiles | REQ-09, NFR-05, NFR-07; free and keyless, built from the same open data. |
| Push notices | Expo Push Service, called with httpx | REQ-17, REQ-18, NFR-07; one API for iPhone and Android, 100 notices per request. |
| Shared API types | TypeScript types generated from FastAPI's API description (tool to be picked) | Keeps the apps in step with the server. |
| Configuration and logging | Pydantic settings from environment variables; Python logging as JSON lines | NFR-04 (secrets stay on the host), NFR-02 (allow-listed fields), measuring the REQ-02 and REQ-17 budgets. |
| Hosting | Not chosen (Q1): Render, Railway, or Azure for Students, in Supabase's region | Must not sleep, or REQ-02 and REQ-17 fail; up to $7 a month (NFR-07). |
 
## 5. Known Constraints
 
| ID | Constraint | Rules out | Forces |
| -- | ---------- | --------- | ------ |
| CON-01 | FERPA consent before saving any data | Saving a schedule or interests before consent, or implied consent | A consent screen, `consented_at`, 403 consent_required (REQ-06), withdrawal and deletion (NFR-01, NFR-03) |
| CON-02 | Open map source, no school maps or logos | Official UC maps and logos, and routing services that receive class locations | OpenStreetMap routed on our server, OpenFreeMap tiles, and the credit (NFR-05, REQ-09) |
| CON-03 | Consent friction versus reach | A long or multi-step consent flow | One consent for schedule and interests, on one screen in the first-run flow |
| CON-04 | Filter by interest and identity-based groups | A feed ranked only by popularity | The 23 categories, including Cultural and Religious, as interests and a filter (REQ-07, REQ-11, NFR-06); there is no separate category for groups such as LGBTQ+ organizations |
| CON-05 | Events next to the student's schedule | An event list that ignores classes | C3's fit check against real walks (REQ-10); events are suggested, not saved to the schedule (Requirements Q6) |
 
## 6. Traceability Matrix
 
Status is Designed, In progress, or Planned. Every row has a design component, so no row is ORPHANED. The Test Case column is TBD because the Week 10 test plan (Assignment 10) is not written yet; each description names its acceptance criterion where one exists.
 
| Req ID | Description | Design Component (D0/D1/D2) | Status | Test Case |
| ------ | ----------- | --------------------------- | ------ | --------- |
| REQ-01 | Accept only PDFs of 10 MB or less (AC-01.6) | C1 Information Check: upload guard, I1a (D2 2.3); A1 size check (2.14) | Designed | TBD |
| REQ-02 | Show 5 courses within 10 s (AC-01.1) | C1 Information Check: parse_schedule(), match_building() (D2 2.3); CourseDraft (D1 Part 5) | Designed | TBD |
| REQ-03 | Review, confirm in 5 s, or cancel (AC-01.2, AC-01.3) | C1 Information Check: PUT /v1/schedule/courses, replace_courses (D1 Part 5; D2 2.2); A1 import flow (2.14) | Designed | TBD |
| REQ-04 | No courses or unreadable: manual entry in 10 s (AC-01.4) | C1 Information Check: import_schedule() error paths (D2 2.3; EC-03, EC-06, EC-08) | Designed | TBD |
| REQ-05 | Building suggestions in 2 s (AC-01.5) | C1 Information Check: GET /v1/buildings (D1 Part 5); building table (D1 Figure 2) | Designed | TBD |
| REQ-06 | Save nothing before consent | C1 Information Check and G1 Authentication: consented_at, require_consent (D2 2.3, 2.10) | Designed | TBD |
| REQ-07 | Save 0 to 20 interest categories | C1 Information Check: PUT /v1/me/interests (D2 2.3); C5 Club Categories list (2.7) | Designed | TBD |
| REQ-08 | Walk time and status for each leg | C2 Route Planner: build_walk_table(), get_walk(), GET /v1/schedule/routes (D1 3.1, Part 5; D2 2.4) | Designed | TBD |
| REQ-09 | Draw each walk on a map | C2 Route Planner: walk_time.path (D1 Figure 2); A1 and A2 route map; E3 (D2 2.14) | Designed | TBD |
| REQ-10 | Suggest events that fit between classes | C3 Event Check: suggest_events() (D1 3.2); C6 list_candidate_events() (I6, D2 2.8) | Designed | TBD |
| REQ-11 | List published events with a category filter | C6 Event Classifier: GET /v1/events and /{event_id} (D2 2.8) | Designed | TBD |
| REQ-12 | Request, approve, or deny a club | C4 Club Check: club_request, decide_club_request (D2 2.6, Figure 4a); A2 admin portal (2.14); E2 check (I12) | Designed | TBD |
| REQ-13 | Revoke a club and cancel its events | C4 Club Check: revoke endpoint; cancel_club_events (D2 2.6, 2.8) | Designed | TBD |
| REQ-14 | Limit event categories to the club's | C5 Club Categories: resolve_event_categories(), club_category (D2 2.7) | Designed | TBD |
| REQ-15 | Post an event, listed in 5 s (AC-02.1 list) | C6 Event Classifier: save_event() (D1 Part 5), publish_event (D2 2.8, Figure 3); C4 posting endpoint (2.6) | Designed | TBD |
| REQ-16 | Reject a missing field, 0 notices (AC-02.3) | C6 Event Classifier: save_event() errors (D1 Part 5; D2 2.8, Figure 3) | Designed | TBD |
| REQ-17 | Notice within 60 s, with retries (AC-02.1) | C7 Interest Finder: notification_job, process_job() (D2 2.9, Figure 4c); G2 Notifications: send_push() (2.11) | Designed | TBD |
| REQ-18 | Cancel and send a cancel notice in 60 s (AC-02.2) | C6 Event Classifier: cancel_event (D2 2.8, Figure 4b); C7 Interest Finder: cancelled jobs (2.9) | Designed | TBD |
| REQ-19 | Show the interested count before cancelling | C7 Interest Finder: count_interested() (D2 2.9); C4 GET /v1/clubs/{club_id}/events | Designed | TBD |
| REQ-20 | Register and remove push tokens | G2 Notifications: push_device, POST and DELETE /v1/devices, I13 (D2 2.11) | Designed | TBD |
| NFR-01 | Withdrawing consent deletes courses and interests | C1 Information Check: DELETE /v1/me/consent function (D2 2.3) | Designed | TBD |
| NFR-02 | Store and send the minimum | G4 Logging allow-list (D2 2.13); G2 messages (2.11); A1 keeps no schedule on disk (2.14); C7 job cleanup (2.9) | Designed | TBD |
| NFR-03 | Account deletion removes everything | C1 Information Check: DELETE /v1/me, delete_account_data, cascades (D2 2.2, 2.3) | Designed | TBD |
| NFR-04 | Data isolation and secrets | G1 Authentication (D2 2.10); G3 Configuration (2.12); row-level security (D1 Part 2) | Designed | TBD |
| NFR-05 | Open map data only, with credit | E1 Open Map Data and C2 Route Planner: import_map() (D2 2.4); E3 OpenFreeMap (2.14) | Designed | TBD |
| NFR-06 | All 23 categories, filterable | C5 Club Categories: category list (D2 2.7); C6 GET /v1/events?category_id (2.8) | Designed | TBD |
| NFR-07 | Free tiers except a host up to $7 a month | Whole system: build versus reuse (D2 2.16); host not chosen (Q1) | In progress | TBD |
| NFR-08 | Suggestions within 2 s | C3 Event Check: suggest_events() (D1 3.2) | Designed | TBD |
| NFR-09 | Only confirmed UC emails | G1 Authentication: current_user() step 3 (D2 2.10); G3 ALLOWED_EMAIL_DOMAINS (2.12); A1 and A2 sign-up check; email sender open (Q4) | In progress | TBD |
| NFR-10 | Accessibility for every field | A1 Mobile app: accessibility rules (D2 2.14); full names from the API (D1 Part 5) | Designed | TBD |
| NFR-11 | No partial writes | C1, C4, C6, and C7: PostgreSQL functions through rpc() (D2 2.2, R-05) | Designed | TBD |
| NFR-12 | No lost notice jobs after a restart | C7 Interest Finder: claim_notification_jobs lease (D2 2.9, Figure 4c); notification_job (2.2) | Designed | TBD |
| CON-01 | FERPA consent | C1 Information Check and G1 Authentication (REQ-06, NFR-01) | Designed | TBD |
| CON-02 | Open map source | E1 Open Map Data, C2 Route Planner, E3 (NFR-05) | Designed | TBD |
| CON-03 | Low consent friction | A1 Mobile app: one consent screen in the first-run flow (D2 2.14) | Designed | TBD |
| CON-04 | Filter by identity-based groups | C5 Club Categories; C6 Event Classifier filter (NFR-06) | Designed | TBD |
| CON-05 | Events next to the schedule | C3 Event Check and the A1 "For you" screen (D2 2.14) | Designed | TBD |
 
## 7. Advisor Review Record
 
| Item | Record |
| ---- | ------ |
| Date of advisor review | Pending |
| What was discussed | Pending |
| Changes it produced | Pending |