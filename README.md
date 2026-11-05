# ReserveX — Equipment & Resource Booking System

An equipment and resource management platform built to help colleges, laboratories, offices, and organizations share and track equipment and facilities—eliminating double bookings, providing real-time availability, and managing the complete reservation, issue, and return lifecycle.

> **Project Status:** In Development  
> **Project Type:** B.Tech Semester 3 Academic Project

---

## 📌 Problem Statement

Colleges, laboratories, offices, and organizations share equipment and resources such as cameras, projectors, laptops, meeting rooms, laboratory instruments, and sports equipment.

When bookings are managed manually (via spreadsheets, logbooks, or verbal requests), users face persistent challenges:
- **Double bookings & scheduling conflicts** due to lack of real-time synchronization.
- **Unclear availability**, making planning events, classes, or research experiments difficult.
- **Lost tracking of issued and returned equipment**, leading to unaccounted inventory, missing accessories, or unchecked damage.

**ReserveX** addresses this by providing an end-to-end resource booking platform where users can browse resources, check availability for specific dates/times, and track reservations, while resource managers can manage inventory, process booking requests, record handovers and returns, and monitor resource utilization.

---

## 🎯 Expected Outcome

- **For Requesters (Students, Faculty, Staff):**
  - Discover resources across categories and locations.
  - Verify live availability before booking.
  - Submit booking requests with specified purpose, time slots, and quantities.
  - Track reservation status (Pending, Approved, Rejected, Cancelled, Completed) with unique booking references.

- **For Resource Managers (Lab Technicians, Facilities, Admins):**
  - Manage inventory catalog and define availability.
  - Review incoming requests, detect scheduling conflicts, and approve/reject with contextual notes.
  - Record physical check-outs (issue) and check-ins (return), logging item condition.
  - Monitor overdue returns, track usage history, and review utilization analytics.

---

## 🚀 Core Features

### 1. Resource Management
- **CRUD Operations:** Add, view, update, and remove resources.
- **Resource Details:** Store name, description, category, images, location, and usage instructions.
- **Capacity & Inventory:** Define total quantity available and toggle availability state.
- **Discovery:** Fast search and multi-facet filtering by category and physical location.

### 2. Availability Management
- **Date & Time Range Selection:** Specify exact start and end timestamps.
- **Dynamic Availability Engine:** Display available resources and remaining quantities for the selected period.
- **Conflict Prevention:** Strict calculation prevents overlapping bookings from exceeding total inventory capacity.
- **Visual Slots:** Clearly indicate fully booked dates or unavailable time windows.

### 3. Booking Requests
- **Request Submission:** Submit requests specifying purpose, date range, and required quantity.
- **Unique Booking Reference:** Automatically generate human-readable, unique identifiers for each reservation (e.g., `RES-2026-XXXX`).
- **Status Lifecycle:** Track states: `Pending`, `Approved`, `Rejected`, `Cancelled`, and `Completed`.
- **Self-Service Cancellation:** Allow users to cancel eligible pending or approved reservations before the start time.
- **Booking History:** Detailed view of past and upcoming reservations for each user.

### 4. Booking Management
- **Approval Queue:** Centralized dashboard for managers to review incoming requests.
- **Decision Workflow:** Approve or reject requests with audit notes and feedback.
- **Calendar & List Views:** View bookings across daily, weekly, or monthly calendar grids and tabular lists.
- **Conflict Detection:** Highlight conflicting requests and overlapping demands during review.
- **Active Tracker:** Live view of resources currently issued vs. returned.

### 5. Return & Usage Tracking
- **Handovers:** Record issue and actual return timestamps.
- **Condition Logging:** Record item condition upon return (e.g., *Good*, *Minor Wear*, *Damaged*, *Needs Maintenance*).
- **Overdue Tracking:** Automatically flag and highlight items overdue for return.
- **Historical Audit Trail:** Maintain complete usage records per resource and user.
- **Real-Time Restocking:** Automatically release capacity back to available inventory once returned.

### 6. Dashboard & Analytics
- **Live Metrics:** Total inventory, currently booked/issued resources, and available stock.
- **Actionable Alerts:** Count of pending approval requests and overdue returns requiring attention.
- **Resource Insights:** Identify top frequently booked resources and high-demand categories.
- **Utilization Trends:** Breakdown of booking volume by category and date ranges.

---

## 🌟 Optional Enhancements

- **QR Code Generation:** Generate scannable QR codes for booking references.
- **QR Handover Scanning:** Scan booking reference QR codes with mobile devices for rapid equipment issue and return.
- **Notifications:** Automated approval, rejection, and due-date reminder emails.
- **Smart Rescheduling:** Suggest alternative available time slots or similar resources when a request conflicts.
- **Utilization Reporting:** Exportable PDF/CSV reports on equipment downtime, usage frequency, and department allocations.

