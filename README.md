# Academic Records Management System (ARMS)

---

## Team Members

| **Name**                  | **ID**   |
|---------------------------|----------|
| **Bora Musoni Herve**     | 24618    |
| **Hakizimana Ghislain**   | 25411    |
| **Rocky Kayitare**        | 24353    |
| **Habimana Daniel**       | 25111    |
| **Cyiza Kayitare Sabine** | 24801    |
| **Adolphe Uwayo**         | 25743    |
| **Gatete Bertrand**       | 25958    |
| **Niyonkuru Valens**      | 25097    |
| **Manzi Munyambanza Darius** | 25491 |
| **Hirwa Clement Rhodin**  | 25787    |
| **Karungi Rebecca**       | 26681    |

---

## Project Overview

The **Academic Records Management System (ARMS)** is designed to modernize and simplify the management of academic data within educational institutions. ARMS enhances **efficiency**, **accessibility**, and **data accuracy**, ensuring secure and reliable handling of academic records for all stakeholders, including students, instructors, and administrators.

---

## Problem Statement

### Challenges in Current Systems
- **Manual Management**: Labor-intensive processes prone to delays.
- **Error-Prone Systems**: High risk of data inaccuracies.
- **Limited Scalability**: Inability to meet modern academic needs.

### Proposed Solution
The **Academic Records Management System (ARMS)** offers:
- **Automation**: Streamlined processes for data handling.
- **Centralization**: Unified platform for all academic data.
- **Improved Accessibility**: Role-based access for students, instructors, and administrators.

---

## Project Objectives

1. **Centralized Data Storage**: A unified database to manage student information, courses, grades, and instructor data.
2. **Role-Based Accessibility**: Secure and transparent access for all stakeholders.
3. **Data Integrity**: Automated processes to maintain accurate and consistent records.
4. **Enhanced Analytics**: Tools for academic planning and performance tracking.

---

## Project Phases Summary

### **Phase 2: Business Process Modeling**
- **Focus**: Identify and map workflows like enrollment, grading, and instructor assignments.
- **Techniques**: 
  - **Swimlane Diagrams**: Define task responsibilities.
  - **BPMN Diagrams**: Visualize workflows.

**Deliverables**:
- `PHASE 2.docx`: Detailed business processes.
- `swimlanes.png`: Role-based task distribution.
- `bpmn.png`: Workflow visualization.

### **Phase 3: Logical Model Design**
- **Focus**: Design a logical database model with entities, relationships, and constraints.

**Deliverables**:
- `PHASE 3.docx`: Comprehensive database model.
- `FLOW_DIAGRAM.PNG`: Visualization of data structure.

---

## Repository Contents

### Documents
- `PHASE 2.docx`: Business process documentation.
- `PHASE 3.docx`: Logical database design.

### Diagrams
1. **Flow Diagram**  
   ![Flow Diagram](FLOW_DIAGRAM.PNG)  

2. **Swimlane Diagram**  
   ![Swimlanes Diagram](swimlanes.png)  

3. **BPMN Diagram**  
   ![BPMN Diagram](bpmn.png)  

### Database Setup
1. **Pluggable Database**  
   ![Pluggable Database](plugable.jpg)  

2. **Oracle Enterprise Manager**  
   ![OEM](OEM.jpg)  

---

## SQL Queries

### Create Tables
```sql
-- Create Students table
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    contact_info VARCHAR(255),
    enrollment_status VARCHAR(20) CHECK (enrollment_status IN ('Active', 'Inactive', 'Graduated')),
    enrollment_date DATE
);

-- Create Courses table
CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    credit_hours INT CHECK (credit_hours > 0),
    prerequisites VARCHAR(255)
);

-- Create Instructors table
CREATE TABLE Instructors (
    instructor_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(100)
);

-- Create Enrollments table
CREATE TABLE Enrollments (
    enrollment_id INT PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    enrollment_date DATE,
    term VARCHAR(20),
    FOREIGN KEY (student_id) REFERENCES Students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE,
    UNIQUE (student_id, course_id)
);

-- Create Grades table
CREATE TABLE Grades (
    grade_id INT PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    grade CHAR(2) CHECK (grade IN ('A', 'B', 'C', 'D', 'F', 'P')),
    term VARCHAR(20),
    comments TEXT,
    FOREIGN KEY (student_id) REFERENCES Students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE,
    UNIQUE (student_id, course_id, term)
);

### DELETE


-- Remove a student record
DELETE FROM Students
WHERE student_id = 5;

-- Remove a course record
DELETE FROM Courses
WHERE course_id = 5;
-- Delete an enrollment record
DELETE FROM Enrollments
WHERE enrollment_id = 3;
-- Delete a grade record
DELETE FROM Grades
WHERE grade_id = 4;
-- Remove a course-instructor relationship
DELETE FROM Courses_Instructors
WHERE course_instructor_id = 2;
-- Remove a department
DELETE FROM Department
WHERE department_id = 5;
```

### UPDATE QUERY

```
-- Update contact information for a student
UPDATE Students
SET contact_info = 'alice.johnson@example.com'
WHERE student_id = 1;

-- Change enrollment status of a student
UPDATE Students
SET enrollment_status = 'Inactive'
WHERE student_id = 5;
-- Update credit hours for a course
UPDATE Courses
SET credit_hours = 4
WHERE course_id = 4;

-- Change the title of a course
UPDATE Courses
SET title = 'Advanced Operating Systems'
WHERE course_id = 4;
-- Update the term for a specific enrollment
UPDATE Enrollments
SET term = 'Winter 2023'
WHERE enrollment_id = 4;

-- Change the course for an enrollment
UPDATE Enrollments
SET course_id = 3
WHERE enrollment_id = 5;
-- Update a student's grade
UPDATE Grades
SET grade = 'A+'
WHERE grade_id = 2;

-- Add a comment for a grade
UPDATE Grades
SET comments = 'Excellent'
WHERE grade_id = 1;

-- Change the instructor for a course
UPDATE Courses_Instructors
SET instructor_id = 5
WHERE course_instructor_id = 3;

-- Update course assignment for an instructor
UPDATE Courses_Instructors
SET course_id = 2
WHERE course_instructor_id = 4;

-- Update the department name
UPDATE Department
SET department_name = 'Modern Literature'
WHERE department_id = 5;

-- Correct a typo in the department name
UPDATE Department
SET department_name = 'Computer and Information Science'
WHERE department_id = 1;
```
### JOIN QUERY
```
SELECT 
    Students.name AS student_name,
    Courses.title AS course_title
FROM 
    Students
CROSS JOIN Courses;

SELECT 
    Students.name AS student_name,
    Courses.title AS course_title
FROM 
    Enrollments
INNER JOIN Students ON Enrollments.student_id = Students.student_id
INNER JOIN Courses ON Enrollments.course_id = Courses.course_id;

SELECT 
    Students.name AS student_name,
    Enrollments.course_id AS enrolled_course_id
FROM 
    Students
LEFT OUTER JOIN Enrollments ON Students.student_id = Enrollments.student_id;

SELECT 
    Courses.title AS course_title,
    Enrollments.student_id AS student_enrolled
FROM 
    Courses
RIGHT OUTER JOIN Enrollments ON Courses.course_id = Enrollments.course_id;
