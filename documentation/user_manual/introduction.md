# Introduction

This ta allocation algorithm is designed to match tas to courses in environments with self contained sessions - i.e., terms or quarters, or whatever your university uses to determine the length of all courses.  

The approach it uses is to turn the many to one allocation problem of putting tas in courses to a one to one allocation problem of assigning each ta to a task. The concept of an `allocation block` will be used repeatedly.  A `student allocation block` is meant to represent a ta - the worker.  A `course allocation block` is meant to represent a task.

A student allocation block contains very little information, just the name of the student, the degree program they are in, either MA or PHd and a random score. A course allocation block can contain a lot of information.  Basic setup is just to create all these blocks.

In most applications, you should create the same number of course allocation blocks and student allocation blocks since you probably have commitments to both your students and courses.  The instructions below will explain how to create shared teaching assistants.  You probably also have some mission critical assignments - for example, you may need a particular student to act as a teaching assistant for the core micro theory course no matter what their preferences are.  These tricks are all explained in the section on `Running the algorithm` below.

Hopefully this outline will get you to the section where you need help quickly.

## Courses

1. [Create a Session](session_create.md)
1. [Create a course](course_create.md)
    ![alt](session.png "Session")