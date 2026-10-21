# User Stories and use cases
**Team members:** Laila Raines, Kaden Sawyer

## Stakeholder Map
- Primary: First year/transfer student
    - Needs: To access and upload scheduling and features with minimal friction, and to get between classes on time on a campus they are not familiar with
- Secondary: Head of student club/org
    - Needs: Able to post and cancel events and reach students who are interested
- Hidden: Student that requires assistive tech to navigate media
    - Needs: Be able to read/listen to their schedule and campus events with accessibility tools built into the app
- Hidden: University student-records privacy office (FERPA compliance)
    - Needs: Schedule data is only stored with the student's consent, kept to the minimum needed, and fully deleted when the student removes it

## User Stories
- US-01: Importing schedule
    - Stakeholder: First year/transfer student (Primary)
    - Description: As a first year/transfer student, I want to load my class schedule without typing in every course by hand, so that I can see where I need to be each day
- US-02: Knowledge of campus events
    - Stakeholder: First year/transfer student (Primary)
    - Description: As a first year/transfer student, I want to see campus events that don't conflict with my class schedule, so that I can participate in events that interest me and meet new people.
- US-03: Increase awareness of campus events
    - Stakeholder: Student club/org (Secondary)
    - Description: As the head of a student club, I want to post and cancel my club's events, so that interested students hear about changes within a minute without me having to message each of them myself.
- US-04: App's assistive technology
    - Stakeholder: Student who requires assistive tech (Hidden)
    - Description: As a student who uses a screen reader, I want every part of my schedule and event information to be read out completely and in order, so that I can find my classes and events on my own
- US-05: Walking time between classes
    - Stakeholder: First year/transfer student (Primary)
    - Description: As a first year/transfer student who doesn't know campus yet, I want to know how long it takes to walk between my back-to-back classes, so that I arrive on time to each one.

## Use Cases
- UC-01: Importing Schedule (Expands US-01)
    - Primary actor: The student importing their courses
    - Secondary actors: Campus building directory (used to match and suggest building names)
    - Preconditions to verify: 
        - Student is signed in and on the schedule screen
        - Student has given consent to store their schedule data
        - Student has their schedule file saved on their device
    - Main Success Flow:
        1. Student chooses to import schedule
            - System response: Shows the files on their device that can be selected
        2. Student selects the schedule file
            - System response: Checks that the file is a PDF of 10MB or less, pulls course and course info, shows the courses for review. Course info that couldn't be pulled will be highlighted 
        3. Student reviews the courses and confirms them
            - System response: Saves the courses, shows a notification saying that the schedule has been accepted, and shows their filled in schedule
    - Alternate Flows:
        1. Student edits fields on a course
            - System response: Updates the course and removes any highlighted incomplete areas as the student fills in information
        2. Student wants to cancel schedule upon review
            - System response: Deletes pulled courses and navigates back to the schedule screen
    - Exception Flows:
        1. No course info can be pulled from the selected file
            - System response: Tells the student no courses were found, saves 0 courses, and offers manual entry
            - Student enters a course manually
            - System response: Provides building name autofill suggestions and allows student to save changes
        2. Student tries to upload a file that isn't a PDF or is larger than 10MB
            - System response: Rejects file and tells student to upload a PDF that is 10MB or less
    - Postcondition: Every course the student confirmed or entered is saved in their schedule. Courses cancelled during review are not saved and do not show up on course schedule
    - Given/When/Then:
        - Main Flow:
            - (AC-01.1)
                - Given: A signed in student that has consented to their schedule being used and has a PDF that is 10MB or less with 5 courses
                - When: The student selects the file
                - Then: Within 10 seconds shows all 5 courses and the information (name, professor, building, room, and time) matching the file
            - (AC-01.2)
                - Given: When reviewing, there are 5 courses shown
                - When: The student confirms
                - Then: A successful notification shows and their schedule shows the 5 courses and the information within 5 seconds
        - Alternate Flow:
            - (AC-01.3)
                - Given: When reviewing, there are 5 courses shown
                - When: The student cancels the import
                - Then: The schedule screen is shown and 0 of the 5 courses are saved to their schedule
        - Exception Flow:
            - (AC-01.4)
                - Given: A signed in student that has consented to their schedule being used and has a PDF that is 10MB or less with no readable courses
                - When: The student selects the file
                - Then: Within 10 seconds a message says no courses were found and offers manual entry, and 0 courses are saved
            - (AC-01.5)
                - Given: The student is entering a course manually
                - When: The student begins typing the building name (first 3 characters)
                - Then: Within 2 seconds, autofill suggestions will be given for 1 to 5 possible building names they may be entering
            - (AC-01.6)
                - Given: The student is on the schedule screen
                - When: The student selects a JPEG file
                - Then: An error message shows asking for PDF only and 0 courses are saved