---

## 🚫 Scope & Restrictions

- **No Online Payments:** Rental charges, security deposits, and online payment gateway integrations are out of scope.
- **No Physical Hardware Integration:** Physical smart locks, IoT lockers, and automated RFID gates are not required.
- **Core Priority:** High-fidelity availability calculation, conflict prevention, approval workflows, and reliable issue/return tracking.

---

## 🔄 Booking Lifecycle State Machine

```text
[Pending]
   │
   ├──► [Approved]
   │       │
   │       ├──► [Issued] (Physical handover recorded)
   │       │       │
   │       │       └──► [Completed] (Returned & condition inspected)
   │       │
   │       └──► [Cancelled] (Cancelled prior to handover)
   │
   ├──► [Rejected] (Declined by manager with note)
   │
   └──► [Cancelled] (Cancelled by requester while pending)
```

### Availability & Overlap Rules
- Two intervals overlap when: `Request.Start < Existing.End && Request.End > Existing.Start`.
- Adjacent bookings (e.g., `10:00 - 11:00` and `11:00 - 12:00`) do not conflict.
- Only confirmed states (`Approved` and `Issued`) consume inventory capacity.
- `Pending` requests do not lock capacity until approved; manager approvals atomically re-validate remaining capacity.
- Overdue items remain in `Issued` status and continue consuming capacity until explicitly marked as returned.

---

## 🎨 Useful Screens & Design References

### Recommended Screens to Study
1. **Resource Catalogue:** Grid and list views with search, category chips, location filter, and real-time availability badges.
2. **Availability Calendar:** Interactive timeline / Gantt view showing resource allocation across hours and days.
3. **Booking Form:** Date/time range picker, quantity selector, purpose field, and instant validation feedback.
4. **Booking Approval Queue:** Manager review list with conflict warnings, requester info, and single-click approve/reject actions.
5. **Equipment Inventory:** Asset table showing total stock, active issues, maintenance status, and quick actions.
6. **Return & Handover Desk:** Quick lookup screen to scan/enter booking references, record return condition, and check in gear.
7. **Booking History & Analytics:** Recharts-powered graphs and summary cards for inventory utilization.

