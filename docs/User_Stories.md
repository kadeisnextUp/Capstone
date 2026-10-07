# User Stories and use cases
**Team members:** Laila Raines, Kaden Sawyer


US-01: Importing semester schedule 
    As a student, I want to import my schedule from a file so I don't have to manully input them.
    
    - Given a valid PDF file, when I upload it, all necessary information (Course name, professor, building name, room number, and meeting times) appears correctly.
    - Given a file that isn't PDF, when I upload it, I get an error telling me that the only valide file type is PDF.
    - Given a valid PDF file with no courses, when I upload it, I am told there is no information and given the opportunity for manual entry.

    

US-02: Review and correct imported courses
    As a student, I want to be able to review and correct anything that may have been imported wrong by the parser.

    - Given a imported schedule, when I view it, I want to be able to edit any information before confirming.
    - Given a course with missing information, when I view it, it should be noticeably flagged as incomplete.
    - Given I cancel while reviewing, when I cancel confirmation, no course information is saved.


US-03: Modify after a schedule change
    As a student, I want to make changes to my selected courses in a event of a schedule change to keep accuracy.

    - Given a existing schedule, when I import, I have the option to merge, replace courses or update information for a course.
    - Given I choose to replace, when it completes, only new courses remain.
    - Given a course was dropped, when I remove it, it does not appear again anywhere in the app.



US-04: Manually select courses
    As a student, I want to be able to input the courses if a import fails so I can still use the app.

    - Given a failed import, when I am on the schedule screen and choose add course, then I can enter Course name, professor, and meeting times.
    - Given I enter a building name, when I type, then i see autocoomplete suggestions from campus building data.
    - Give when some fields are blank, when I save, I can see which information is missing.

US-05: Increase awareness of Campus activites
    As a student club/organization, I want to be able to input events so users of the app can view it.

    - Given a new event, I want to be able to enter the necessary information ( Event name, Meeting time, location, and short description).
    - Given a event cancelation, I want to be able to notify students who were interested in it.
    

US-06: Aware of professor rating
    As a student, I want to know if the professor for the course I am taking has a good rating by other students.

    - Given a existing schedule, when I view the professor name, I should a numberic rating next to it.
    - Given I complete a course, when I click the professor name, I am taken to ratemyprofessor.

US-06: Knowledge of what's going on on campus
    As a student, I want to know what is going on on campus at any given time.

    - Given I have an exisiting schedule and I open the app, when I view campus events, I am shown events that don't conflict with my schedule (but can toggle on/off
    events that do conflict and show which course)
    - Given I have an interest in a event, when I view the event, I have the option to select interest in the event so I can stay updated.

US-07: 
