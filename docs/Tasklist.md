# Tasklist.md
- **Team Members:** Kaden Sawyer, Laila


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

