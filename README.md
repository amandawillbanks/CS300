Overview
This is a simple C++ program that acts as a Course Planning Tool. It allows users to:

Load course information from a file

Display a sorted list of courses

View course details including prerequisites

Search for specific courses by course ID

The program uses a Binary Search Tree (BST) to store and organize the course data, providing fast lookup and sorted traversal.

Features
Load Course Data: Import course information from a .txt or .csv file.

View All Courses: Display a sorted list of all available courses.

View Course Details: Search for a course by ID and view its title and prerequisites.

Error Handling: Alerts if course data isn't loaded yet or if an invalid option is selected.

Technologies Used
C++ (Standard Library)

Binary Search Tree (BST) for data storage

Command-line interface

How to Run
Compile the program using a C++ compiler (e.g., g++ ProjectTwo.cpp -o CoursePlanner).

Run the executable (e.g., ./CoursePlanner on Mac/Linux or CoursePlanner.exe on Windows).

Follow the on-screen menu options:

1 to load course data

2 to print all courses alphabetically

3 to search for a course and view prerequisites

9 to exit the program

Note: When loading course data, ensure the input file is correctly formatted with each line representing a course:
CourseID,Course Name,Prerequisite1,Prerequisite2,...

Example:

css
Copy
Edit
CS101,Introduction to Programming
CS102,Data Structures,CS101
CS201,Algorithms,CS102
Project Structure
Course: A structure representing a course with an ID, name, and list of prerequisites.

Node: A node in the binary search tree containing a Course.

CourseBST: A class that manages course insertion, in-order traversal, and search operations.

Why a Binary Search Tree (BST)?
The BST automatically maintains an ordered structure, making it easy to print courses alphabetically and efficiently search for a specific course. Its balance of insert and search operations (O(log n) average) makes it ideal for this application.

Author
Developed for CS-300: Data Structures and Algorithms

Part of Project Two Assignment
