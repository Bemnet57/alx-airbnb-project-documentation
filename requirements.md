1. User Authentication
Functional Requirements:
Users can sign up as students or admins.

Users can log in using email and password.

Role-based access control must restrict endpoints (e.g., only admins can approve applications).

JWT-based session management.

API Endpoints:
POST /api/auth/signup
Input:

json
Copy
Edit
{
  "fullName": "Jane Doe",
  "email": "jane@example.com",
  "password": "SecurePass123",
  "role": "student"
}
Validation Rules:

Email must be unique and valid.

Password: min 8 characters, includes letter and number.

Role must be student or admin.

Output (Success):

json
Copy
Edit
{
  "message": "User created successfully",
  "userId": "abc123"
}
POST /api/auth/login
Input:

json
Copy
Edit
{
  "email": "jane@example.com",
  "password": "SecurePass123"
}
Output (Success):

json
Copy
Edit
{
  "token": "JWT_TOKEN_STRING",
  "role": "student"
}
Output (Error):

json
Copy
Edit
{
  "error": "Invalid credentials"
}
Performance Criteria:
Authentication should respond within 300ms under normal load.

JWT should expire in 24 hours.

Rate limit login attempts: 5 per minute per IP.

🏢 2. Internship Management
Functional Requirements:
Admins can create, edit, and delete internship posts.

Students can view available internships and apply.

API Endpoints:
POST /api/internships
(Admin only)

Input:

json
Copy
Edit
{
  "title": "Software Intern",
  "company": "Tech Inc.",
  "description": "Build features using Flutter.",
  "requirements": ["Flutter", "Git"],
  "deadline": "2025-08-01"
}
Validation Rules:

Title and company: non-empty, 100 characters max.

Deadline must be a future date.

Output:

json
Copy
Edit
{
  "message": "Internship created successfully",
  "internshipId": "int123"
}
GET /api/internships
Output:

json
Copy
Edit
[
  {
    "id": "int123",
    "title": "Software Intern",
    "company": "Tech Inc.",
    "deadline": "2025-08-01"
  },
  ...
]
Performance Criteria:
Must support 100+ internships without noticeable delay (<500ms query).

Full-text search by title and company name should be available.

📄 3. Application Submission and Review System
Functional Requirements:
Students submit applications for specific internships.

Admins can view, filter, and update application statuses.

API Endpoints:
POST /api/applications
Input:

json
Copy
Edit
{
  "internshipId": "int123",
  "resumeUrl": "https://cdn.site.com/resumes/jane.pdf"
}
Validation:

Must be logged in as a student.

Resume URL must be valid and accessible.

One application per internship per student.

Output:

json
Copy
Edit
{
  "message": "Application submitted",
  "applicationId": "app456"
}
GET /api/applications
(Admin only)

Query Parameters: status=pending|approved|rejected, internshipId

Output:

json
Copy
Edit
[
  {
    "id": "app456",
    "studentName": "Jane Doe",
    "internshipTitle": "Software Intern",
    "status": "pending"
  }
]
PATCH /api/applications/:id/status
Input:

json
Copy
Edit
{
  "status": "approved"
}
Validation:

Only admins can approve/reject.

Status must be one of pending, approved, rejected.

Performance Criteria:
Review page loads <600ms with up to 200 applications.

Application status updates reflected in real-time (if using sockets or polling).
