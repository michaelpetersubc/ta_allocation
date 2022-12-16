# Introduction

This ta allocation algorithm is designed to match tas to courses in environments with self contained sessions - i.e., terms or quarters, or whatever your university uses to determine the length of all courses.  

The approach it uses is to turn the many to one allocation problem of putting tas in courses to a one to one allocation problem of assigning each ta to a task. The concept of an `allocation block` will be used repeatedly.  A `student allocation block` is meant to represent a ta - the worker.  A `course allocation block` is meant to represent a task.

A student allocation block contains very little information, just the name of the student, the degree program they are in, either MA or PHd and a random score. A course allocation block can contain a lot of information.  Basic setup is just to create all these blocks.

In most applications, you should create roughly the same number of course allocation blocks and student allocation blocks. One thing to be aware of is that the algorithm won't always match every ta with a course (nor every course with a ta).  The algorithm uses a Ph'd student first approach.  Ph'ds are allocated according to their preferences, even if they would like to ta for courses that only require Masters students as tas.  If Ph'd students rank Masters level courses highly, they may be allocated to them, and push out some Masters students who might otherwise have been allocated as tas.  If this happens some Ph'd level courses will not be allocated tas. 

The instructions below will explain how to create an allocation that is shared across two different courses.  You probably also have some mission critical assignments - for example, you may need a particular student to act as a teaching assistant for the core micro theory course no matter what their preferences are.  These tricks are all explained in the section on `Running the algorithm` below.

Hopefully this outline will get you to the section where you need help quickly.

## Configuration

## Sessions

1. [Create a Session](sessions/create_session.md)
1. [Create a course](course_create.md)
    