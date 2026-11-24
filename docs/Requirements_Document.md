# Campus Companion: Requirements Document
- **Team Members:** Kaden Sawyer, Laila
- **Version:** 0.1 | **Date:** 2026-11-11
- **Related Files:** User_stories.md | Design_Diagrams| Technical_Specification.md

1. Version and Change Log
| Version | Date | Change | Author |
| 0.1 | 2026-11-11 | Initial draft adding information from the user stories and use cases, and D0 block diagram. | Laila Raines and Kaden Sawyer

2. Purpose and scope
- Purpose: This document states what our app, Campus Companion will do and for who. This document will not describe how the system is built.

- Scope: Campus Companion helps first year and transfer students at the University of Cincinnati get to class on time and find campus events that will fit their schedule and interests. This will document cover:
    - Importing a class schedule from a PDF, reviewing it, and entering courses manually
    - Showing walking time and map route between consecutive classes, using open map data
    - Showing campus events that do not conflict with the students classes and match their interests
    - Letting verified club heads post and cancel events, and notify the interested students
    - Reading all schedule and event information with a screen reader

3. Stakeholders and User Goals
| Stakeholder | Category | Goal | Stories |
| First year/transfer students | Primary | Upload a schedule with minimal friction, get between classes on time on an unfamiliar campus, and find events that do not conflict with their schedule | US-01, US-02, US-05 |
| Head of a student club or org | Secondary | Post and cancel evvents and reach interested students without messaging each one | US-02 |
| Student who requires assistive tech | Hidden | Read or listen to their schedule and campus events with accessibility tools built into the app | US-04|
| University student-records privacy office | Hidden | Schedule data is stored only with the student's consent, kept to the minimum, and fully deleted when the student removes it | None**(See REQ-06, REQ-07)

4. Glossary

| Term | Definition |
| ---- | ---------- |
| Administrator | A person who approves club or organization submissions before they are published. |
| Assistive-technology user | A student who uses a screen reader or similar tool to use the app. |
| Campus Links | The University of Cincinnati organization list the system checks to confirm that a club exists (external system E2 in D0). |
| Club or organization | A student club or organization at the University of Cincinnati that posts events. |
| Club head | The person who leads a club or organization. A verified club head belongs to an approved club and is signed in. |
| Consent | Explicit student permission, given on a consent screen, to save their schedule data (FERPA principle). |
| Course | One class entry in a schedule: name, professor, building, room, and time. |
| Downtime | Time between a student's classes in which the student has no class. |
| Event | A campus activity posted by a club or organization, with a name, time, location, and description. |
| FERPA | The U.S. Family Educational Rights and Privacy Act, which governs student education records. |
| Interest category | A topic label, including identity-based organization labels, used to match students with clubs and events. |
| Notification | A message to a student or club head about a published or cancelled event. |
| Onboarding | First-use setup in which the student chooses their interest categories. |
| Open map data | Freely licensed map data of campus buildings and paths (external system E1 in D0). |
| Privacy office | The University student-records privacy office responsible for FERPA compliance. |
| Review step | The screen after a PDF import where the student checks, edits, and confirms or cancels the extracted courses. |
| Route | A path along campus paths between two buildings. |
| Schedule | The set of a student's saved courses. |
| Schedule file | A PDF of the student's class schedule, 10 MB or less. |
| Screen reader | Software that reads screen content aloud, such as VoiceOver (iOS) and TalkBack (Android). |
| Student | A first-year or transfer student who uses the app. |
| Walking time | The estimated minutes to walk a route, shown in whole minutes. |

5. Functional Requirements
Verification methods: Test, Inspection, Demonstration, Analysis. Acceptance criteria (AC-xx.x) are in [User_Stories.md](User_Stories.md).
 
