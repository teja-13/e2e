# Smart Library Management System Documentation

## Overview

`teja-13/e2e` is a full-stack Smart Library Management System. It provides role-based workflows for students, librarians, and administrators, including authentication, signup approvals, book catalog management, reservations, borrowing, returns, overdue fines, notifications, and analytics.

The project consists of a React/TypeScript/Vite frontend and an Express/MongoDB backend.

## Technology Stack

- **Frontend:** React 19, TypeScript, Vite 7
- **Backend:** Node.js, Express 5
- **Database:** MongoDB through Mongoose 8
- **Authentication:** JSON Web Tokens using `jsonwebtoken`
- **HTTP client:** Axios
- **Charts:** Recharts
- **Configuration:** dotenv
- **Cross-origin requests:** CORS
- **Deployment targets:** Vercel for the frontend and Render/MongoDB Atlas for the backend

## Repository Structure

```text
backend/
  mockAuthServer.js       Main Express application and REST API
  models/                 Mongoose database models
  API_END_POINTS.md       Backend endpoint reference
  SCHEMAS.md              Database schema reference
  users.json              Local fallback authentication seed
  .env.example            Backend configuration template
  package.json            Backend scripts and dependencies

frontend/
  src/
    main.tsx              React entry point
    App.tsx               Application route configuration
    components/            Shared layouts, analytics, and UI components
    pages/                 Public, student, librarian, and admin pages
    services/              API and authentication helpers
    assets/                Images and other frontend assets
  public/                  Static files
  index.html               Vite HTML entry point
  vite.config.ts           Vite configuration
  tsconfig*.json           TypeScript configuration
  eslint.config.js         ESLint configuration
  vercel.json              Vercel configuration
  .env.example             Frontend configuration template
  routes.txt               Human-readable route inventory

README.md                  Project overview and setup guide
DOCUMENTATION.md           Complete technical documentation
```

`backend/node_modules` is generated dependency content and should not normally be committed. Dependencies should be restored with `npm install`.

## System Architecture

The frontend is a single-page React application. `frontend/src/main.tsx` creates the React root, enables `StrictMode`, initializes `BrowserRouter`, and routes requests to either the shader debug page or `App.tsx`.

`frontend/src/App.tsx` defines public routes and nested role-specific route shells:

- `StudentLayout` for students
- `LibrarianLayout` for librarians
- `AdminLayout` for administrators

The pages communicate with the backend using Axios through `frontend/src/services/api.ts`. Protected requests use JWT bearer tokens.

The backend starts in `backend/mockAuthServer.js`. During startup it:

1. Loads environment variables.
2. Validates required configuration.
3. Connects to MongoDB.
4. Ensures default staff and student accounts exist.
5. Starts the overdue-notification scheduler.
6. Starts the Express server.

## User Roles

### Student

Students can:

- Log in.
- Submit a signup request.
- Search the catalog.
- Request available books.
- View reservations and borrowed books.
- View overdue fines.
- View and acknowledge notifications.
- View profile information.
- Change their password.

### Librarian

Librarians can:

- Log in or submit a signup request.
- View the librarian dashboard.
- Create books.
- View students.
- Reset student passwords.
- Delete students while returning their active books.
- Review and approve reservations.
- View active loans.
- Mark borrowed books as returned.

### Administrator

Administrators can:

- Log in or submit a signup request.
- View administrative dashboards and analytics.
- Manage students and librarians.
- Approve or reject registrations.
- Add staff accounts.
- Delete librarians.
- Reset student and librarian passwords.
- Manage books.
- Clear active borrowing data and pending reservations.

## Frontend Routes

### Public Routes

| Route | Page | Description |
|---|---|---|
| `/` | `StudentLogin` | Student login and application homepage |
| `/request-signup` | `StudentSignupRequest` | Student signup request form |
| `/librarian/login` | `LibrarianLogin` | Librarian login |
| `/librarian/request-signup` | `LibrarianSignupRequest` | Librarian signup request |
| `/admin/login` | `AdminLogin` | Administrator login |
| `/admin/signup` | `AdminSignup` | Administrator signup request |
| `/shader-debug` | `ShaderBackground` | Standalone shader background preview |

