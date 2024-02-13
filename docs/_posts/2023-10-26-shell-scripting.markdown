---
layout: post
title:  "Learning Unix Shell Scripting Like It's The 90s"
date:   2023-10-26 18:00:00 -0400
categories: Tech Rambles
---

About a month ago, I had the pleasure of coming across a book entitled _Unix Shell Programming_. On the surface, there is nothing remarkable about the book. But when I looked past the front cover, I was delighted to learn that the edition I was holding was published in 1994. At that moment, the book became an exciting peek into the world of software development in the 90s.


# Quirks Of Early Unix Systems
Although I'm no expert when it comes to shell scripting, I do have a baseline level of knowledge that I have acquired during my time developing in a modern Linux environment. I was surprised to learn just how forward-compatible most of the concepts were. Although there were some quirks (such as a discussion on `ed`, the original text editor program), many of the utilities that were discussed in the book were still in use today.

The basic commands such as `grep`, `cd`, `cat`, `head`, and many, many more are still used today. I think it's unlikely these commands are going to go away any time soon.


# Relational Database File System
The first third or so of the book discusses the basics of these commands as well as common applications. In this section, I'd like to focus on a peculiar aspect of the second part of the book - a discussion on relational database design. Because of my extensive work with databases, this section interested me in particular.

Here, the authors discuss an implementation of a relational database system written entirely with shell programs. Although the implementation is not very performant by modern standards, it is still an interesting discussion in design. The book uses an example of an employee payroll system, and gives an image describing the design of the application.

<figure>
  <img src="/blog/images/unix-relational-database.jpg" alt="An example relational database."/>
</figure>

The table storage is accomplished by using a simple tab-deliniated text file, with the first field being the index or key that uniquely identifies the data item contained in that row.

The system shown above is designed to wrok with just three tables:
 - Employee
 - Time Worked
 - Tax Tables

The Employee and Time worked tables would have a key common to both, which could be _joined_ on. For example, if we had an employee ID field, then we could design the following Employee table

| Employee ID | Last   | First  | Birthday   | Salary |
| ----------- | ------ | ------ | ---------- | ------ |
| 123456      | Lowell | Arthur | 1951/12/18 | 35000  |

Along with an accompanying Time Worked table 

| Employee ID | Day        | Time Worked |
| ----------- | ---------- | ----------- |
| 123456      | 1990/01/02 | 9.0         |
| 123456      | 1990/01/03 | 7.5         |
| 123456      | 1990/01/04 | 8.0         |

The rest of the chapter goes on to discuss how to build an interactive data entry and reporting application for this payroll system using just unix utilities. It is a great way to see all the potential ways the basic building blocks of the shell can be put to good use. It gives examples of uses for the following commands within this system:
 - `tput` for accepting user input.
 - `join` for merging records from multiple tables and displaying the output.
 - `grep` to select information relevant to a particular query.
 - `sed` and `awk` to update information belonging to just a single record.
 - `unique` to de-duplicate the output of queries.
 - `sort` to maintain a sorted order in the tables.
 - `pr` to print paginated reports of output.

In other words, the writers of the book manage to build _an entire CRUD application_ just with built-in unix shell utilies. It cannot be understated that the authors gave a complete specification of such a piece of software. That was when I began to grasp the power of the shell. No longer did it seem like a programming language only suited for quick one-liners dislpaying a bit of information. Rather, it is a powerful scripting language that could enable fast prototyping, and the best part is that it comes packed with a powerful suite of tools built to work well together.

By today's standards, such a system would be slow, primitive, and ugly. But that didn't matter - because the beauty of the shell is in the eye of the beholder.

<figure>
  <img src="/blog/images/fantastical-shell.jpg" alt="Fantastical Shell"/>
</figure>
