---
layout: page
title: Syllabus
catalog: BIO 470
credits: 1.5
semester: Spring 2025
professor: Dr. Gregory Owens (he/him)
office: Cunningham 040
email: grego@uvic.ca
schedule: ['Tuesdays, 1:30-2:20 pm PT', 'Wednesdays, 2:30-5:20 pm PT']
location: ['Cunningham 146 (Tuesday)', 'Clearihue A103 (Wednesday)']
---

## Course

{{ site.title }}

{{ page.catalog }}, {{ page.credits }} Credits, {{ page.semester }}

### Instructor

{{ page.professor }}

Office: {{ page.office }}

Email:
[{{ page.email }}](mailto:{{ page.email }})



### Location

Lecture (Tuesdays): Cunningham 146
Lab (Wednesdays): Clearihue A103


### Times

{% for class in page.schedule %}
  {{ class }}
{% endfor %}


### Office Hours

By appointment. Email me and we can find a time. I will also schedule a review session before each exam. 


### Website

The syllabus and other relevant class information and resources will be posted
at [{{ site.url}}]({{ site.baseurl }}/).
Changes to the schedule will be posted to this site so please try to check it
periodically for updates. 

The online version of this syllabus will be updated to clarify points, but the version
of record is saved [here]({{ site.baseurl }}/syllabus_current.pdf).


### Course Communications

Brightspace: Messages in brightspace will be the primary way I contact you.

Email: [{{ page.email }}](mailto:{{ page.email }})


### Required Texts

There is no required text book for this class. All primary literature will be available on Brightspace.

All needed material is openly available on the course website. Additional texts will be linked in individual 
lessons if you're interested in the material.


### Course Description

An introduction to computational genomics. Class consists of a short introduction
to commandline programming and R, and then include practical excercises in genomics.
No background in computing is required.


### Prerequisite Knowledge and Skills

Knowledge of basic biology and genetics. Completion of Biol 230. 


### Purpose of Course

In this course you will learn the fundamentals of how genomic data is generated 
and analyzed. You will have practical examples of code to run specific genomic 
techniques and have the skills to visualize the results. You will also have better
ideas on how to plan future genomic projects.


### Course Intended Learning Outcomes

Students completing this course will be able to:

* Navigate on unix command line.
* Use R for data wrangling and plotting.
* Understand how genomic data is generated.
* Run command line bioinformatic programs.
* Explain the principles behind genome assembly and variant detection.
* Turn sequence data into genetic variation.
* Run basic population genetic analyses.
* Present scientific analyses.




### Instructional Methods

Class is divided into lectures on Tuesday and labs on Wednesday. The lecture will
cover fundamental and technical aspects of genomic methods and analyses. Before each 
lecture students will be required to read a primary literature article on the topic 
and submit answers to questions on Brightspace. The lab will involve demonstrations by 
the instructor, code along sections and independent or group work. While students are working on exercises the
instructor will actively engage with students to help them understand material
they find confusing, explain misunderstandings and help identify mistakes that
are preventing students from completing the exercises, and discuss novel
applications and alternative approaches to the data analysis challenges students
are attempting to solve.

## Course Policies

### Attendance Policy

Attendance to the lecture portion is highly encouraged. Lectures will be recorded and 
posted on Brightspace. Given the central importance of 
the lab exercises, attendance for the lab portion is mandatory and recorded. Missing more than 
two (2) lab sections will result in an incomplete grade in the course. For missed lab section,
the student should complete the tutorial on their own time. 
Even when the lab section is missed, the lab assignment is still due the following week.


### Quiz/Exam Policy

There will be two exams in this course. One on the week of the February 14th, the second 
on the week of April 3rd. Both exams will be in person during class time and are not open book. Exams will not be cumulative, although concepts and material may span both exam periods. 

Dr. Owens will talk with each student who has a CAL accommodation to plan an exam that works for them.



### Make-up policy

Life happens and therefore there is an automatic grace period of 24 hours for
the submission of late assignments with no need to request an extension.
However, it is highly recommended that you submit assignments on time when
possible because assignments build on one another and it can be hard to catch up
if you fall behind. Reasonable requests for longer extensions will also be granted.
Assignments turned in after the 24 hour grace period without an extension will be
be graded with a 20% penalty, and not accepted one week after the initial deadline.


### Assignment policy

Lecture assignments will be due roughly every 3 weeks, and will be announced in class and online. 
Assignments will focus on lecture material and will involve reading and interpreting primary literature. 
In lab assignments will cover the same material as the lab activities and will demonstrate understanding 
and expertise. They are available on brightspace. 

### Final group project

In teams of two (2), students will analyse a simulated genomic dataset to understand,
how populations are related, and identify the gene causing a trait. This will involve applying 
computational techniques learned in the lab to develop a custom workflow. The project will be marked
based on the results, their interpretation and an annotated code document. The final project is due
the week of April 7th, exact date to be determined. Detailed rubric and instructions will be released
on Brightspace.

### Grade policy

| Item    | Percentage |
| -------- | ------- |
| Lecture assignments (4) | 20%  |
| In lab assignments (9) | 27% |
| Mid term Exam 1 | 15%     |
| Mid term Exam 2 | 15% |
| Final Group Project     | 23%    |

