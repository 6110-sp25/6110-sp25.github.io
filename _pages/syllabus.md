---
title: Syllabus
layout: home
nav_order: 10
---

<h1> Computer Language Engineering (6.110) </h1>

{% include toc.html %}

## Changelog

- **2/5/25**: Changed recitation location from TR (32-124) to TR (32-155).
- **2/11/25**: Changed recitation period in catalog description to be from 32-124 to 32-155.

## Basic Information

### MIT Catalog Description

> [6.1100<sub>\[6.035\]</sub> Computer Language Engineering][catalog] \\
> **Prereq:** [6.1020<sub>\[6.031\]</sub>][031] and [6.1910<sub>\[6.004\]</sub>][004] \\
> **Units:** 4-4-4 \\
> **Lecture:** _MWF11_ ([32-123][bldg32]) Recitation: _TR12_ ([32-155][bldg32])
>
> Analyzes issues associated with the implementation of higher-level programming languages. Fundamental concepts, functions, and structures of compilers. The interaction of theory and practice. Using tools in building software. Includes a multi-person project on compiler design and implementation.

### Course Staff

#### Faculty

- [Martin Rinard](https://people.csail.mit.edu/rinard/) (<rinard@csail.mit.edu>)

#### Teaching Assistants

- Chengyuan Ma (<macy404@mit.edu>)
- Youran (Yoland) Gao (<youran@mit.edu>)
- Shruti Siva (<shrutsiv@mit.edu>)
- Kosi Nwabueze (<kosinw@mit.edu>)
- Felix Prasanna  (<fpx@mit.edu>)

Please contact the course staff by posting on [Piazza][piazza] or emailing <6.110-staff@mit.edu>.

### Prerequisites

6.1100 is designed for students with an intermediate to advanced level of programming maturity. The compiler project is a multi-week, multi-person software engineering project. Some exposure to low-level programming and assembly is recommended. Some exposure to algorithms may be helpful for the project.

### Communication

- We will distribute assignments here and make all announcements via [Piazza][piazza]. Important announcements will also be made via email.
  - Office hours details will be announced on [Piazza][piazza].
  - Since lecture dates are not all finalized at the start of the semester, please check the [schedule][schedule] regularly.
- For all general questions and/or concerns, please post publicly on [Piazza][piazza]. If the matter is private, please post privately on [Piazza][piazza] or email the course staff at <6.110-staff@mit.edu>.
- Weekly check-ins need to be completed via [Gradescope][gradescope]. The join code is __<u>XGP4YW</u>__.

### Recommended Texts

6.110 has no officially required textbook. All of the material you need is taught in class, with the exception of the documentation for your implementation language and associated libraries.

We can point you to many resources such as well-known textbooks, technical papers, interesting and useful blog posts, and reference guides. These are available on the [Resources][resources] page.

## Course Components

We want you to succeed in this course. You should be aware of the following components of the course and utilize them for your benefit.

### Compiler project

The main component of this course (75% of the grade) is a project where you will build a compiler almost entirely from scratch. The [first phase][phase_1] (lexing and parsing) of the project will be done individually, and the rest of the project will be done in groups of 3-4.
Details about the project can be found on the [Project Overview][project] page. Specific instructions for each phase of the project will be released later in the class.

### Class meetings

Nominally, class meetings are one hour long and take place at ~~32-124~~ (**see updates below**) at the following times:

- At 11 a.m. on Mondays, Wednesdays, and Fridays in **<u>32-123</u>**
- At 12 p.m. (noon) on Tuesdays and Thursdays in **<u>32-155</u>**

_Lectures_, which will cover the fundamental concepts and structures of compilers, will be held on Mondays through Thursdays, and _recitations_, which will focus more on the project, will be held on Fridays. (Please *disregard* the official listing of "lectures" versus "recitations".)

__Please check the [schedule][schedule] regularly.__ Lecture dates are not all finalized at the start of the semester. Some lectures may be canceled or moved depending on the class's comfort and progress on the projects.

There will be weeks that have few or no lectures, especially as we draw closer to project deadlines. (Last year, there were around 7 weeks of lectures in total.) We will also announce urgent changes on [Piazza][piazza].

#### Re-lectures

If you are unable to attend a lecture, there are recorded re-lectures videos from 2024 that we can provide to you that cover the content that you miss.

### Weekly Check-ins

In an effort to ensure your project stays on track, your team should submit a short weekly check-in on [Gradescope][gradescope]. We will provide a weekly "mini-quiz" graded for completion only. They are useful for checking your understanding of the lecture material and also serve as good preparation material for the quizzes.

### Quizzes

Two quizzes, each worth 10%, will be held during class time on **March 14th** and **May 2nd**.
More information about quizzes, including practice material, will be released closer to the quiz dates.
If you have a conflict with the quiz dates, please let the course staff know as early as possible.

### Office hours

See the "Office Hour" page on the website. We will try to have more Office Hours during weeks with phase deadlines.

If none of the office hours times work for you, please reach out to us at <6.110-staff@mit.edu>.

## Policies

### Grading

#### Option A

| Component                                                    | Weight |
| ------------------------------------------------------------ | ------ |
| Project phase 1 (lexing and parsing)                         | 5%     |
| Project phase 2 (IR and semantic checking)                   | 5%     |
| Project phase 3 (code generation)                            | 15%    |
| Project phase 4 (dataflow optimization)                      | 10%    |
| Project phase 5 (register allocation and other optimizations) <br/>and final report/submission | 40%    |
| Quiz 1                                                       | 10%    |
| Quiz 2                                                       | 10%    |
| Participation (Weekly check-in, miniquizzes, checkpoint meetings) | 5%     |

#### Option B

| Component                                                    | Weight |
| ------------------------------------------------------------ | ------ |
| Final project report + final submission                      | 75%    |
| Quiz 1                                                       | 10%    |
| Quiz 2                                                       | 10%    |
| Participation (Weekly check-in, miniquizzes, checkpoint meetings) | 5%     |

Your grade will be the higher of Option A or Option B, and you do not need to inform us ahead of time which option you are taking. 

Although Option B allows you to pass the class by submitting a working compiler by the Phase 5 deadline, we **strongly encourage** you to follow the phase 1/2/3/4 deadlines, as the compiler is a non-trivial software system that will take a significant amount of engineering time on your part. Before submitting your team preference form, make sure your team members have agreed on a timeline for completing the project.

For more information on the way the compiler project is graded, see the [Project Overview][project].

### Late policy

We expect you to submit all components of the class on time. For extensions under extenuating circumstances (e.g., a member of your team is sick, family emergencies), we require a letter from one of the student deans at [Student Support Services (S<sup>3</sup>)][s3].

### Collaboration policy

While you may discuss the high-level approaches to the project with anybody, you must develop your code within your team (or by yourself for [phase 1][phase_1] of the project). In particular:
- You _are allowed_ to use reference material available online, as well as resources and existing libraries, as long as you cite them in your project reports, and they don't trivialize the project. (Please use your best judgment here, and ask the course staff if you are unsure.) If you decide to use larger code snippets, please also explain how you adapted and used them in your project report.
- You _are allowed_ to use LLM-generated code. Make sure to read [the LLM policy](#llm-policy) below.
- You _may not_ share any code with other teams.
- You _may not_ post your project code on publicly accessible websites or file spaces, including public GitHub repositories.

### LLM policy

We encourage you to try to use LLM-generated code in your project. Your use of LLM generated code will have no effect, either positive or negative, on your grade. To cite one extreme example, if you type "Can you please generate a compiler for the MIT Decaf programming language" into the prompt box of an LLM and get a full compiler back (we have tried this and it does not seem to work), you will get full credit if the compiler satisfies the course requirements.

If you decide to use LLM-generated code, you should explain your general approach to using LLMs in your project report.
We also ask that all teams document their use of LLM generated code via a LLM code usage survey turned in three days after each project phase deadline. This survey will be made available on Gradescope.
One easy way to record your interactions with LLMs is, for example, the "Share link to Chat" feature that ChatGPT provides.

[004]: https://student.mit.edu/catalog/m6a.html#6.1910
[031]: https://student.mit.edu/catalog/m6a.html#6.1020
[bldg26]: http://whereis.mit.edu/map-jpg?mapterms=26
[bldg32]: http://whereis.mit.edu/map-jpg?mapterms=32
[catalog]: https://student.mit.edu/catalog/m6a.html#6.1100
[github]: https://github.com/6110-sp25/
[gradescope]: https://www.gradescope.com/courses/931853
[piazza]: https://piazza.com/mit/spring2025/6110/home
[s3]: https://studentlife.mit.edu/s3
[phase_1]: {% link _pages/project/phase_1.md %}
[quizzes]: {% link _pages/quizzes.md %}
[miniquizzes]: {% link _pages/miniquizzes.md %}
[resources]: {% link _pages/resources.md %}
[schedule]: {% link _pages/schedule.md %}
[project]: {% link _pages/project.md %}
