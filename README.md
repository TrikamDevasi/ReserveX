# ReserveX

A campus resource and equipment booking platform built to help students, faculty, and campus managers discover shared resources, request bookings, manage approvals, and track equipment issue and return workflows.

> **Project status:** In development  
> **Project type:** B.Tech Semester 3 academic project

## Overview

Colleges and universities share equipment and facilities such as projectors, cameras, microphones, lab kits, sports equipment, and meeting spaces. When requests and handovers are managed informally, availability becomes unclear, double-bookings happen, and equipment returns are difficult to track.

ReserveX brings these workflows into one place. It is designed around the complete resource lifecycle:

**Discover → Check availability → Request → Approve → Issue → Return → Review history**

## Core features

- **Resource catalogue:** Browse resources by category, location, and status.
- **Availability checking:** Check capacity for a selected time range before submitting a request.
- **Booking requests:** Submit a request with purpose, quantity, and requested dates/times.
- **Approval workflow:** Managers can approve or reject pending requests with an optional note.
- **Booking management:** View booking status and cancel eligible bookings.
- **Issue and return tracking:** Record physical handover, expected return, actual return, and item condition.
- **Inventory management:** Maintain resource details and quantities while preserving historical booking records.
- **Activity history:** Keep a traceable record of important booking and inventory actions.
- **Overview and analytics:** Surface operational information such as pending requests, active issues, overdue returns, and resource usage.

Features will be delivered incrementally. Optional enhancements such as QR-based lookup and notifications are not prerequisites for the core booking workflow.

## Booking lifecycle

```text
Pending
  ├── Approved
  │     ├── Issued
  │     │     └── Completed
  │     └── Cancelled
  ├── Rejected
  └── Cancelled
```

Only valid state transitions should be allowed. An issued item remains unavailable until it is physically returned and the return is recorded.

## Important availability rules

- Two time ranges overlap when `A.start < B.end && A.end > B.start`.
- Adjacent bookings (where one ends exactly when another starts) do not overlap.
- Pending requests do not consume confirmed inventory capacity.
- Approved and Issued bookings count against capacity during overlapping periods.
- Availability must be checked again when a manager approves a request.
- Availability is calculated from booking records rather than maintained as a manually edited counter.
- An overdue item is not considered returned automatically; it remains issued until a return is recorded.

## Planned technology stack

| Layer | Technology |
| --- | --- |
| Web application | Next.js App Router |
| Language | TypeScript |
| UI styling | Tailwind CSS |
| API | Next.js Route Handlers |
| Database | MongoDB Atlas |
| Data modelling | Mongoose |
| Validation | Zod |
| Forms | React Hook Form |
| Charts | Recharts |
| Icons | Lucide React |
| Date utilities | date-fns |
| Deployment target | Vercel |

Dependencies and implementation details may evolve as development progresses.

## Data model overview

The initial domain model is expected to include:

### Resource
Stores the resource name, description, category, location, total quantity, active status, usage instructions, and timestamps.

### Booking
Stores the booking reference, resource, requester details, department, purpose, quantity, requested start/end times, status, manager note, and issue/return timestamps and condition.

### ActivityLog
Records important actions, the affected booking/resource, actor information, a description, and relevant metadata.

Historical records should be preserved. Resources with booking history should generally be deactivated rather than hard-deleted.

## Getting started

### Prerequisites

- Node.js 20 or later
- npm
- A MongoDB Atlas database
- Git

### Installation

Clone the repository:

```bash
git clone https://github.com/TrikamDevasi/ReserveX.git
cd ReserveX
```

Install dependencies:

```bash
npm install
```

Create a local environment file named `.env.local`:

```env
MONGODB_URI=your_mongodb_atlas_connection_string
```

Do not commit real credentials or secrets to Git.

Start the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

> The commands above assume the Next.js application and its dependencies have been added to this repository. The repository README does not imply that the application source code has already been pushed.

## Development roadmap

- [ ] Initialize the Next.js TypeScript App Router project
- [ ] Configure MongoDB Atlas and Mongoose
- [ ] Implement database connection handling and domain models
- [ ] Build resource catalogue and resource details
- [ ] Implement availability calculation
- [ ] Implement booking creation and validation
- [ ] Implement manager approval/rejection and capacity rechecks
- [ ] Implement issue and return workflows
- [ ] Add activity history
- [ ] Build responsive overview and analytics
- [ ] Add automated tests for booking overlap and status transitions
- [ ] Deploy the application

## Security and scope

Authentication and authorization are not part of the initial assignment scope. Any role selector used during demonstrations is a **demo convenience only** and must not be treated as real access control.

MongoDB credentials must remain in environment variables. Production deployments should add proper authentication, authorization, input validation, rate limiting, and secure audit controls before being used with real campus data.

## UI direction

The interface is intended to feel like a practical campus operations product:

- Responsive layouts for desktop, tablet, and mobile
- Clear navigation and task-focused screens
- Accessible forms, readable tables, and visible interaction states
- Restrained neutral surfaces with a consistent green accent
- Explicit loading, empty, error, disabled, and success states

## Academic context

ReserveX is being developed as a B.Tech Semester 3 Next.js project. The project brief is based on the [Semester 3 Next.js portfolio project assignment](https://github.com/codinggita/CGxSU_Semester_1/blob/main/Semester_3/NextJS/nextjs-portfolio-project.md).

## Author

**Trikam Devasi**  
GitHub: [@TrikamDevasi](https://github.com/TrikamDevasi)

---

Built as an academic project to practise full-stack application development, database modelling, API design, and reliable booking workflows.
