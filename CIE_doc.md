 # Project Documentation: CIE Management System

## 1. Project Overview
The **CIE Management System** is a comprehensive web application designed to manage the daily operations of a **Center for Innovation and Entrepreneurship**. It serves as a central hub for students, faculty, and administrators to manage projects, inventory (lab & library), facility bookings, and academic activities.

**Key Goals:**
* Streamline **Project Registration** and approval workflows.
* Manage **Inventory** (Lab components and Library books) with issue/return tracking.
* Facilitate **Space Management** through location bookings.
* Provide role-based dashboards for **Students, Faculty, and Admins**.

---

## 2. Technical Architecture
The application is built using a modern full-stack web architecture:

* **Frontend & Backend Framework:** Next.js (React-based, handles both UI and API routes).
* **Database:** PostgreSQL (Relational database).
* **ORM (Object-Relational Mapping):** Prisma (Used for defining data models and querying the database).
* **Authentication:** Custom JWT/Session-based auth.
* **Styling:** Tailwind CSS.

### Core Data Models
The system is built around these primary entities (from `schema.prisma`):
1.  **Users:** Distinct roles including `STUDENT`, `FACULTY`, `ADMIN`, `COORDINATOR`.
2.  **Projects:** Tracks project status (`PENDING`, `APPROVED`, `REJECTED`), domains, and team members.
3.  **Inventory:**
    * `LabComponent`: Electronic components with stock tracking.
    * `LibraryItem`: Books and resources.
4.  **Locations:** Rooms/Labs available for booking.

---

## 3. User Roles & Functionalities

### **Student**
* **Project Management:** View available projects, form teams, and submit new project proposals.
* **Inventory Requests:** Request lab components (e.g., Arduino, Sensors) and library books.
* **Resource Booking:** Book locations (Study rooms/Labs) for specific time slots.
* **Academics:** View attendance records and class schedules.

### **Faculty**
* **Approvals:** Review and Approve/Reject/Shortlist student project applications.
* **Course Management:** Manage Course Units, Attendance, and assignments.
* **Oversight:** View Lab/Library requests related to their specific domain.

### **Admin**
* **System Oversight:** Full control over all users and data.
* **User Management:** Add/Remove Faculty and Students.
* **Master Data:** Manage Domains, Locations, and Inventory items.
* **Analytics:** View usage statistics and override approvals if necessary.

---

## 4. Operational Workflows

### A. Authentication Workflow
**Goal:** Securely log users in and redirect to the correct dashboard.
1.  **User Action:** Enters credentials (Email/SRN and Password).
2.  **System Logic:**
    * Verify credentials against `User` table.
    * If valid, generate session/token.
    * **Role Check:**
        * `ADMIN` -> `/admin/dashboard`
        * `FACULTY` -> `/faculty/dashboard`
        * `STUDENT` -> `/student/dashboard`
3.  **Error Handling:** Invalid credentials trigger an error message.

### B. Project Application Lifecycle
**Goal:** Enable students to submit projects for approval.
1.  **Submission (Student):** Fills Title, Description, Domain, and Team Members. System creates `Project` with status `PENDING`.
2.  **Review (Faculty):** Faculty views "Project Requests".
    * **Approve:** Status -> `APPROVED`. Project becomes active.
    * **Reject:** Status -> `REJECTED`.
    * **Shortlist:** Intermediate state for further interview/review.

### C. Lab Component Request Flow
**Goal:** Track inventory borrowing.
1.  **Request (Student):** Selects component and quantity.
    * *Validation:* `if (Available >= Requested)`.
2.  **Approval (Coordinator):** Staff reviews and marks as `APPROVED`.
3.  **Issue (Admin):** Student collects item. Status -> `ISSUED`. Stock is decremented.
4.  **Return:** Student returns item. Status -> `RETURNED`. Stock is incremented.

### D. Location Booking
**Goal:** Prevent double-booking.
1.  **Search:** User selects Room and Time.
2.  **Conflict Check:** System queries `LocationBooking` for overlaps.
3.  **Booking:** If no conflict, create booking record (Confirmed/Pending).

---

## 5. Visual Workflow Logic