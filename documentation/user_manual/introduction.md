# Introduction

This ta allocation algorithm is designed to match tas to courses in environments with self contained sessions - i.e., terms or quarters, or whatever your university uses to determine the length of all courses.  

The approach it uses is to turn the many to one allocation problem of putting tas in courses to a one to one allocation problem of assigning each ta to a task. The concept of an `allocation block` will be used repeatedly.  A `student allocation block` is meant to represent a ta - the worker.  A `course allocation block` is meant to represent a task.

A student allocation block contains very little information, just the name of the student, the degree program they are in, either MA or PHd and a random score. A course allocation block can contain a lot of information.  Basic setup is just to create all these blocks.

In most applications, you should create the same number of course allocation blocks and loc
