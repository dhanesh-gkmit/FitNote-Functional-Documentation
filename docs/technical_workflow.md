<h1>System Architecture</h1>



## Overview
This system follows a modern, cloud-based full-stack architecture built for scalability, flexibility, and security.
Each part of the application — the frontend interface, backend services, and database — works independently but connects seamlessly through secure REST APIs and automated CI/CD pipelines.<br>

- **Frontend**: React.js (v19.2.0) <br>
- **Backend**: Node.js (v20 LTS) + Express (v4.19) with **JWT-based authentication** <br>
- **Email Service**: Nodemailer (v6.9) for transactional and notification emails <br>
- **Cloud Hosting**: AWS EC2 instances for both frontend and backend deployment <br>
- **Database**: MongoDB Atlas (v7.0) – fully managed NoSQL database <br>
- **Repositories**: Separate GitHub repositories for frontend and backend codebases <br>
- **CI/CD Pipeline**: Automated builds and deployments via **GitHub Actions** <br>

![systemarchitecture](images/System_architecture.png)
---

## Development Workflow

<div style="display: flex; gap: 20px; justify-content: space-between; flex-wrap: wrap;">

<div style="flex: 1; min-width: 320px; background: var(--md-default-bg-color); border: 1px solid #e0e0e0; border-radius: 10px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.05);">
 
<h3><b>Frontend Repository</b></h3>
<ul>
  <li>Developed using <b>React.js</b></li>
  <li>Code changes are pushed to <b>GitHub</b></li>
  <li>Each commit triggers an automated <b>GitHub Actions</b> workflow</li>
  <li>GitHub Actions builds and deploys the frontend to the <b>AWS EC2</b> instance</li>
</ul>

</div>

<div style="flex: 1; min-width: 320px; background: var(--md-default-bg-color); border: 1px solid #e0e0e0; border-radius: 10px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.05);">

<h3><b>Backend Repository</b></h3>
<ul>
  <li>Developed using <b>Node.js + Express</b></li>
  <li>Implements <b>JWT-based authentication</b> for secure access</li>
  <li>Code pushed to <b>GitHub</b> triggers GitHub Actions</li>
  <li>GitHub Actions automatically builds, tests, and deploys the backend to <b>AWS EC2</b></li>
</ul>

</div>

</div>

---

## Deployment Architecture

<div style="display: flex; gap: 20px; justify-content: space-between; flex-wrap: wrap;">

<div style="flex: 1; min-width: 300px; background: var(--md-default-bg-color); border: 1px solid #e0e0e0; border-radius: 10px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.05);">

<h3><b>Frontend EC2 (React.js)</b></h3>
<ul>
  <li>Serves the production build (HTML, CSS, JS) built with React.js</li>
  <li>Receives HTTP requests from user browsers</li>
  <li>Makes API calls to the Backend EC2 instance</li>
  <li>Receives API responses from the backend</li>
  <li>Renders the final UI and sends HTTP responses</li>
  <li>Update automatically via <b>GitHub Actions</b> CI/CD pipeline</li>
</ul>

</div>

<div style="flex: 1; min-width: 300px; background: var(--md-default-bg-color); border: 1px solid #e0e0e0; border-radius: 10px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.05);">

<h3><b>Backend EC2 (Node + Express)</b></h3>
<ul>
  <li>Handles all API endpoints and routes</li>
  <li>Verifies <b>JWT tokens</b> for authentication</li>
  <li>Implements <b>Role-Based Protected Routes</b> — only authorized users can access restricted APIs</li>
  <li>Processes API requests and interacts with MongoDB Atlas</li>
  <li>Update and managed via <b>GitHub Actions</b> CI/CD</li>
</ul>

</div>

<div style="flex: 1; min-width: 300px; background: var(--md-default-bg-color); border: 1px solid #e0e0e0; border-radius: 10px; padding: 18px; box-shadow: 0 2px 8px rgba(0,0,0,0.05);">

<h3><b>MongoDB Atlas</b></h3>
<ul>
  <li>Cloud-hosted NoSQL database</li>
  <li>Stores all user, workout, and authentication data</li>
  <li>Accessible only to the backend through secure connection strings</li>
  <li>Ensures data consistency and redundancy</li>
</ul>

</div>

</div>

---

## User Interaction Flow
1. User opens the application in a browser  
2. Browser sends HTTP request → Frontend EC2  
3. The browser loads and displays the React app.
4. React sends API requests → Backend EC2  

---
## System Communication Flow

<div style="display: flex; gap: 20px; justify-content: space-between; flex-wrap: wrap;">

<div style="flex: 1; min-width: 300px; border: 1px solid #e0e0e0; border-radius: 10px; padding: 16px;">

<h3><b>Frontend → Backend Communication</b></h3>
<ul>
  <li>React sends API requests to the backend</li>
  <li>Backend processes and returns <b>JSON responses</b></li>
  <li>React updates the UI based on the response</li>
</ul>

</div>

<div style="flex: 1; min-width: 300px; border: 1px solid #e0e0e0; border-radius: 10px; padding: 16px;">

<h3><b>Backend → Database Communication</b></h3>
<ul>
  <li>Backend API sends queries to <b>MongoDB Atlas</b></li>
  <li>Atlas returns data to the backend</li>
  <li>Backend processes data and sends it to the frontend</li>
</ul>

</div>

</div>

---

## System Workflow Summary
- Developer commits code → GitHub  
- GitHub Action trigger and deploy code to EC2  
- User → Browser → Frontend EC2  
- Frontend → Backend EC2 (API)  
- Backend → MongoDB Atlas  
- Backend → Frontend response  
- UI Appers
---