### Student Routes

| Route | Component | Description |
|---|---|---|
| `/student/dashboard` | `Dashboard` | Student landing page |
| `/student/books/search` | `BookSearch` | Search and request books |
| `/student/my-books` | `MyBooks` | View reservations and borrowing information |
| `/student/settings` | `Settings` | Student settings container |

The settings area includes:

- `Profile.tsx`
- `Fines.tsx`
- `Notifications.tsx`
- `ChangePassword.tsx`

### Librarian Routes

| Route | Component | Description |
|---|---|---|
| `/librarian/dashboard` | `LibrarianDashboard` | Librarian operational dashboard |
| `/librarian/books` | `ManageBooks` | Create and view books |
| `/librarian/students` | `ManageStudents` | Manage student accounts |
| `/librarian/reservations` | `Reservations` | Review and approve requests |
| `/librarian/borrowed` | `Borrowed` | View active loans and process returns |

### Administrator Routes

| Route | Component | Description |
|---|---|---|
| `/admin/dashboard` | `AdminDashboard` | Administrative dashboard |
| `/admin/users` | `ManageUsers` | Manage students, librarians, and registrations |
| `/admin/books` | `ManageBooks` | Administrative catalog management |
| `/admin/analytics` | `AdminAnalytics` | Analytics dashboard |

## Frontend Modules

### Application bootstrap

- `main.tsx` initializes React and routing.
- `App.tsx` defines the main application routes.
- `App.css` contains application-wide component styles.
- `index.css` contains global styles.

### Layout components

- `StudentLayout.tsx` provides student navigation and the student page shell.
- `LibrarianLayout.tsx` provides librarian navigation and the librarian page shell.
- `AdminLayout.tsx` provides administrator navigation and the admin page shell.

### Student pages

- `StudentLogin.tsx` authenticates students.
- `StudentSignupRequest.tsx` submits student registration information.
- `Dashboard.tsx` provides the student landing page.
- `BookSearch.tsx` loads books and submits book requests.
- `MyBooks.tsx` displays student reservations and borrowing information.
- `Settings.tsx` organizes student settings pages.
- `Profile.tsx` loads student profile data.
- `Fines.tsx` displays overdue items and calculated fines.
- `Notifications.tsx` lists notifications and marks them as read.
- `ChangePassword.tsx` changes the student password.

### Librarian pages

- `LibrarianLogin.tsx` authenticates librarians.
- `LibrarianSignupRequest.tsx` submits librarian registration requests.
- `LibrarianDashboard.tsx` displays librarian information and actions.
- `ManageBooks.tsx` displays and creates books.
- `ManageStudents.tsx` lists students, resets passwords, and removes student accounts.
- `Reservations.tsx` displays and approves reservations.
- `Borrowed.tsx` displays active loans and handles returns.

### Administrator pages

- `AdminLogin.tsx` authenticates administrators.
- `AdminSignup.tsx` submits administrator registration requests.
- `AdminDashboard.tsx` displays administrator information.
- `ManageUsers.tsx` handles registrations, users, deletion, and password resets.
- `ManageBooks.tsx` provides administrative catalog management.
- `AdminAnalytics.tsx` fetches and composes analytics data.

### Analytics components

The analytics components are reusable presentation components used by the administrator dashboard:

- `StatsCards.tsx` displays aggregate metrics.
- `BookAvailabilityChart.tsx` visualizes available inventory.
- `BorrowTrendChart.tsx` displays monthly borrowing trends.
- `FineAnalyticsChart.tsx` displays fine-related data.
- `TopBooksChart.tsx` displays the most borrowed books.
- `OverdueBooksTable.tsx` displays overdue books and fines.
- `RecentActivity.tsx` displays recent activity and notifications.

### Services

- `services/api.ts` centralizes API communication and the configured backend base URL.
- `services/mockAuth.ts` contains authentication-related client helpers.