| ID | Requirement | Priority | Source story | Verification |
| -- | ----------- | -------- | ------------ | ------------ |
| REQ-01 | The system should accept a schedule file only if it is PDF that is 10 MB or less; for any other file/file size, it should reject the file, ask for a PDF of 10 MB or less, and save 0 courses. | Should | US-01 (AC-01.6) | Test |
| REQ-02 | When a student selects a valid PDF containing 5 courses, the system should display all 5 courses with the name, professor, building, room, and time for each. It should matching the file they submitted and display within 10 seconds. | Must | US-01 (AC-01.1) | Test |
| REQ-03 | The system should show extracted courses in the review step, highlight each field it could not extract, let the student edit any field (removing the highlight once filled), and save courses only after the student confirms. On confirmation it should show the saved schedule within 5 seconds and on cancellation it shall save 0 of the extracted courses and return to the schedule screen. | Must | US-01 (AC-01.2, AC-01.3) | Test |
| REQ-04 | When no courses can be extracted from a valid PDF, the system should, within 10 seconds, tell the student that no courses were found, save 0 courses, and offer manual entry. | Should | US-01 (AC-01.4) | Test |
| REQ-05 | The system should let a student add a course manually and, once the student has typed the first 3 characters of a building name, it should show 1 to 5 building-name suggestions within 2 seconds. | Should | US-01 (AC-01.5) | Test |
| REQ-06 | The system should ask and record the student's explicit consent before saving any schedule data and should save 0 schedule records for a student who has not consented. | Must | US-01 (UC-01 precondition); privacy office | Test, Inspection |
| REQ-07 | When a student removes their schedule data, the system shall delete all of that student's stored courses and interest categories within 60 seconds† so that 0 of them are returned by the app afterward. | Should | Privacy office (no story yet) | Test |
| REQ-08 | For each pair of consecutive classes on the same day, the system shall display the walking time in whole minutes and a route on the campus map built from open map data, within 3 seconds† of the student opening the schedule screen, for 100% of building pairs present in the map data. | Must | US-05 | Test, Analysis (timed walks of 10† building pairs agree within 2 minutes†) |
| REQ-09 | The system shall list as available only events whose time does not overlap any class in the student's schedule, so that 0 overlapping events appear in that list. | Must | US-02 | Test |
| REQ-10 | The system shall let a student select 1 or more interest categories during onboarding and shall notify that student only about events from clubs tagged with a selected category. | Should | US-02, US-03 | Test, Demonstration |
| REQ-11 | The system shall let a student filter the event list by interest category and by identity-based organizations so that, when a filter is chosen, 0 events outside the filter are shown. | Could | US-02; CON-04 | Test |
| REQ-12 | The system shall let a verified club head create an event with name, time, location, and description, and shall reject a submission with any field missing or with a time in the past, highlight each missing field, and send 0 notifications. | Must | US-03 (AC-02.3) | Test |
| REQ-13 | When a verified club head submits a valid event, the system shall show it in the event list within 5 seconds and send a notification with the event information within 60 seconds to each student who has the club as an interest. | Must | US-03 (AC-02.1) | Test |
| REQ-14 | When a verified club head cancels a published event, the system shall mark it cancelled, remove it from the event list and from the schedules of students who added it, and send a cancellation notification to the club head and to each interested student within 60 seconds. | Should | US-03 (AC-02.2) | Test |
| REQ-15 | The system shall publish a club only after an administrator approves it and its existence at the university is checked against Campus Links, so that 0 events from unapproved clubs are visible to students. | Should | US-03 (UC-02 precondition) | Test, Inspection |
| REQ-16 | The system shall assign each approved club at least 1 interest category derived from its description and shall classify each submitted event, so that 100% of published events carry at least 1 interest category. | Should | US-02, US-03 | Test |

6. Non-Functional Requirements 
| ID | Requirement | Priority | Source | Verification |
| -- | ----------- | -------- | ------ | ------------ |
| NFR-01 | Performance: with 200† concurrent students, the system shall display the schedule screen and the event list within 2 seconds† for 95%† of requests. | Should | US-02, US-05 | Test (load test) |
| NFR-02 | Security: the system shall send all data between the app and the server over TLS 1.2 or higher, with 0 endpoints reachable over unencrypted HTTP. | Must | Privacy office; CON-01 | Inspection, Test (scan) |
| NFR-03 | Security: the system shall encrypt stored schedule data with AES-256† and allow only the owning student to read it, so that 0 of 20† cross-account read attempts in access-control tests succeed. | Must | Privacy office; CON-01 | Test, Inspection |
| NFR-04 | Security: the system shall require a signed-in, verified club head to create or cancel events and shall reject 100% of unauthenticated or non-head requests with an authorization error (HTTP 401 or 403). | Must | US-03 (UC-02 precondition) | Test |
| NFR-05 | Usability (accessibility): the system shall let a screen reader read 100% of schedule and event fields in order, verified with VoiceOver (iOS) and TalkBack (Android) on every schedule and event screen. | Must | US-04 | Test (manual) |
| NFR-06 | Usability: in a test with 5† first-time student users, at least 4† shall import a schedule from a PDF, including the consent step, in 3 minutes† or less without help. | Should | US-01; CON-03 | Test (usability) |
| NFR-07 | Reliability: the system shall be available 99%† of each month during the academic term, measured by uptime monitoring. | Should | US-05 | Analysis |
| NFR-08 | Reliability: the system shall keep 100% of confirmed schedules and published events after a server restart, with 0 records lost in the restart test. | Should | US-01, US-03 | Test |

