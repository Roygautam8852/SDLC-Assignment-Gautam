# Student Attendance App

## Introduction

A university startup is developing a **Student Attendance Application** that automatically marks attendance when students enter a classroom.

The application initially uses **Geo-fencing technology** to detect when a student enters the classroom area. Geo-fencing works by creating a virtual boundary around a specific location. When a student's mobile device enters that boundary, the system automatically records attendance.

However, during the middle of development, the Dean of the university introduces a new requirement: **Biometric Face ID verification** must be integrated into the system to prevent proxy attendance. Proxy attendance occurs when students mark attendance for others who are not physically present.

This unexpected requirement creates a major challenge for the development team. The situation highlights the difference between the traditional **Waterfall model** and the **Agile Scrum framework**. This assignment analyzes why the Waterfall model would fail in this situation and how Agile Scrum can successfully handle the change.

## Project Overview

The project aims to develop a smart attendance system that automates the process of recording student presence in classrooms. Instead of manually marking attendance, the system uses location technology to detect student entry.

### Original Features
- Student login system
- Geo-fencing based attendance marking
- Classroom boundary detection
- Attendance dashboard for teachers

### New Requirement Added Mid-Development
- Biometric Face ID verification
- Camera integration
- Identity verification to prevent proxy attendance

The addition of biometric authentication changes the project scope significantly. The development model used by the team will determine whether the project succeeds or fails.

## Task 1: Waterfall Model Failure Analysis

### Overview
The Waterfall model is a traditional software development model that follows a linear and sequential approach. Each phase must be completed before the next phase begins.

### Typical Waterfall Phases
1. Requirement Analysis
2. System Design
3. Development
4. Testing
5. Deployment
6. Maintenance

Although the Waterfall model works well when requirements are stable, it becomes problematic when changes occur during development.

### Reason 1: Rigidity of the Waterfall Model

The Waterfall model is extremely rigid. Once the project moves past the requirement phase, it is very difficult to go back and modify requirements.

In this project, the original requirement only included geo-fencing attendance. The system architecture, database design, and application workflow would have been planned based on this requirement.

When the Dean introduces the biometric verification requirement, the development team would need to:
- Redesign the authentication process
- Integrate camera functionality
- Update security architecture
- Modify the user interface
- Update the database to store biometric data

However, since the project is already in the development phase, returning to the requirement and design phase disrupts the entire workflow. This rigidity makes Waterfall unsuitable for projects with changing requirements.

### Reason 2: High Cost of Change

One of the biggest disadvantages of the Waterfall model is the high cost of change. Because all design decisions are made at the beginning of the project, any change introduced later requires major modifications.

Adding biometric Face ID verification affects multiple parts of the system including:
- Application architecture
- Mobile camera permissions
- Biometric recognition libraries
- Backend authentication services
- Database structure

The development team may need to rewrite large portions of the codebase. This increases development costs and delays the project timeline.

**Example:** If the original development cycle was planned for 6 months, integrating biometric verification halfway through could add 2–3 additional months of work.

### Reason 3: Delayed Testing Problems

In the Waterfall model, testing occurs only after the development phase is completed. This means that integration problems may not be discovered until the very end of the project.

If biometric authentication is added late in the development cycle, several issues may appear:
- Face recognition API compatibility problems
- Camera permission errors
- Geo-fencing and biometric modules conflicting with each other
- Performance issues on student devices

Since testing happens late, fixing these issues becomes extremely expensive and time-consuming. This delay increases the risk of project failure.

## Task 2: Agile Pivot Using Scrum Framework

To overcome the limitations of the Waterfall model, the development team decides to switch to the **Agile Scrum framework**.

Agile is an iterative development methodology that focuses on flexibility, collaboration, and continuous improvement.

### Key Characteristics of Agile
- Short development cycles called **Sprints**
- Continuous feedback from stakeholders
- Frequent testing and improvements
- Ability to adapt to changing requirements

### Scrum Roles

**Product Owner:**
Responsible for defining product features and maintaining the product backlog.

**Scrum Master:**
Ensures the Scrum process is followed and removes obstacles for the development team.

**Development Team:**
Designs, develops, and tests the application features during each sprint.

### Sprint Planning for the Attendance App

To manage the new requirement efficiently, the project is divided into two 2-week sprints. Each sprint focuses on delivering working features and gathering feedback before moving to the next sprint.

#### Sprint 1: Minimum Viable Product (MVP)

The goal of Sprint 1 is to build the **Minimum Viable Product (MVP)**. An MVP is the simplest version of the product that still delivers core functionality.

**Sprint 1 Features:**
- Basic user interface
- Student login system
- Geo-fencing attendance detection
- Basic attendance dashboard

At the end of Sprint 1, students can log in and automatically mark attendance when they enter the classroom geo-fence.

#### Sprint 2: Feature Update (Biometric Integration)

During Sprint 2, the development team integrates the new biometric requirement.

**Sprint 2 Features:**
- Face ID biometric authentication
- Camera integration
- Identity verification before marking attendance
- Improved dashboard with attendance analytics
- Bug fixes and performance improvements

By implementing the biometric feature in a separate sprint, the team avoids disrupting the entire system architecture.

## Task 3: Sprint Backlog Example (Sprint 1)

A **Sprint Backlog** is a list of tasks that the development team commits to completing during a sprint.

### Example Tasks for Sprint 1

1. Design login page interface
2. Implement user authentication system
3. Create database for students and attendance records
4. Research Geo-fencing APIs
5. Implement Geo-fencing logic
6. Develop automatic attendance marking module
7. Create attendance dashboard
8. Perform unit testing
9. Fix bugs and integrate modules

These tasks are managed in a project management tool or spreadsheet where team members track progress.

## Benefits of Using Agile in This Project

Switching to Agile provides several advantages:

### Flexibility
New requirements such as biometric authentication can be added easily.

### Faster Delivery
Working features are delivered at the end of each sprint.

### Continuous Feedback
Teachers and administrators can test the system early and suggest improvements.

### Reduced Risk
Problems are detected early through continuous testing.

These benefits make Agile an ideal development model for modern software projects.

## Conclusion

This case study demonstrates the limitations of the Waterfall model when project requirements change during development. The rigid structure, high cost of change, and delayed testing make Waterfall unsuitable for dynamic projects.

In contrast, the **Agile Scrum framework** allows teams to adapt quickly to new requirements through short development cycles called sprints. By implementing the Minimum Viable Product in Sprint 1 and adding biometric Face ID verification in Sprint 2, the development team can deliver a flexible, scalable, and reliable attendance system.

Therefore, **Agile provides a more effective approach for modern software development projects** where requirements frequently evolve.
