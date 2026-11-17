# Campus Companion: Requirements Document
- **Team Members:** Kaden Sawyer, Laila
- **Version:** 0.1 | **Date:** 2026-11-11
- **Related Files:** User_stories.md | Design_Diagrams

## Version and Change Log
| Version | Date | Change | Author |
| 0.1 | 2026-11-11 | Initial draft adding information D0 block diagram. | Laila Raines and Kaden Sawyer

## Architecture Summary
    - Campus Companion uses a client-server architecture 

## 
1. Deployment Model
 - The client talks to one server deploment over HTTPS. Map dtaa from W1 is imported into C2 ahead of time (D0 Decision D-02), so route requests do not depend on the outside source at runtime. Hosting site has not been chosen***

2. Data Flow
- In the student flow, the schedule PDF and onboarding interest questionnaire (I1) go to C1, which takes the courses and shows them for review before saving the information (D-01). Confirmed courses (I2) go to C2, Which uses E1 map data (I5) to compute routes and walking times (I3). C3 checks events against the schedule and returns route suggestions and matching eents (I4). In the club flow, club information and events (I7) go to C4, which will confirm the club against E2 Campus Links (I12) and holds it manual administrator approval (D-04). The approved club passes through C5 for categories (I8, I9), C6 for event classification (I10), and C7, which finds the interested students and sends club information, events, and notifications (I11). The two flows connect throug I6, a request and response link between the two systems. 

5. Technology Stack

6. Constraints
| ID | Constraint | Area |
| -- | ---------- | ---- |
| CON-01 | Follow FERPA's consent principles: require explicit student permission before saving any student data. | Legal |
| CON-02 | Integrate an open map source instead of copyrighted school maps or logos, to avoid intellectual property issues. | Legal |
| CON-03 | Consent screens add friction at sign-up, which pressures the social goal of reaching as many new students as possible; the consent step must stay short and clear. | Legal, Social |
| CON-04 | Let students filter the event feed by interest and identity-based organizations instead of showing only the most popular events. | Diversity and Cultural |
| CON-05 | Put campus events on the student's schedule to lower the barrier to taking part, making event recommendations a main feature. | Social |

7. Traceability Matrix
| Req ID | Description | Design Component (D0/D1/D2) | Status | Test Case |
| ------ | ----------- | --------------------------- | ------ | --------- |
|REQ-01 | 


8. Advisor Review 