### Industry & Design References
- [Cheqroom — Equipment booking and asset management](https://www.cheqroom.com/)
- [Skedda — Space and resource booking](https://www.skedda.com/)
- [Dribbble — Booking calendar dashboard designs](https://dribbble.com/search/booking-calendar-dashboard)
- [Dribbble — Room and resource booking UI designs](https://dribbble.com/search/room-booking)
- [Behance — Equipment booking system UI designs](https://www.behance.net/search/projects/equipment%20booking%20system)

---

## 🛠️ Planned Technology Stack

| Layer | Technology | Purpose |
| --- | --- | --- |
| **Framework** | Next.js 15 (App Router) | Full-stack React framework with SSR and Route Handlers |
| **Language** | TypeScript | Type safety across client and server |
| **Styling** | Tailwind CSS | Utility-first responsive design system |
| **Database** | MongoDB Atlas | Document database for flexible resource schemas |
| **ODM** | Mongoose | Schema modeling, indexes, and transaction support |
| **Validation** | Zod | Runtime schema validation for forms and API routes |
| **Forms** | React Hook Form | Controlled, performant form state management |
| **Visualizations** | Recharts | Analytics dashboards and utilization graphs |
| **Icons** | Lucide React | Consistent iconography |
| **Date Utilities** | date-fns | Interval overlap calculations and date formatting |
| **Deployment** | Vercel | Production hosting and edge optimizations |

---

## 🗄️ Domain Data Model Overview

### `Resource`
```typescript
{
  name: string;
  description: string;
  category: "Electronics" | "Lab Equipment" | "Meeting Rooms" | "Sports" | "Audio/Visual" | "Other";
  images: string[];
  location: string;
  totalQuantity: number;
  isActive: boolean;
  instructions?: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### `Booking`
```typescript
{
  bookingReference: string; // e.g. "RES-2026-9842"
  resourceId: ObjectId;
  requester: {
    name: string;
    email: string;
    department?: string;
    role?: string;
  };
  purpose: string;
  quantity: number;
  startDate: Date;
  endDate: Date;
  status: "Pending" | "Approved" | "Rejected" | "Cancelled" | "Issued" | "Completed";
  managerNote?: string;
  issuedAt?: Date;
  returnedAt?: Date;
  returnCondition?: "Good" | "Fair" | "Damaged" | "Maintenance Required";
  createdAt: Date;
  updatedAt: Date;
}
```

### `ActivityLog`
```typescript
{
  action: "CREATED" | "APPROVED" | "REJECTED" | "CANCELLED" | "ISSUED" | "RETURNED" | "UPDATED";
  entityType: "RESOURCE" | "BOOKING";
  entityId: ObjectId;
  actor: string;
  notes?: string;
  timestamp: Date;
}
```

---

## 📂 Target Project Structure

```text
ReserveX/
├── public/
│   ├── images/
│   └── icons/
├── src/
│   ├── app/
│   │   ├── (platform)/
│   │   │   ├── dashboard/
│   │   │   │   └── page.tsx           # Manager overview & KPI metrics
│   │   │   ├── resources/
│   │   │   │   ├── page.tsx           # Resource catalogue & search
│   │   │   │   └── [resourceId]/
│   │   │   │       └── page.tsx       # Resource details & booking request form
│   │   │   ├── bookings/
│   │   │   │   ├── page.tsx           # User's bookings & status tracking
│   │   │   │   └── [bookingId]/
│   │   │   │       └── page.tsx       # Booking confirmation & details
│   │   │   ├── requests/
│   │   │   │   └── page.tsx           # Manager approval queue & decisions
│   │   │   ├── calendar/
│   │   │   │   └── page.tsx           # Visual availability calendar / timeline
│   │   │   ├── inventory/
│   │   │   │   └── page.tsx           # Inventory management (CRUD)
│   │   │   ├── issue-return/
│   │   │   │   └── page.tsx           # Handover & return check-in desk
│   │   │   ├── analytics/
│   │   │   │   └── page.tsx           # Utilization reports & statistics
│   │   │   ├── activity/
│   │   │   │   └── page.tsx           # Historical audit log
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
│   │   │   │       ├── approve/route.ts
│   │   │   │       ├── reject/route.ts
│   │   │   │       ├── cancel/route.ts
│   │   │   │       ├── issue/route.ts
│   │   │   │       └── return/route.ts
│   │   │   ├── dashboard/route.ts
│   │   │   └── analytics/route.ts
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── ui/                        # Buttons, inputs, modals, badges, dropdowns
│   │   ├── layout/                    # Header, sidebar, shell
│   │   ├── resources/                 # Resource card, catalogue filters, search bar
│   │   ├── bookings/                  # Booking timeline, form, status badge
│   │   ├── inventory/                 # Inventory table, add/edit modal
│   │   ├── dashboard/                 # Metric cards, overdue alert banner
│   │   └── shared/                    # Empty states, confirmation dialogs
│   ├── lib/
│   │   ├── db/
│   │   │   └── mongoose.ts            # Cached Mongoose connection helper
│   │   ├── utils.ts                   # Class merge (clsx/twMerge) & helpers
│   │   ├── constants.ts               # Categories, statuses, config values
│   │   └── date-time.ts               # Overlap logic & date formatters
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
│       ├── useAvailability.ts
│       └── useBookings.ts
├── tests/
│   ├── unit/
│   └── integration/
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
└── README.md
```

---

## 🚦 Getting Started

### Prerequisites
- **Node.js**: v20 or later
- **npm** or **pnpm**
- **MongoDB Atlas** cluster or local MongoDB instance
- **Git**

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/TrikamDevasi/ReserveX.git
   cd ReserveX
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure environment variables:**
   Create a `.env.local` file in the root directory:
   ```env
   MONGODB_URI=your_mongodb_connection_string
   NEXT_PUBLIC_APP_URL=http://localhost:3000
   ```

4. **Run the local development server:**
   ```bash
   npm run dev
   ```

5. **Open in browser:**
   Navigate to [http://localhost:3000](http://localhost:3000).

---

## 🗺️ Implementation Roadmap

- [ ] Project initialization with Next.js App Router & Tailwind CSS
- [ ] MongoDB Atlas connection and Mongoose schemas (`Resource`, `Booking`, `ActivityLog`)
- [ ] Resource catalogue with search, category filtering, and location filters
- [ ] Availability check algorithm (preventing overlapping over-capacity bookings)
- [ ] Booking request form with validation and unique reference generation
- [ ] Manager approval workflow (capacity recheck, approve/reject with notes)
- [ ] Equipment issue & return check-in desk with condition recording
- [ ] Overdue items tracking and alert indicators
- [ ] Interactive availability calendar & timeline view
- [ ] Dashboard metrics & analytics graphs (total/booked/available, utilization rates)
- [ ] Unit & integration tests for overlap detection and state transitions

---

## 👤 Author

**Trikam Devasi**  
- GitHub: [@TrikamDevasi](https://github.com/TrikamDevasi)  
- College Project: B.Tech Semester 3 — Next.js Portfolio Project