## Backend Configuration

Create `backend/.env` from `backend/.env.example`.

```dotenv
PORT=5000
MONGODB_URI=mongodb+srv://<user>:<pass>@<cluster>/<db>
JWT_SECRET=change-me

ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=adminpass

LIBRARIAN_EMAIL=librarian@example.com
LIBRARIAN_PASSWORD=librarianpass

STUDENT_EMAIL=student@example.com
STUDENT_PASSWORD=studentpass

ADMIN_ID=admin
LIBRARIAN_ID=librarian1
STUDENT_ID=student1
```

Required variables are:

- `ADMIN_EMAIL`
- `ADMIN_PASSWORD`
- `LIBRARIAN_EMAIL`
- `LIBRARIAN_PASSWORD`
- `STUDENT_EMAIL`
- `STUDENT_PASSWORD`
- `JWT_SECRET`
- `MONGODB_URI`

`PORT`, `ADMIN_ID`, `LIBRARIAN_ID`, and `STUDENT_ID` have defaults.

## Authentication

The backend authenticates users through `/login`.

The login process checks:

1. MongoDB student records.
2. MongoDB admin and librarian records.
3. The fallback `users.json` file.

Successful authentication returns a one-hour JWT containing the user ID, email, and role.

Example token payload:

```json
{
  "id": "student1",
  "email": "student@example.com",
  "role": "student"
}
```

Protected requests must include:

```http
Authorization: Bearer <token>
```

The `authenticate(roles)` middleware validates the token and checks whether its role is allowed for the route. Missing or invalid tokens return `401`; valid tokens with insufficient permissions return `403`.

## Database Models

### Book

File: `backend/models/Book.js`

| Field | Type | Rules |
|---|---|---|
| `title` | String | Required |
| `author` | String | Required |
| `category` | String | Optional |
| `isbn` | String | Required and unique |
| `publisher` | String | Optional |
| `publishedYear` | Number | Optional |
| `language` | String | Defaults to `English` |
| `pages` | Number | Optional |
| `price` | Number | Defaults to `0`, minimum `0` |
| `finePerWeek` | Number | Defaults to `0`, minimum `0` |
| `copiesAvailable` | Number | Defaults to `1` |

### Student

File: `backend/models/Student.js`

| Field | Type | Rules |
|---|---|---|
| `studentId` | String | Required and unique |
| `email` | String | Required and unique |
| `password` | String | Required |
| `name` | String | Optional |
| `rollNumber` | String | Optional |
| `branch` | String | Optional |
| `section` | String | Optional |
| `role` | String | Defaults to `student` |
| `borrowedBooks` | Array | Embedded borrowing history |

Each `borrowedBooks` entry contains a book reference, borrowed date, due date, return state, return date, and weekly fine value.

### User

File: `backend/models/User.js`

Used for administrators and librarians.

| Field | Type | Rules |
|---|---|---|
| `userId` | String | Required and unique |
| `email` | String | Required and unique |
| `password` | String | Required |
| `name` | String | Optional |
| `role` | String | Required; `admin` or `librarian` |

### Registration

File: `backend/models/Registration.js`

Stores signup requests before approval.

| Field | Type | Rules |
|---|---|---|
| `name` | String | Optional |
| `email` | String | Required and unique |
| `password` | String | Required |
| `rollNumber` | String | Used for student requests |
| `branch` | String | Used for student requests |
| `section` | String | Used for student requests |
| `role` | String | `student`, `librarian`, or `admin` |
| `extra` | Object | Optional additional data |

### Reservation

File: `backend/models/Reservation.js`

| Field | Type | Description |
|---|---|---|
| `book` | ObjectId | References `Book` |
| `studentId` | String | Requesting student identifier |
| `studentEmail` | String | Requesting student email |
| `status` | String | `pending`, `approved`, or `rejected` |
| `approvedAt` | Date | Approval timestamp |

### Borrow

File: `backend/models/Borrow.js`

