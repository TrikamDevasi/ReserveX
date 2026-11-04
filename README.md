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

## Recommended project structure

ReserveX uses a feature-oriented structure inside the Next.js App Router. Route files stay focused on page composition and HTTP handling; reusable UI, business logic, database models, and validation are kept in separate modules.

```text
ReserveX/
├── public/
│   ├── images/
│   └── icons/
├── src/
│   ├── app/
│   │   ├── (platform)/
│   │   │   ├── dashboard/
│   │   │   │   └── page.tsx
│   │   │   ├── resources/
│   │   │   │   ├── page.tsx
│   │   │   │   └── [resourceId]/
│   │   │   │       └── page.tsx
│   │   │   ├── bookings/
│   │   │   │   ├── page.tsx
│   │   │   │   └── [bookingId]/
│   │   │   │       └── page.tsx
│   │   │   ├── requests/
│   │   │   │   └── page.tsx
│   │   │   ├── calendar/
│   │   │   │   └── page.tsx
│   │   │   ├── inventory/
│   │   │   │   └── page.tsx
│   │   │   ├── issue-return/
│   │   │   │   └── page.tsx
│   │   │   ├── analytics/
│   │   │   │   └── page.tsx
│   │   │   ├── activity/
│   │   │   │   └── page.tsx
│   │   │   └── layout.tsx
│   │   ├── api/
│   │   │   ├── resources/
│   │   │   │   ├── route.ts
│   │   │   │   └── [resourceId]/
│   │   │   │       ├── route.ts
│   │   │   │       └── availability/
│   │   │   │           └── route.ts
│   │   │   ├── bookings/
│   │   │   │   ├── route.ts
│   │   │   │   └── [bookingId]/
│   │   │   │       ├── route.ts
│   │   │   │       ├── approve/
│   │   │   │       │   └── route.ts
│   │   │   │       ├── reject/
│   │   │   │       │   └── route.ts
│   │   │   │       ├── cancel/
│   │   │   │       │   └── route.ts
│   │   │   │       ├── issue/
│   │   │   │       │   └── route.ts
│   │   │   │       └── return/
│   │   │   │           └── route.ts
│   │   │   ├── dashboard/
│   │   │   │   └── route.ts
│   │   │   ├── analytics/
│   │   │   │   └── route.ts
│   │   │   └── activity/
│   │   │       └── route.ts
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── ui/
│   │   ├── layout/
│   │   ├── resources/
│   │   ├── bookings/
│   │   ├── inventory/
│   │   ├── dashboard/
│   │   └── shared/
│   ├── lib/
│   │   ├── db/
│   │   │   └── mongoose.ts
│   │   ├── utils.ts
│   │   ├── constants.ts
│   │   └── date-time.ts
│   ├── models/
│   │   ├── Resource.ts
│   │   ├── Booking.ts
│   │   └── ActivityLog.ts
│   ├── services/
│   │   ├── resource.service.ts
│   │   ├── availability.service.ts
│   │   ├── booking.service.ts
│   │   ├── inventory.service.ts
│   │   └── analytics.service.ts
│   ├── validations/
│   │   ├── resource.schema.ts
│   │   └── booking.schema.ts
│   ├── types/
│   │   ├── resource.ts
│   │   ├── booking.ts
│   │   └── api.ts
│   └── hooks/
├── tests/
│   ├── unit/
│   └── integration/
├── .env.example
├── .env.local                 # local only; never commit
├── .gitignore
├── next.config.ts
├── package.json
├── tsconfig.json
└── README.md
```

### Folder responsibilities

| Folder | Responsibility |
| --- | --- |
| `src/app/` | App Router pages, layouts, loading/error UI, and Route Handlers |
| `src/app/api/` | HTTP request parsing, validation, service invocation, and response mapping |
| `src/components/` | Reusable UI grouped by shared purpose or product feature |
| `src/lib/db/` | Mongoose connection setup and database infrastructure |
| `src/models/` | Mongoose schemas and model definitions |
| `src/services/` | Booking rules, availability calculations, inventory operations, and business workflows |
| `src/validations/` | Zod schemas for validating incoming data |
| `src/types/` | Shared TypeScript domain and API types |
| `src/hooks/` | Reusable client-side React hooks |
| `tests/` | Unit and integration tests for critical workflows |
| `public/` | Static assets served directly by Next.js |

### Architecture rules

- Keep business rules out of React components and Route Handler files; put them in `services/`.
- Route Handlers should validate input, call a service, and return a consistent HTTP response.
- Keep database access in models and service-layer operations; do not connect to MongoDB from client components.
- Use `server-only` for modules that must never enter the client bundle, especially database and secret-dependent code.
- Use Zod schemas at API boundaries and infer TypeScript types where practical.
- Keep resource availability derived from overlapping Approved and Issued bookings; do not store a manually maintained available-quantity value.
- Use MongoDB transactions for approval operations that must atomically recheck capacity, update the booking, and write an activity log. Configure Atlas as a replica set.
- Add folders and files as the corresponding milestone is implemented. This is the target structure, not a claim that every file already exists in the repository.

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
