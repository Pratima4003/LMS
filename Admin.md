# 🛡️ **Admin Role – Deep Dive & Specifications**

This document details the **Admin** role for the Learning Management System (LMS). The Admin is the "Superuser" responsible for **Governance, Quality Control, and Platform Health**.

---

## **1. Core Responsibilities**

The Admin does not participate in learning or teaching. Their focus is entirely on managing the ecosystem.

### **A. Gatekeeper for Instructors**
*   **Vetting Process**: Instructors cannot simply sign up and start publishing. They must be approved.
*   **Action**: Admin reviews instructor applications (profile, credentials) and grants `INSTRUCTOR` status.

### **B. Quality Control (Course Moderation)**
*   **Publication Approval**: Courses created by instructors may default to a `PENDING_APPROVAL` state.
*   **Takedown Power**: Admin has the absolute authority to **Disable** or **Archive** any course that violates platform policies (e.g., copyright infringement, inappropriate content).
*   **Category Management**: Admin defines the taxonomy (e.g., "Web Development", "Data Science") to keep the platform organized.

### **C. User Management**
*   **Oversight**: View a master list of all users (Students and Instructors).
*   **Enforcement**: Ability to **Ban** or **Suspend** users who violate terms of service.

### **D. Platform Health & Analytics**
*   **Command Center**: The Admin Dashboard provides high-level insights, not personal progress.
    *   **Financials**: Total Revenue, Instructor Payouts.
    *   **Growth**: Daily/Weekly Signups, Active Users.
    *   **Engagement**: Most popular courses, completion rates.

---

## **2. Functional Requirements (Workflows)**

### **2.1 User Management**
*   **List Users**: Paginated view of all users with filters (Role, Status).
*   **Verify Instructor**: Button to approve a pending instructor request.
*   **Ban User**: Toggle user status to `BANNED`.

### **2.2 Course Management**
*   **List Courses**: View all courses across the platform.
*   **Approve Course**: Transition course status from `PENDING` to `PUBLISHED`.
*   **Reject/Disable Course**: Transition course status to `DRAFT` or `ARCHIVED` with a reason.

### **2.3 Category Management**
*   **CRUD Categories**: Create, Read, Update, Delete course categories.

### **2.4 Dashboard**
*   **Stats Cards**: Total Users, Total Courses, Total Revenue.
*   **Charts**: User Growth (Line Chart), Revenue (Bar Chart).

---

## **3. Technical Implications**

### **3.1 Middleware & Security**
*   **`verifyAdmin` Middleware**: A strict Express middleware that checks `req.user.role === 'ADMIN'`.
*   **Route Protection**: All `/api/admin/*` routes must be protected by this middleware.

### **3.2 Database Schema Updates**
*   **User Model**:
    *   `role`: Enum `['STUDENT', 'INSTRUCTOR', 'ADMIN']`
    *   `isVerified`: Boolean (specifically for Instructors)
    *   `status`: Enum `['ACTIVE', 'BANNED']`
*   **Course Model**:
    *   `status`: Enum `['DRAFT', 'PENDING_APPROVAL', 'PUBLISHED', 'ARCHIVED']`

### **3.3 Frontend Architecture**
*   **Layout**: A dedicated `AdminLayout` (sidebar with Admin-specific links) separate from the main app layout.
*   **Components**:
    *   **Data Tables**: High-density tables with sorting/filtering (e.g., using TanStack Table).
    *   **Charts**: Visualizations for the dashboard (e.g., using Recharts).

---

## **4. Future Enhancements**
*   **Audit Logs**: Track every action taken by an admin (e.g., "Admin X banned User Y").
*   **Broadcast System**: Send platform-wide notifications to all users.