| Field | Type | Description |
|---|---|---|
| `book` | ObjectId | References `Book` |
| `student` | ObjectId | References `Student` |
| `issuedBy` | ObjectId | References issuing `User` |
| `borrowedAt` | Date | Defaults to current time |
| `dueDate` | Date | Required return deadline |
| `returned` | Boolean | Defaults to `false` |
| `returnDate` | Date | Set when returned |
| `fineAmount` | Number | Defaults to `0` |

### Notification

File: `backend/models/Notification.js`

Notifications contain a student reference, borrow reference, message, read flag, and timestamps.

## Borrowing Lifecycle

### Requesting a book

A student calls:

```http
POST /student/books/:bookId/request
```

The server verifies that the book exists and has available copies, then creates a pending reservation.

### Approving a reservation

A librarian calls:

```http
PATCH /librarian/reservations/:id/approve
```

The server:

1. Loads the reservation and associated book.
2. Confirms that the reservation is pending.
3. Confirms that a copy is available.
4. Decrements `copiesAvailable` atomically.
5. Sets the reservation status to `approved`.
6. Creates a due date fourteen days in the future.
7. Adds a borrowing entry to the student.
8. Creates a `Borrow` record.

### Returning a book

A librarian calls:

```http
PATCH /librarian/borrowed/:id/return
```

The server:

1. Loads the borrow record.
2. Rejects already returned records.
3. Increments the book inventory.
4. Marks the student’s embedded borrowing entry returned.
5. Removes the approved reservation.
6. Marks the borrow record returned and records the return date.

## Fine Calculation and Notifications

The current `calculateFine` function applies a fine only after the due date has passed:

```text
weeksLate = ceil(current-time - due-date / one-week)
weeklyFine = 25% of book price
fine = weeksLate × weeklyFine
```

The backend checks once per minute for 11:00 server time. It finds active borrows older than one week and creates one notification per student and borrow per day.

The stored `finePerWeek` field exists in the Book and Student schemas, but the current overdue calculation uses 25% of the book price. These two rules should be unified if configurable fines are required.

## Backend API Reference

All routes below that are not explicitly marked public require a JWT bearer token.

### Public authentication routes

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/student/signup` | Submit a student registration request |
| `POST` | `/librarian/signup` | Submit a librarian registration request |
| `POST` | `/admin/signup` | Submit an administrator registration request |
| `POST` | `/login` | Authenticate a student, librarian, or administrator |

### Registration routes

Allowed roles: `admin`, `librarian`.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/registrations` | List registrations; optional `role` query filter |
| `PATCH` | `/registrations/:id/approve` | Approve and convert a registration into an account |
| `DELETE` | `/registrations/:id` | Reject or remove a registration |

### Admin routes

Allowed role: `admin`.

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/admin/add-user` | Add an administrator or librarian |
| `GET` | `/admin/students` | List all students |
| `GET` | `/admin/librarians` | List all librarians |
| `DELETE` | `/admin/librarians/:id` | Delete a librarian |
| `PUT` | `/admin/students/:id/password` | Reset a student password |
| `PUT` | `/admin/librarians/:id/password` | Reset a librarian password |
| `POST` | `/admin/clear-student-borrows` | Close active loans and clear pending reservations |
| `GET` | `/admin/analytics/summary` | Return administrative analytics |

### Librarian routes

| Method | Endpoint | Allowed roles | Description |
|---|---|---|---|
| `GET` | `/librarian/students` | librarian, admin | List students |
| `DELETE` | `/librarian/students/:id` | librarian, admin | Remove a student and return active books |
| `PUT` | `/librarian/students/:id/password` | librarian | Reset a student password |
| `GET` | `/librarian/reservations` | librarian | List reservations; optional `status` filter |
| `PATCH` | `/librarian/reservations/:id/approve` | librarian | Approve a reservation and create a borrow |
| `GET` | `/librarian/borrowed-active` | librarian | List active loans |
| `POST` | `/librarian/books` | librarian, admin | Create a book |
| `PATCH` | `/librarian/borrowed/:id/return` | librarian | Mark a loan returned |

### Catalog routes

| Method | Endpoint | Authentication | Description |
|---|---|---|---|
| `GET` | `/books` | Public | List up to 50 books |
| `GET` | `/books?q=term` | Public | Search book titles case-insensitively |
| `POST` | `/librarian/books` | librarian, admin | Create a book |

### Student routes

Allowed role: `student`.

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/student/books/:bookId/request` | Request a book |
| `GET` | `/student/me` | Load profile information |
| `PUT` | `/student/password` | Change password |
| `GET` | `/student/reservations` | List reservations |
| `GET` | `/student/borrowed` | List overdue borrowed items and computed fines |
| `GET` | `/student/notifications` | List notifications |
| `PATCH` | `/student/notifications/:id/read` | Mark a notification read |