- UC-02: Publish a club event (Expands US-03)
    - Primary actor: Head of the student club
    - Secondary actors: The interested students
    - Preconditions to verify: 
        - Head of club is signed in and has been verified as head of the club
        - For cancellation, a published event for the club exists and its start time is in the future
    - Main Success Flow:
        1. Head of club creates a new event
            - System response: Fields for name, time, location, and description
        2. Head of club enters information and submits the event
            - System response: Checks that every field is filled in and that the time is in the future, then publishes the event. It sends a confirmation notification to the head of the club and an event notification to interested students.
    - Alternate Flow:
        1. Head of club opens the published event, cancels it, and confirms cancellation
            - System response: Shows the event details and how many students are interested, then marks the event as cancelled and removes it from the event list and from the schedules of interested students who added it. It will send a cancellation confirmation to the head of the club and to those interested.
    - Exception Flow:
        1. Head of the club tries to add an event with missing info
            - System response: Highlights the missing items and doesn't add event until all info has been entered
    - Postcondition: A created event has a name, time, location, and description and is available to all students. If cancelled, the event is no longer seen on calendars and notifies the head of student club and interested students.
    - Given/When/Then:
        - Main flow:
            - (AC-02.1)
                - Given: The verified head of the club filled in the information for an upcoming event
                - When: They submit the event
                - Then: The event shows up in the event list within 5 seconds and the students who have the club as an interest will get a notification with the event info within 60 seconds
        - Alternate flow:
            - (AC-02.2)
                - Given: A submitted event has 5 students interested
                - When: The head of the club cancels it and confirms cancellation
                - Then: Within 60 seconds all 5 students will get a cancellation notification with the event info and it will be removed from the calendars/schedule
        - Exception flow:
            - (AC-02.3)
                - Given: The head of the club misses the location of event
                - When: They try to submit the event
                - Then: 0 notifications are sent and the event will not be published and the "location" field will be highlighted until they fill it in.


## INVEST Self Check
- US-01: 
    - Independent: Yes 
    - Negotiable: Yes
    - Valuable: Yes
    - Estimable: Yes
    - Small: Somewhat
    - Testable: Yes
    - Notes: Import, review, and manual entry can be split up later. Editing or removing courses after the schedule is saved is left for a later story. Expanded in UC-01
- US-02: 
    - Independent: Somewhat
    - Negotiable: Yes
    - Valuable: Yes
    - Estimable: Yes
    - Small: Yes
    - Testable: Yes
    - Notes: Needs schedule info from US-01 or a manual entry. 
- US-03: 
    - Independent: Yes
    - Negotiable: Yes
    - Valuable: Yes
    - Estimable: Yes
    - Small: Yes
    - Testable: Yes
    - Notes: Expanded in UC-02
- US-04: 
    - Independent: Yes
    - Negotiable: Yes
    - Valuable: Yes
    - Estimable: Somewhat
    - Small: Somewhat
    - Testable: Yes
    - Notes: Broad and it will be split by feature once screens are known. Tested by checking that every schedule and event field is read out in order by VoiceOver (iOS) and TalkBack (Android)
- US-05: 
    - Independent: Somewhat
    - Negotiable: Yes
    - Valuable: Yes
    - Estimable: Somewhat
    - Small: Yes
    - Testable: Yes
    - Notes: Needs schedule info from US-01 and campus building locations. Hard to estimate until we pick a map/routing data source. Replaced a professor ratings story, which was cut because it depended on an outside ratings source and its benefit (choosing courses) didn't fit students who have already registered
