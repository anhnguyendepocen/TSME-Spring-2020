Time Series Models in Ecology  
Syllabus
================
Spring 2020

### Course Information

Spring 2020  
OCS 7001, Section 5  
Dr. Steve Midway, <smidway@lsu.edu>  
Dr. Mike Dance, <mdance1@lsu.edu>

### Meeting time

Monday 11–11:50am, ECE 2215

### Course Text

There is no required text for this course. However, there are a number
of useful texts that I recommend knowing about as references.
Additionally, there will be some papers and book chapters to read for
different weeks. Readings will be freely available or available through
your access to the LSU Library.

Recommended texts (may be available for free electronically)

  - *Introduction to WinBUGS for Ecologists*, Marc Kery
  - *Data Analysis Using Regression and Multilevel/Hierarchical Models*,
    Gelman and Hill

My teaching materials will be compiled into a bookdown document that
will serve as the course text, to which I may refer. Please access the
course text via this link, and know that the material will be added and
developed often.

Course text: <https://bookdown.org/steve_midway/BHME>

### Prerequisites

It is strongly encouraged that you have basic to intermediate
statistical experience, and essential that you are comfortable using
`R`. For any specific questions about course preparation, please see the
instructor.

### Computing

We will be working in a conference room (not a computer lab), and you
are expected to bring a laptop to class. Although there may be days we
do not use computers, it is best to anticipate using one, or to ask the
prior class period about the need. Computers should be loaded with
program `R`, ideally run through R Studio. You will also need to have
the program JAGS loaded, although we will run it through R and not work
directly in JAGS.

  - Program R: <https://cran.r-project.org/>
  - R Studio: <https://www.rstudio.com/>
  - JAGS: <http://mcmc-jags.sourceforge.net/>

### Course Objectives

1.  Understand the need for hierarchical models used with ecological
    data
2.  Be able to develop, code, and interpret hierarchical models and
    their output
3.  Become familiar with Bayesian approaches to statistics

### Course Structure

This class will meet twice per week. We will work through a number of
topics that are designed to help you become familiar with hierarchical
models and Bayesian estimation procedures, such that you can eventually
run these models on your own. Class time may be entirely lecture,
entirely coding exercises, literature discussions, or any combination of
these (and other) activities. I anticipate that we will get enough time
in lecture for coding, which is a critical part of learning these
models; however, we may develop a side “lab” or supplemental time during
which additional coding may take place.

### Classroom Rules

Students are expected to adhere to LSU policy concerning conduct.
Plagiarism or cheating will not be tolerated. See the current LSU
catalogue for details. If you copy from one of your classmates, your
score for the activity in question will be zero (= no credit).
(<http://saa.lsu.edu/code-student-conduct>)

Eating and drinking is not allowed in classroom.

Computers, iPads etc. are allowed for note taking exclusively. Answering
cell phone calls and engaging in conversation is not allowed, unless
needed in cases of emergency.

All course and college communication will be via LSU (lsu.edu) email
addresses. Students are responsible for regularly checking their email.
Immediately contact the IT Help Desk 225-578-3375 (578-DESK)
(<http://grok.lsu.edu/categories.aspx?parentcategoryid=2787>) if there
are problems with your email.

### Exam Policy

Students are required to take all exams. If a student misses an exam due
to extenuating and unavoidable circumstances, the instructor MAY provide
an opportunity for the student to take the exam, at the instructor’s
discretion, provided: 1) the student has made advance notice of the
situation to the instructor, if possible, prior to the exam, 2) the
student provides written documentation to the instructor that is deemed
adequate, and 3) the exam can be rescheduled and taken within a five
business-day period. “Extenuating circumstances” are defined for the
purposes of this syllabus to include the following: (documented) serious
accidents to the student or immediate family, serious illness or medical
procedure, military duty, religious holidays, and catastrophic events
such as fire or storm damage.

In the event that the student misses more than one exam for any of the
reasons described above, it may be necessary to request an “incomplete”
if the specific circumstances warrant that action (see the current
Student Handbook for details.)

### Attendance

Attendance may be documented. Both tardiness and early departure are
considered absenteeism. Students are expected to return to class
following a break in instruction. Failure to attend class may jeopardize
a student’s scholastic standing.

### Student Withdrawal from Course

Students are responsible for withdrawing from this course on or before
the withdrawal deadline as listed in the LSU Academic Calendar.

### Communication