**To pass the course**, students must:
1. Attend the lab section for at least 11 out of 13 weeks.
2. Complete the final group project.
3. Complete both midterm exams.
4. Score a final grade of 50.0 points for the entire course.
  

*If any of 1 through 3 are not completed, the student will automatically fail the course and receive an “N” 
(‘Incomplete’) on their transcript. If a student successfully completes 1 through 2, 
but is not successful in 4, they will receive an “F” on their transcript.*


### Course Technology

Students are required to work on Apple computers for the lab portion of the course. 
Apple computers are available in the lab space, and it is recommended that students 
use those and not personal laptops. Outside of lab time, apple computers can
be used in the library. Windows computers can complete the lab assignments
but the file system differences between operating systems can cause problems and 
complexity. 

### Use of AI in the course

AI use is a challenging topic. In the future, much of the code in genomics
may be written by AI so understanding how to use AI effectively is important.
That being said, the course is designed to teach you genomics and programming
skills, not chatGPT. Here is my general policy:
1. For in class activities, *do not* use AI to directly answer the questions. 
The effort to figure out these problems is part of the learning activity. 
2. *Do* use AI to better understand parts of the course material. For example,
you could ask it to explain what each part of an example script does. 
3. *Do not* use AI to write your assignments or projects. All material handed in needs to be written by you.
 Consider AI as a friend who 
has already graduated and knows the material. You wouldn't ask your friend 
to write your assignments, that would be academically dishonest. You can use
AI to ask background questions around the assignment topic. 
4. *Do not* use AI to edit your assignments or projects. This would be considered
the same as AI writing your assignment.

Ultimately, you need to write all of the text and code you hand in for assignments
and projects.

### Materials and Supplies Fees

There are no materials and supplies fees for this course.

## Graduate Students
Graduate students taking Computational Genomics as a directed studies course have several differences from undergraduate students.
1. Graduate students will not take the exams.
2. Graduate students will work complete a bioinformatic project related to their thesis.
3. Graduate students will present a lecture on a topic relevant to the class, either solo or in a team depending on time availability.

| Item    | Percentage |
| -------- | ------- |
| Lecture assignments (4)  | 20%    |
| In lab assignments (9) | 27% |
| Lecture | 20% |
| Final Solo Project     | 33%    |

## UVic Policies


### Territory Acknowledgement

We acknowledge and respect the lək̓ʷəŋən peoples on whose traditional territory the university stands, 
and the Songhees, Esquimalt and WSÁNEĆ peoples whose historical relationships with the land continue to this day. 

### Important Dates

Note these important dates:  
* Last day for 100% reduction of tuition fees for standard courses – Jan. 19  
* Last day for adding second term courses  – Jan 22 
* Last day for 50% reduction of tuition fees for standard courses – Feb. 9  
* Last day for withdrawing from 2nd term courses without penalty of failure –  Feb. 28 

Holidays (no class):
Tues. Feb 18 – Reading Break
Wed. Feb. 19 – Reading Break


### Medical documentation

Medical documentation for short-term absences is not required. Attendance is important.  Students who cannot attend due to illness are asked to notify their instructors immediately. If you miss (or know beforehand that you will be missing) a test because of illness, accident, family affliction, you are required to contact the appropriate instructor in a timely manner after the test (normally within seven calendar days).  
Policies regarding undergraduate student academic concessions and deferrals are also detailed here: https://www.uvic.ca/registrar/students/appeals/acad-concession/index.php   


### University Policy on Academic Misconduct

Students are required to abide by all academic regulations set as set out in the University calendar, including standards of academic integrity. Violations of academic integrity (e.g. cheating and plagiarism) are considered serious and may result in significant penalties. 
 
It is absolutely essential that you understand and uphold academic integrity. To this end, I ask you to please read UVic’s framework on academic integrity, which you can find here: 
https://www.uvic.ca/students/academics/academic-integrity/index.php 
 
You can also find UVic’s Policy on Academic Integrity in the UVic calendar: https://www.uvic.ca/calendar/undergrad/index.php#/policy/Sk_0xsM_V 
 
To help avoid plagiarism and cheating, please read the UVic Libraries’ plagiarism guide: https://www.uvic.ca/library/research/citation/plagiarism/  
 
I reserve the right to use plagiarism detection software or other platforms to assess the integrity of student work. 
 
Please see read UVic’s Student code of conduct and standards for professional behaviour: https://www.uvic.ca/services/advising/advice-support/academic-units/student-codeof-conduct/index.php 


## Grading Policies

In lab assignments will be graded as follows for coding based questions:

* Produces the correct answer using the requested approach: 100%
* Generally uses the right approach, but a minor mistake results in an incorrect
    answer: 90%
* Attempts to solve the problem and makes some progress using the core concept, but returns the wrong answer and does not demonstrate comfort with the core concept:
    50%
* Answer demonstrates a lack of understanding of the core concept: 0%


### Grading scale

- **A+ 89.5-100**
- **A 84.5-89.4**
- **A- 79.5-84.4**
- **B+ 76.5-79.4**
- **B 72.5-76.4**
- **B- 69.5-72.4**
- **C+ 64.5-69.4**
- **C 59.5-64.4**
- **D 50.0-59.4**
- **F <50.0**