## Analytics

`GET /admin/analytics/summary` returns:

- Total books.
- Available copies.
- Total reservations.
- Pending reservations.
- Approved reservations.
- Total students.
- Total staff.
- Active loans.
- Overdue loans.
- Top five borrowed books.
- Borrowing trends for recent months.
- Overdue-book rows with calculated fines.
- Recent notification activity.

`AdminAnalytics.tsx` uses the response to render metric cards, charts, tables, and recent-activity components.

## Running the Project

### Prerequisites

- Node.js 18 or newer.
- MongoDB Atlas or a local MongoDB server.

### Backend setup

```bash
cd backend
cp .env.example .env
# Edit .env with MongoDB, JWT, and seed credentials
npm install
npm start
```

The backend listens on `http://localhost:5000` by default.

Backend scripts:

```text
npm start    node mockAuthServer.js
npm test     Intentionally exits with an error because no test suite is configured
```

### Frontend setup

```bash
cd frontend
cp .env.example .env
# Set VITE_API_BASE_URL when using a custom backend
npm install
npm run dev
```

Frontend scripts:

```bash
npm run dev       Start the Vite development server
npm run build     Type-check and create a production build
npm run lint      Run ESLint
npm run preview   Preview the production build
```

Frontend configuration:

```dotenv
VITE_API_BASE_URL=http://localhost:5000
```

The committed example uses the hosted backend URL instead.

## Deployment

The documented deployment model is:

- Build the frontend with Vite and deploy it to Vercel.
- Run the backend with Node.js on Render.
- Store application data in MongoDB Atlas.
- Configure all backend secrets and connection strings in the hosting provider environment.

## Security and Maintenance Notes

1. **Hash passwords.** Passwords are currently stored and compared as plain text. Use bcrypt or Argon2 before production deployment.
2. **Remove committed secrets.** A `backend/.env` file exists in the repository tree. Rotate any exposed credentials and remove the file from version control.
3. **Do not commit dependencies.** Remove `backend/node_modules` from source control and rely on `package-lock.json`.
4. **Split the backend.** `mockAuthServer.js` contains all routes, middleware, startup logic, analytics, and business operations. Separating these into routes, controllers, services, and middleware would improve maintainability.
5. **Add client-side route guards.** Backend authorization exists, but explicit frontend guards would prevent unauthenticated users from reaching protected page shells.
6. **Add automated tests.** The backend currently has no test suite, and `npm test` intentionally fails.
7. **Unify fine rules.** Decide whether fines are configured through `finePerWeek` or calculated as a percentage of book price.
8. **Use transactions where appropriate.** Reservation approval updates several collections and would be more resilient with a MongoDB transaction.
9. **Validate request data.** Add schema validation for email formats, password strength, ISBN format, pagination, and numeric ranges.
10. **Restrict CORS.** The backend currently enables broad CORS through `app.use(cors())`; production should allow only trusted frontend origins.

## Reference Files

- `README.md` — Quick-start project overview.
- `backend/API_END_POINTS.md` — Compact API reference.
- `backend/SCHEMAS.md` — Compact model reference.
- `frontend/routes.txt` — Frontend route inventory.
- `backend/mockAuthServer.js` — Backend implementation.
- `frontend/src/App.tsx` — Frontend route implementation.