7. MoSCoW Summary
 - **Must:** REQ-02 schedule extraction, REQ-03 review and confirm, REQ-06 consent, REQ-08 walking time and route, REQ-09 conflict-free events, REQ-12 create event, REQ-13 publish and notify, NFR-02 TLS, NFR-03 access control and encryption, NFR-04 club head authentication, NFR-05 screen-reader access.
 - **Should:** REQ-01, REQ-04, REQ-05, REQ-07, REQ-10, REQ-14, REQ-15, REQ-16, NFR-01, NFR-06, NFR-07, NFR-08.
 - **Could:** REQ-11 filter by interest and identity-based organizations.
 - **Won't for this release:**
    - Professor ratings. The story was cut because it depended on an outside ratings source and did not help students who had already registered.
    - Editing or removing individual saved courses after the schedule is saved (left for a later story; REQ-07 removes all data only).
    - Schedule import from any format other than PDF, such as screenshots or JPEG files.
    - Use of official university maps or logos (CON-02).
    - Any connection to university student-records systems; the only student data source is the PDF the student provides.

8. Inherited Constraints
| ID | Constraint | Area |
| -- | ---------- | ---- |
| CON-01 | Follow FERPA's consent principles: require explicit student permission before saving any student data. | Legal |
| CON-02 | Integrate an open map source instead of copyrighted school maps or logos, to avoid intellectual property issues. | Legal |
| CON-03 | Consent screens add friction at sign-up, which pressures the social goal of reaching as many new students as possible; the consent step must stay short and clear. | Legal, Social |
| CON-04 | Let students filter the event feed by interest and identity-based organizations instead of showing only the most popular events. | Diversity and Cultural |
| CON-05 | Put campus events on the student's schedule to lower the barrier to taking part, making event recommendations a main feature. | Social |

9. Assumptions and Open Questions
| ID | Item | Owner | Date (target) |
| -- | ---- | ----- | ------------- |
| A-01 | The client is a mobile app for iOS and Android, assumed from US-04's use of VoiceOver and TalkBack. | Kaden Sawyer | 2026-11-13 |
| A-02 | Numbers marked † are drafting proposals; the team confirms or replaces each one. | Laila Raines | 2026-11-14 |
| A-03 | University schedule PDFs have a consistent enough layout to extract courses from; sample PDFs are needed to check. | Kaden Sawyer | 2026-11-16 |
| OQ-01 | Which open-source map and routing source is used for E1 (result of the mapping research task in Tasklist.md)? | Kaden Sawyer | 2026-11-16 |
| OQ-02 | Should REQ-07 (deletion) be a Must for legal reasons, and how long may backups keep deleted data? | Laila Raines | 2026-11-14 |
| OQ-03 | Who acts as administrator, and what exactly does the check against Campus Links look for (REQ-15)? | Laila Raines | 2026-11-16 |
| OQ-04 | What is the starting list of interest categories and identity-based organization labels? | Laila Raines | 2026-11-16 |
| OQ-05 | Do events need an end time? UC-02 lists only "time", but REQ-09 needs an end time to detect overlaps. | Kaden Sawyer | 2026-11-13 |
| OQ-06 | Are notifications push messages, in-app messages, or both? | Kaden Sawyer | 2026-11-16 |
| OQ-07 | D2 edge cases are not complete; new requirements or changes to existing ones may follow. | Laila Raines, Kaden Sawyer | After D2 is complete |
| OQ-08 | Does the team accept the Won't list in section 7? Items beyond the Week 4 notes were added in this draft. | Laila Raines | 2026-11-14 |