<p align="center"><img src="Images/docker.svg" height="200"></p>

# ACS 3220: Docker, DevOps, & Deployments

<span class="refresh-instructions">This syllabus is a living document! Hold down the `SHIFT` key and press `Refresh` to get the latest version.</span>

<!-- omit in toc -->
## Table of Contents

1. [Course Description](#course-description)
1. [Prerequisites](#prerequisites)
1. [Course Specifics](#course-specifics)
1. [Learning Outcomes](#learning-outcomes)
1. [Schedule](#schedule)
1. [Class Assignments](#class-assignments)
   1. [Tutorials](#tutorials)
   1. [Challenges](#challenges)
   1. [Final Project](#final-project)
1. [Evaluation](#evaluation)
1. [Class Recordings](#class-recordings)

## Course Description

In this course students will learn the two main flavors of Developer Operations (DevOps), one that uses containers and one that does not. Students will learn the leading container pattern with Docker and explore the pros and cons of containers by implementing them. The course will tie this pattern together with generic patterns of operation, such as environmental design, development controls, and uptime management.

## Prerequisites

- [ACS 1120](https://bit.ly/acs1120)

## Course Specifics

**Course Delivery**: online | 7 weeks | 14 sessions<br>
**Course Credits**: 3 units | 37.5 Seat Hours | 75 Total Hours

## Learning Outcomes

_By the end of this course, you will be able to&hellip;_

1. Build, deploy, and orchestrate that containers in any web-based application.
1. Compare and contrast traditional and container-based deployment patterns.
1. Demonstrate proficiency for professional DevOps roles.
1. Implement and maintain a cloud infrastructure utilizing traditional DevOps techniques.

## Schedule

**Course Dates:** Wednesday, August 17 - Wednesday, October 5, 2022<br>
**Class Times:** Monday, Wednesday at 4:00 PM PST - 6:45 PM PST

| Class |   Date   | Topic                                                                                        |     |
|:-----:|:--------:| -------------------------------------------------------------------------------------------- | --- |
|   1   |  Aug, 17 | [Course Orientation] & 🔬 **Lab**: [Scripting in Bash]                                       |     |
|   2   |  Aug, 22 | [How Containers Work]                                                                        |     |
|   3   |  Aug, 24 | [Domains & DNS]                                                                              |     |
|   4   |  Aug, 29 | [Dockerizing Web Apps] + [Dockerizing Your Web App]                                          |     |
|   -   |  Sep, 5  | [Docker Compose] + 📝 **Lab**: [Review Worksheet]: _Containers, Orchestration, Optimization_ |     |
|   5   |  Sep, 7  | [Domains & Droplets]                                                                         |     |
|   6   | Sep,  12 | [Your Personal PaaS]                                                                         |     |
|   7   |  Sep, 14 | [Alerts] + [Continuous Integration]                                                          |     |
|   8   |  Sep, 19 | [Project Kickoff]                                                                            |     |
|   9   |  Sep, 21 | [Advanced Container Orchestration Techniques]                                                |     |
|   10  |  Sep, 26 | [Docker Volumes]                                                                             |     |
|   11  |  Sep, 28 | NEW: [Docker Swarm]                                                                          |     |
|   12  |  Oct, 3  | TBA                                                                                          |     |
|   13  |  Oct,  5 | [Course Review]                                                                              |     |

## Class Assignments

### Tutorials

To access each tutorial, click the links below. Be sure to complete the exercise in your browser and follow each instruction carefully.

**_Complete all five tutorials before the midterm_**:

| Name                                                                                                              | Description                                                |
| ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| **Tutorial 1**: [Your First Linux Containers](https://training.play-with-docker.com/ops-s1-hello)                 | _Explore Docker fundamentals._                             |
| **Tutorial 2**: [Deploy and Manage Multiple Containers](https://training.play-with-docker.com/ops-s1-swarm-intro) | _Scale like the pros using Swarm and Compose._             |
| **Tutorial 3**: [Doing More with Docker Images](https://training.play-with-docker.com/ops-s1-images)              | _Create & share Docker images; examine the layers inside._ |
| **Tutorial 4**: [Docker Networking Lab](https://training.play-with-docker.com/docker-networking-hol)              | _Hands-on practice using key Docker Networking concepts_.  |
| **Tutorial 5**: [Docker Orchestration Lab](https://training.play-with-docker.com/orchestration-hol)               | _Hands-on practice in high-availability techniques._       |


Knowledge gained in the tutorials will be evaluated on the [Containers, Orchestration, Optimization](https://www.gradescope.com/courses/203051/assignments/835995) worksheet as well as participation during the final [Course Review].

### Challenges

Gain valuable real-world experience in DevOps through these hands-on activities.

**_Complete all five challenges to pass the course_**:

| Name                                                 | More Info                                     |
| ---------------------------------------------------- | --------------------------------------------- |
| **Challenge 1**: Your Own Domain Name                | [Instructions](Projects/Challenges.md)        |
| **Challenge 2**: Connect Your Domain to GitHub Pages | [Instructions](Guides/InfiniteGithubPages.md) |
| **Challenge 3**: Dockerize Any Flask Application     | [Instructions](Projects/Challenges.md)        |
| **Challenge 4**: Your First Server                   | [Instructions](Projects/Challenges.md)        |
| **Challenge 5**: Compose a Web Application           | [Instructions](Projects/Challenges.md)        |


### Final Project

**In your final project, you'll add the following features to an existing codebase of your choosing**:

- **Develop** a `Dockerfile` and `docker-compose.yml` file that successfully deploys any web application or open source project.
- **Deploy** your application's image to your own cloud production server.
- **Monitor** your application’s uptime using a health check.
- **Demonstrate** confidence writing and speaking about Docker, DevOps, and deployment topics by summarizing the experience in a blog post.

**_Review the [Requirements Document](Projects/FinalProject.md) to learn more about the project, including_**:

- [⭐️ Project Goals](#%e2%ad%90%ef%b8%8f-project-goals)
- [📋 Project Requirements](#%f0%9f%93%8b-project-requirements)
- [🗓 Deliverables & Due Dates](#%f0%9f%97%93-deliverables--due-dates)
  - [2️⃣ **Blog Post**: Due December 10 @ 11:59pm](#2%ef%b8%8f%e2%83%a3-blog-post-due-129--1159pm)
  - [3️⃣ **Repository**: Due December 10 @ 11:59pm](#3%ef%b8%8f%e2%83%a3-repository-due-129--1159pm)

## Evaluation

**To pass this course, you must**:

- **Complete all five tutorials** listed in the [**Tutorials**](#tutorials) section.
- **Complete all five DevOps challenges** listed in the [**Challenges**](#challenges) section; each submitted on [Gradescope] when complete.
  - URL-based deliverables must return a valid response to earn full credit.
  - Docker-based deliverables that successfully build and run to earn full credit.
- **Submit a [final project](Projects/FinalProject.md) on [Gradescope]** and pass according to the [rubric](Projects/FinalProject.md#rubric).
- Actively **participate in class** and **abide by the attendance policy**.
- **Make up all classwork** from all absences.

## Class Recordings

All class recordings will be available [here](https://bit.ly/droxey-vids) no later than 24 hours after the class session. For privacy reasons, please do not share the recordings outside of the ACS student body.

[Alerts]: Lessons/Alerts.md
[Architecture Diagrams]: Lessons/Diagrams.md
[Code Once, Run Anywhere]: Lessons/Containers.md
[Continuous Integration]: https://docs.google.com/presentation/d/18DNt9UXHaPUufQogj-mThiKpvhkJzXprnPmQtaptUp8
[Course Orientation]: Lessons/CourseOrientation.md
[Docker Compose]: Lessons/Compose.md
[Docker Hub]: Lessons/Hub.md
[Docker Swarm]: Lessons/Swarm.md
[Dockerizing Web Apps]: Lessons/WebServers.md
[Dockerizing Your Web App]: Lessons/WebServers.md#60m--lab-writing-dockerfiles
[Domains & DNS]: Lessons/DNS.md
[Domains & Droplets]: Lessons/Droplets.md
[Final Presentations]: Projects/FinalProject.md#Deliverables
[Final Project]: Projects/FinalProject.md
[Gradescope]: https://www.gradescope.com/courses/203051
[How Containers Work]: Lessons/Dockerfiles.md
[Multi-Stage Builds]: Lessons/Builds.md
[Networking]: Lessons/Networking.md
[Project Kickoff]: Projects/FinalProject.md
[Review Worksheet]: https://www.gradescope.com/courses/203051/assignments/835995
[Scripting in Bash]: https://github.com/veltman/clmystery
[Security]: Lessons/Security.md
[Volumes]: Lessons/Volumes.md
[Your Personal PaaS]: Lessons/PaaS.md
[Docker Volumes]: Lessons/Volumes.md
[Advanced Container Orchestration Techniques]: Lessons/AdvancedOrchestration.md
[Course Review]: Lessons/CourseReview.md