## Authentication (JWT Flow)

```mermaid
flowchart TD
    %% ---------------- LOGIN PROCESS ----------------
    A0["User Submits Login Credentials"] --> B0["Backend Receives Credentials"]

    B0 --> C0{"Are Credentials Valid?"}

    C0 -- "NO" --> U0["401 Unauthorized
    Invalid Username/Password"]

    C0 -- "YES" --> D0["Backend Generates Access + Refresh Tokens"]

    D0 --> E0["Frontend Stores Access Token (localStorage)
    and Refresh Token (HttpOnly Cookie)"]

    E0 --> F0["Frontend Sends Access Token
    Authorization: Bearer <access_token>"]

    F0 --> G0["Backend Verifies Access Token
    (Signature, Expiry, Issuer)"]

    G0 -- "Valid" --> H0["Backend Allows Access → Protected Route"]
    G0 -- "Expired" --> I0["401 Access Token Expired"]

    %% ---------------- TOKEN REFRESH FLOW ----------------
    I0 --> J0["Frontend Detects 401 + 'Access token expired'
    Calls /auth/refresh with Refresh Token"]

    J0 --> K0["Backend Verifies Refresh Token"]

    K0 -- "Invalid / Missing" --> U1["403 Forbidden
    Invalid or Missing Refresh Token → Please Login Again"]

    K0 -- "Valid" --> L0["Backend Generates New Access Token"]

    L0 --> M0["Backend Sends New Access Token to Frontend"]

    M0 --> N0["Frontend Updates Token in localStorage
    and Retries Original Request"]

    %% ---------------- STYLING ----------------
    classDef frontend fill:#dbeafe,stroke:#3b82f6,stroke-width:1px;
    classDef backend fill:#dcfce7,stroke:#16a34a,stroke-width:1px;
    classDef error fill:#fee2e2,stroke:#ef4444,stroke-width:1px;

    class A0,E0,F0,I0,J0,M0,N0 frontend;
    class B0,C0,D0,G0,K0,L0,H0 backend;
    class U0,U1 error;
```

---

## Authorization (Role-Based Access Control)

```mermaid
flowchart TD

    %% ---------------- FRONTEND CHECK ----------------
    A0["Client (User) Tries to Access Protected Route 
    e.g., /dashboard or /workoutplans"] --> B0{"Is Access Token Present in localStorage?"}

    B0 -- "NO" --> U0["Frontend Redirects to /login
    Message: 'Unauthorized - Please Login'"]

    B0 -- "YES" --> A["User API Request 
    e.g., /getWorkoutPlan (Bearer access token)"]

    %% ---------------- BACKEND AUTH FLOW ----------------
    A --> B{"Is Authorization Header Present?"}

    B -- "NO" --> U1["401 Unauthorized
    Missing Authorization Header"]

    B -- "YES" --> C["Verify JWT Access Token
    (Signature, Expiry, Issuer)"]

    C -- "Invalid Token" --> U1
    C -- "Expired Token" --> X["401 Access Token Expired → 
    Frontend Triggers Refresh Token Flow"]

    C -- "Valid Token" --> D["Extract userId, role, scopes 
    Attach to req.user"]

    D --> E{"Is the Route Protected?"}

    E -- "NO" --> G["Controller Executes
    Request Allowed"]

    E -- "YES" --> F{"Is User Role Authorized 
    to Access this Route?"}

    F -- "NO" --> U2["403 Forbidden 
    User Lacks Permission"]

    F -- "YES" --> G["Controller Executes 
    Success Response"]

    %% ---------------- TOKEN REFRESH FLOW ----------------
    X --> J["Frontend: POST /auth/refresh (sends refresh token cookie)"]

    J --> K["Backend: Verify Refresh Token"]

    K -- "Valid" --> L["Backend: Generate new Access Token"]
    K -- "Invalid / Missing" --> U3["401 Unauthorized
    Refresh invalid → Frontend redirects to /login"]

    L --> M["Backend: Return new Access Token"]

    M --> N["Frontend: Update access token and retry original request"]

%% ------------------ STYLING ------------------
classDef frontend fill:#dbeafe,stroke:#3b82f6,stroke-width:1px;
classDef backend fill:#dcfce7,stroke:#16a34a,stroke-width:1px;
classDef error fill:#fee2e2,stroke:#ef4444,stroke-width:1px;

class A0,B0,U0,X,J,M,N frontend;
class A,B,C,D,E,F,G,K,L backend;
class U1,U2,U3,U0 error;
```

---

# Data Flow Diagrams (DFD)

Below are the complete DFDs for the system (Level 0 and Level 1).  

---

## DFD – Level 0 (Context Diagram)

```mermaid
flowchart LR
    %% --- External Entities & Processes ---
    User["User (Client)"]
    Frontend[" React (Frontend EC2)"]
    Backend[" Node/Express API (Backend EC2)"]
    DB[" MongoDB Atlas"]

    %% --- Data Flows ---
    User -->|"HTTP / HTTPS Requests"| Frontend
    Frontend -->|"API Requests"| Backend
    Backend -->|"DB Queries"| DB
    DB -->|"Query Results"| Backend
    Backend -->|"JSON Responses"| Frontend
    Frontend -->|"Rendered UI"| User
```

---

## DFD – Level 1 (Authentication + Protected Endpoints)

![dfd1](images/Dfd_l1.png){ width=1300 }

---


