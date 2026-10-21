# User Stories and use cases
**Team members:** Laila Raines, Kaden Sawyer

## Stakeholder Map
- Primary: First year/transfer student
    - Needs: To access and upload scheduling and features with minimal friction 
- Secondary: Head of student club/org
    - Needs: Able to post and cancel events and reach students who are interested
- Hidden: Student that requires assistive tech to navigate media
    - Needs: Be able to read/listen to their schedule and campus events with accessibility tools built into the app

## User Stories
- US-01: Importing schedule
    - Stakeholder: First year/transfer student (Primary)
    - Description: As a first year/transfer student, I want to load my class schedule without typing in every course by hand, so that I can see where I need to be each day
- US-02: Knowledge of campus events
    - Stakeholder: First year/transfer student (Primary)
    - Description: As a first year/transfer student, I want to see campus events that don't conflict with my class schedule, so that I can participate in events that interest me and meet new people.
- US-03: Increase awareness of campus events
    - Stakeholder: Student club/org (Secondary)
    - Description: As the head of a student club, I want to post and cancel my club's events, so that interested students know about changes within a minute.
- US-04: App's assistive technology
    - Stakeholder: Student who requires assistive tech (Hidden)
    - Description: As a student who uses assistive tech, I want to be able to read my schedule and event information with the tools I already use, so that I can find my classes and events on my own
- US-05: Professor Ratings
    - Stakeholder: First year/transfer student (Primary)
    - Description: As a first year/transfer student, I want to see how other students rate my professors, so that I know what to expect from my courses and can choose them accordingly.

## Use Cases
- UC-01: Importing Schedule (Expands US-01)
    - Primary actor: The student importing their courses
    - Secondary actors: College course and building info/data
    - Preconditions to verify: 
        - Student is signed in and on the schedule screen
        - Student has given consent to store their schedule data
        - Schedule file is 10MB or smaller and is a PDF 
    - Main Success Flow:
        1. Student chooses to import schedule
            - System response: Shows the files on their device that can be selected
        2. Student selects the schedule file
            - System response: Checks the file, pulls course and course info, shows the courses for review. Course info that couldn't be pulled will be highlighted 
        3. Student reviews the courses and confirms them
            - System response: Saves the courses, shows a notification saying that the schedule has been accepted, and shows their filled in schedule
    - Alternate Flows:
        1. Student edits fields on a course
            - System response: Updates the course and removes any highlighted incomplete areas as the student fills in information
        2. Student wants to cancel schedule upon review
            - System response: Deletes pulled courses and navigates back to the schedule screen
        3. Student schedule has already been saved and wants to update information
            - System response: Allows student to edit fields and add new courses, and saves changes
        4. Student wants to remove courses
            - System response: deletes the courses, saves changes, and shows updated schedule
    - Exception Flows:
        1. Students course info cannot be pulled and is asked to manually enter information
            - System response: navigates to the manual entry section, provides autofill suggestions, and allows student to save changes
        2. Student tries to upload a file that isn't a PDF or is larger than 10MB
            - System response: rejects file and tells student to upload a PDF that is 10MB or less
    - Postcondition: Every course the student confirmed or entered is saved in their schedule. Courses cancelled during review are not saved and do not show up on course schedule
    - Given/When/Then:
        - Main Flow:
            - (AC-01.1)
                - Given: A signed in student that has consented to their schedule being used and has a PDF that is 10MB or less with 5 courses
                - When: The student selects the file
                - Then: withing 10 seconds shows all 5 courses and the information (name, professor, building, room, and time) matching the file
            - (AC-01.2)
                - Given: When reviewing, there are 5 courses shown
                - When: The student confirms
                - Then: A successful notification shows and their schedule shows the 5 courses and the information within 5 seconds
        - Alternate Flow:
            - (AC-01.3)
                - Given: The student has one saved course
                - When: The student edits the course, deletes it, and saves changes
                - Then: The course is removed from their schedule and shows no courses
        - Exception Flow:
            - (AC-01.4)
                - Given: The student is entering a course manually
                - When: The student begins typing the building name (first 3 charactes)
                - Then: within 2 seconds, autofill suggestions will be given for 1 to 5 possible building names they may be entering
            - (AC-01.5)
                - Given: The student is on the schedule screen
                - When: The student selects a JPEG file
                - Then: A error message shows asking for PDF only and 0 courses are saved

- UC-02: Add and cancel a campus event (Expands US-03)
    - Primary actor: Head of the student club
    - Secondary actors: The interested students
    - Preconditions to verify: 
        - Head of club is signed in and has been verified as head of the club
        - For cancelleation, A published event for the club exists and its start time is in the future
    - Main Success Flow:
        1. Head of club creates a new event
            - System response: Fields for name, time, location, and description
        2. Head of club enters information
            - System response: Highlights missed fields and will check that time is in the future. After it is saved, it sends a confirmation notification to head of   club and an event notification to interested students.
        3. Head of club opens a created event
            - System reponse: Shows event details, allows for edits, and how many students are interested
    - Alternate Flow:
        1. Head of club edits event, cancels it, anc confirms cancellation
            - System response: Marks event as cancelled and removes it from the event list and from schedules of interested student who added it. It will send a    cancellation confirmation to the head of the club and to thouse interested.
    - Exception Flow:
        1. Head of the club tries to add an event with missing info
            - System response: Higlights the missing items and doesn't add event until all info has been entered
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
    - Notes: Import, review, and manual entry can be split up later. Expanded in UC-01
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
    - Notes: Broad and it will be split by feature once screens are known
- US-05: 
    - Independent: Somewhat
    - Negotiable: Yes
    - Valuable: Yes
    - Estimable: Somewhat
    - Small: Yes
    - Testable: Yes
    - Notes: Needs schedule info from US-01 and an outside source for ratings, which alo makes it hard to estimate until we pick one


    