All course and college communication will be via LSU email addresses.
Students are responsible for regularly checking their email. Immediately
contact the IT Help Desk if there are problems with your email.

### Accommodations

LSU makes reasonable accommodations for persons with documented
disabilities. Please discuss any accommodations with me during the first
two weeks of class so that I may assist with your learning needs.

### Student/Faculty issues

Students are expected to address classroom issues in a timely manner and
to follow the LSU Student Handbook process for resolving classroom
issues.

### Campus safety

A student’s safety is important in the learning process. Please report
any suspicious activity to the LSU Police at 225-578-3231 (or 8-3231
from any on-campus telephone or Campus Call Box). Sign up for the
Campus-wide emergency text messaging service, by following the link
<https://www.lsu.edu/vetmed/publications/safety_policies_and_procedures.pdf>
to enter your contact information. More safety information is posted on
the LSU Police webpage. You should also familiarize yourself with the
Emergency Response Plan posted in each hall. If necessary, please exit
quickly, and once outside continue to a safe distance away from the
building. Take your possessions with you.

### Assignments

  - Exercises 1–4, 40% (10% each)
  - Literature reviews, 20% (10% each)
  - Research paper, 30%
  - Participation, 10%

### Grades

| Grade |                |               |                |
| :---- | -------------- | ------------- | -------------- |
| A     | 97 to 100: A+  | 93 to \<97: A | 90 to \<93: A- |
| B     | 87 to \<90: B+ | 83 to \<87: B | 80 to \<83: B- |
| C     | 77 to \<80: C+ | 73 to \<77: C | 70 to \<73: C- |
| D     | 67 to \<70: D+ | 63 to \<67: D | 60 to \<63: D- |
| F     | 0 to \<60: F   |               |                |

### Calendar of Material

The following topics are approximately in the order we will cover them
as many are concepts that will build on previous work. We may make
modifications to the topics as the course materials develop, and we will
devote varying amounts of time to each topic, based on interest and
capacity.

| Date           | Topic                                  | Reading(s) |
| :------------- | :------------------------------------- | :--------- |
| M August 26    | Introduction                           |            |
| W August 28    | What are hierarchical models?          |            |
| M September 2  | *No Class (Labor Day)*                 |            |
| W September 4  | Introduction to Bayesian estimation    |            |
| M September 9  | Shrinkage and centering                | 1          |
| W September 11 | Priors                                 | 2          |
| M September 16 | Intro to working in JAGS               | 3          |
| W September 18 | Intro to working in JAGS               | 4          |
| M September 23 | Varying intercept models               | 5          |
| W September 25 | Varying intercept models               | 6          |
| M September 30 | *No Class (Midway at conference)*      |            |
| W October 2    | *No Class (Midway at conference)*      |            |
| M October 7    | Varying slopes models                  | 7 and 8    |
| W October 9    | Varying slopes models                  | 9 and 10   |
| M October 14   | Varying coefficients models            | 11         |
| W October 16   | Varying coefficients models            |            |
| M October 21   | Modeling varying coefficients          |            |
| W October 23   | Modeling varying coefficients          | 12         |
| M October 28   | Non-linear hierarchical models         | 13         |
| W October 30   | Non-linear hierarchical models         | 14         |
| M November 4   | Generalized linear hierarchical models |            |
| W November 6   | Generalized linear hierarchical models |            |
| M November 11  | Model selection                        | 15         |
| W November 13  | *No Class (Midway at conference)*      |            |
| M November 18  | *No Class (Midway at conference)*      |            |
| W November 20  | Flex                                   |            |
| M November 25  | *No Class*                             |            |
| W November 27  | *No Class*                             |            |
| M December 2   | Site-Occupancy Model                   | 16         |
| W December 4   | Binomial Mixture Model                 |            |

## Readings

1.  Stephens, P. A., Buskirk, S. W., & del Rio, C. M. (2007). Inference
    in ecology and evolution. Trends in Ecology & Evolution, 22(4),
    192–197.
2.  Link, W. A., & Barker, R. J. (2009). Chapter 1 in *Bayesian
    inference: with ecological applications*. Academic Press.
3.  Kruschke, J. K. (2013). Bayesian estimation supersedes the *t* test.
    Journal of Experimental Psychology: General, 142(2), 573.
4.  Lele, S. R., & Dennis, B. (2009). Bayesian methods for hierarchical
    models: are ecologists making a Faustian bargain? Ecological
    Applications, 19(3), 581–584.
