## App Workflow — Diagrammatic Overview

![System Flow](images/App_workflow.png){ width=1300 }
---

## Some Possible Edge Cases

### 1. Duplicate Client Credentials
- **Issue:** Mentor tries to create a new client with an already existing username or password.  
- **Solution:** Add backend validation to check for existing username and password before saving a new client.

### 2. Multiple Report Submissions for Same Day
- **Issue:** Client edits a previous report and resends it, generating multiple unnecessary emails to the mentor.  
- **Solution:** Allow only one report submission for assigned schedule. Disable editing after saving reports in database.

### 3. Missing Required Fields
- **Issue:** Client tries to save a report without filling required details (weight, reps, etc.).  
- **Solution:** Enforce frontend + backend validation; mark critical fields as `required` and return clear error messages.

### 4. Empty Report Submission 
- **Issue:** Client attempts to send an empty report to the mentor.  
- **Solution:** The workflow is designed so that the report must be saved in the database before it can be mailed to the mentor. Database-level validations ensure that no empty fields are allowed while saving the report.

### 5. If Client Forgot to Send the Report

- **Issue:** The client forgot to send the report of the previous day and wants to send it to the mentor now.  
- **Solution:** If the client has saved the report of that schedule in the database but forgot to send it, then they ca go to the **Previous Workout** section. From there, they can send the report of that schedule to the mentor.



---

# Tech Stack

The project is built using a simple and powerful tech stack suitable for real deployment.

## MERN Stack
- **MongoDB Atlas** – Database for storing users, workouts, Diet.
- **Express.js** – Backend API framework.
- **React.js** – Frontend UI framework.
- **Node.js** – Server runtime.

## Version Control
- **Git** – Version control for managing changes.
- **GitHub** – Remote code repository for collaboration and backups.
- **GitHub Actions** - For CI/CD workflow

## Cloud Hosting
- **AWS EC2** – Cloud server used to deploy and run the production environment.

---

## Expected Outcome
- Trainers can manage all clients digitally from one dashboard.  
- Client follow structured routines with historical records.  
- Exercise progress and personal records are tracked visually.  
- Gym management becomes efficient, data-driven, and paper-free.

---

