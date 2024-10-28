# Zen Class Program Management System

This project is a MongoDB-based application designed to streamline data management for the Zen Class program, encompassing user information, attendance tracking, task submissions, placement drives, and mentor-student relationships. The system provides a centralized database that facilitates efficient data storage and retrieval, with capabilities for managing code kata progress, tracking placement opportunities, and managing mentor-mentee dynamics.

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Usage Instructions](#usage-instructions)
- [Database Schema](#database-schema)
- [Sample Queries](#sample-queries)

---

## Project Overview

The **Zen Class Program Management System** provides a structured database to track critical aspects of student participation and progress within the Zen Class program. The database stores records on attendance, task completion, company placement drives, and mentor-mentee relationships. The system is designed to offer easy querying and data management using MongoDB, making it straightforward to generate reports or monitor student progress.

---

## Features

- **User Management:** Maintain records of students' details, including their batch, contact information, and other identifiers.
- **Attendance Tracking:** Log daily attendance for each student, enabling the generation of attendance reports.
- **Task Submission Tracking:** Record and update each student's task completion status to monitor engagement and learning progress.
- **Code Kata Progress Monitoring:** Track students' progress in coding exercises, facilitating targeted mentorship and learning plans.
- **Company Drive Management:** Log and manage records of company recruitment drives, including dates and associated companies.
- **Mentor-Student Relationships:** Define and track mentor-mentee pairings to ensure each student has appropriate guidance.

---

## Technology Stack

- **Database:** MongoDB, with MongoDB Compass for graphical management.

---

## Usage Instructions

1. **Setting Up the Database:**
   - Open MongoDB Compass and connect to your MongoDB instance.
   - Create the database and collections by importing the provided JSON files or creating collections manually.

2. **Importing Data:**
   - Import the following collections into your MongoDB database:
     - `users` — Student data
     - `attendance` — Attendance records
     - `codekata` — Code kata progress
     - `tasks` — Task submission details
     - `company_drives` — Placement drive information
     - `mentors` — Mentor data and mentee lists

3. **Running Queries:**
   - Run various queries in MongoDB Compass or Mongo Shell to retrieve, update, or delete data as needed. See the [Sample Queries](#sample-queries) section for examples.

---

## Database Schema

The MongoDB database is organized into several collections with defined fields for easy management and querying:

1. **Users**
   - `user_id`: Unique identifier for the user (ObjectId)
   - `name`: Full name of the student (String)
   - `email`: Student’s email (String, unique)
   - `batch`: Batch to which the student belongs (String)
   - `attendance`: Array of attendance records with date and status fields
   - `tasks_submitted`: Array detailing task submissions with status and submission date

2. **Company Drives**
   - `drive_id`: Unique identifier for each drive (ObjectId)
   - `company_name`: Name of the recruiting company (String)
   - `date`: Date of the drive (ISODate)

3. **Mentors**
   - `mentor_id`: Unique identifier for each mentor (ObjectId)
   - `mentor_name`: Name of the mentor (String)
   - `mentees`: Array of User IDs representing the students mentored by this mentor

4. **Code Kata Progress**
   - `user_id`: Unique identifier linking to the user (ObjectId)
   - `completed_levels`: Array representing the levels completed by the student

5. **Tasks**
   - `task_id`: Unique identifier for each task (ObjectId)
   - `title`: Title of the task (String)
   - `description`: Brief description of the task (String)
   - `due_date`: Task submission deadline (ISODate)
   - `status`: Submission status (e.g., "submitted", "pending")

---

## Sample Queries

Here are some examples of common queries you may run in the Zen Class Program Management System:

1. **Retrieve all students with attendance in the last 30 days:**
   ```javascript
   db.attendance.find({
       date: { $gte: new Date(new Date().setDate(new Date().getDate() - 30)) }
   });
   ```

2. **Find all tasks submitted by a specific user:**
   ```javascript
   db.tasks.find({
       "user_id": ObjectId("USER_ID_HERE"),
       "status": "submitted"
   });
   ```

3. **List all mentors along with their mentees:**
   ```javascript
   db.mentors.aggregate([
       { $lookup: { from: "users", localField: "mentees", foreignField: "_id", as: "mentee_details" } }
   ]);
   ```

4. **Retrieve all company drives for a specified month:**
   ```javascript
   db.company_drives.find({
       date: {
           $gte: ISODate("2023-08-01"),
           $lte: ISODate("2023-08-31")
       }
   });
   ```

5. **Count the total code kata levels completed by each student:**
   ```javascript
   db.codekata.aggregate([
       { $group: { _id: "$user_id", total_completed: { $sum: 1 } } }
   ]);
   ```

---

## Additional Notes

- **Data Consistency:** Ensure all entries in `mentors.mentees` reference valid user IDs from the `users` collection.
- **Optimized Indexing:** For faster querying, create indexes on frequently queried fields such as `user_id`, `date`, and `status`.
- **Data Validation:** Use MongoDB schema validation to enforce data integrity, such as unique constraints on fields like `email`.

---