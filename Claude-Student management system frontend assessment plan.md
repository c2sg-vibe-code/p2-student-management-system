# Student management system frontend assessment plan

**Created:** 6/16/2026 21:20:48  
**Updated:** 6/16/2026 21:20:48  
**Exported:** 6/18/2026 14:51:09  
**Link:** [https://claude.ai/share/6b796b2d-41aa-4458-ab96-953f78582e1d](https://claude.ai/share/6b796b2d-41aa-4458-ab96-953f78582e1d)  

## User:

> 6/16/2026 20:46:16

You are acting as a senior frontend engineer helping me complete a timed frontend assessment.
Read everything carefully before proposing a solution.
Assessment Context
I need to build a Student Management System prototype.
The application must be built as a frontend-only application.
Persistence MUST use browser localStorage.
There is NO backend.
There is NO database.
There are NO API routes.
There are NO Server Actions.
The application will be deployed to GitHub and Vercel.
Technology stack:

* Next.js App Router
* TypeScript
* Tailwind CSS
* React Hook Form
* Zod
* TanStack Table
The goal is to complete the assessment efficiently while maintaining clean architecture and good engineering practices.
Functional Requirements
1. Student Registration
Create a form that allows adding students.
Fields:

* Student ID
* Name
* Email
* Program
* Academic Year
Display all students in a table.
2. Course Enrollment
Create a form that allows enrolling students.
Fields:

* Enrollment ID
* Student ID
* Course ID
* Enrollment Date
Display all enrollments in a table.
3. Grade Submission
Create a form that allows submitting grades.
Fields:

* Grade ID
* Student ID
* Course ID
* Grade
* Semester
Display all grades in a table.
Validation:

* Grade must be between 0 and 100.
Display all grades in a table.
Data Persistence
Use browser localStorage.
The application must:

* Load seed data on first run.
* Persist newly created records.
* Restore data after refresh.
Bonus Features
Student ID Validation
Format:
2620009
Meaning:

* First 2 digits = admission year
* Third digit = shift (1 = Morning, 2 = Day)
* Last 4 digits = student serial
Registration is only allowed for the current year.
Show validation errors if invalid.
Table Features
Implement:

* Search
* Sorting
* Pagination
URL State Persistence (Optional)
Persist:

* Search query
* Sort field
* Sort direction
* Current page
Example:
/students?page=2&search=ayesha&sort=name&order=asc
Seed Data
(Insert the full seed dataset here)
Important Constraints
Do NOT generate:

* Server Actions
* API routes
* Prisma
* Supabase
* Database layers
* Authentication
* Dynamic detail pages
* Edit pages
* Delete pages
* Backend architecture
Everything should work entirely in the browser using localStorage.
What I Want From You
First, do NOT generate code.
Instead provide:

1. Recommended folder structure.
2. High-level architecture.
3. State management approach.
4. localStorage strategy.
5. Validation strategy.
6. Table strategy.
7. Build order optimized for a 45-minute assessment.
8. Risk areas and common mistakes.
9. Recommended implementation phases.
Wait for my approval before generating any code.

## Assistant:

> 6/16/2026 20:47:38

## Student Management System — Architecture & Planning Document

---

### 1. Recommended Folder Structure

```
src/
├── app/
│   ├── layout.tsx                  # Root layout, nav shell (Server Component)
│   ├── page.tsx                    # Redirect → /students
│   ├── students/
│   │   └── page.tsx                # Thin shell → renders <StudentsPage />
│   ├── enrollments/
│   │   └── page.tsx
│   └── grades/
│       └── page.tsx
│
├── components/
│   ├── layout/
│   │   └── Navbar.tsx              # "use client" — active link state
│   ├── students/
│   │   ├── StudentsPage.tsx        # "use client" — owns local state
│   │   ├── StudentForm.tsx
│   │   └── StudentsTable.tsx
│   ├── enrollments/
│   │   ├── EnrollmentsPage.tsx
│   │   ├── EnrollmentForm.tsx
│   │   └── EnrollmentsTable.tsx
│   ├── grades/
│   │   ├── GradesPage.tsx
│   │   ├── GradeForm.tsx
│   │   └── GradesTable.tsx
│   └── ui/
│       ├── InputField.tsx          # Reusable labeled input + error
│       ├── SelectField.tsx
│       └── DataTable.tsx           # Generic TanStack Table wrapper
│
├── lib/
│   ├── schemas.ts                  # All Zod schemas
│   ├── seed.ts                     # Seed dataset + bootstrap logic
│   ├── storage.ts                  # Typed localStorage get/set/init
│   └── hooks/
│       ├── useStudents.ts
│       ├── useEnrollments.ts
│       └── useGrades.ts
│
└── types/
    └── index.ts                    # Student, Enrollment, Grade interfaces
```

**Key rule:** Every file that touches `window`, hooks, or forms gets `"use client"`. The `app/*/page.tsx` files stay as thin server shells — they just import and render the corresponding `*Page.tsx` client component. This is the cleanest App Router pattern.

---

### 2. High-Level Architecture

```
app/students/page.tsx  (Server Component — zero JS)
        │
        └── <StudentsPage />  ← "use client"
                │
                ├── useStudents()           ← reads/writes localStorage
                │       └── useState([])   ← source of truth in memory
                │
                ├── <StudentForm />         ← RHF + Zod, calls hook.add()
                └── <StudentsTable />       ← TanStack Table, URL state sync
```

All three modules (Students, Enrollments, Grades) follow **identical structure**. This repetition is intentional — it's fast to build and easy to reason about under time pressure.

---

### 3. State Management Approach

**No global state manager.** Use three independent custom hooks, one per domain:

```
useStudents()     →  { students, addStudent }
useEnrollments()  →  { enrollments, addEnrollment }
useGrades()       →  { grades, addGrade }
```

Each hook:
- Initializes from localStorage on mount (inside `useEffect` — never on render)
- Holds data in `useState`
- On every mutation, updates both `useState` and localStorage atomically

This is intentionally simple. No Context, no Zustand, no Jotai. The scope doesn't justify it and it costs build time.

---

### 4. localStorage Strategy

**Three keys, one responsibility each:**

| Key | Value |
|---|---|
| `sms_students` | `Student[]` |
| `sms_enrollments` | `Enrollment[]` |
| `sms_grades` | `Grade[]` |

**Bootstrap logic** lives in `lib/seed.ts`:

```
On first run:
  → localStorage key missing or null
  → write seed data to all three keys
  → hook reads and returns that data

On subsequent runs:
  → key exists
  → parse and return existing data (seed is never re-applied)
```

**SSR guard** — wrap all localStorage access:

```ts
// lib/storage.ts
const isBrowser = typeof window !== 'undefined'
```

All reads/writes go through typed helpers in `storage.ts`. Never access `localStorage` directly in components.

**⚠️ Seed data note:** You wrote `(Insert the full seed dataset here)` — make sure you have at least 15–20 students, 10+ enrollments, and 10+ grades for pagination to be demonstrable. I'll generate realistic BRAC-style seed data when we enter the build phase.

---

### 5. Validation Strategy

All schemas in a single file: `lib/schemas.ts`

**Student ID bonus validation** (current year = 2026, so prefix = `26`):

```
Format:  [YY][S][SSSS]
         26   1  0009   → Morning shift, serial 0009

Regex:   ^26[12]\d{4}$

Rules enforced via Zod .refine():
  - Must be exactly 7 digits
  - First 2 digits must equal current year's last 2 ("26")
  - 3rd digit must be 1 (Morning) or 2 (Day)
  - Remaining 4 digits are the serial
  - Show shift label in success/error message
```

**Grade validation:**

```
z.number().min(0).max(100)
```

**Duplicate ID check** — handled at the hook level (not Zod), since Zod has no access to existing records. The `addStudent` function checks `students.some(s => s.studentId === newId)` before appending.

**Form wiring:** `zodResolver` from `@hookform/resolvers/zod` on every form. No manual `register` parsing — use the resolver, extract `formState.errors`, done.

---

### 6. Table Strategy

Use **TanStack Table v8** (`@tanstack/react-table`).

Build one **generic `<DataTable />`** component that accepts:
- `columns` definition
- `data` array
- `searchKey` (column to filter on)

This handles all three tables without repetition.

**Features per table:**

| Feature | Implementation |
|---|---|
| Search | `globalFilterFn` or column filter on `useReactTable` |
| Sorting | `getSortedRowModel()` — click column headers |
| Pagination | `getPaginationRowModel()` — page size 10 |
| URL state (bonus) | `useSearchParams` + `useRouter` synced to table state |

**URL state gotcha (critical):** `useSearchParams()` in App Router **requires a `<Suspense>` boundary**. Wrap the table component:

```tsx
// in StudentsPage.tsx
<Suspense fallback={<TableSkeleton />}>
  <StudentsTable ... />
</Suspense>
```

Forgetting this causes a build-breaking error that's hard to debug under time pressure.

---

### 7. Build Order (45-Minute Assessment)

```
⏱ 0:00 – 0:08  │ FOUNDATION
                │ types/index.ts → lib/schemas.ts → lib/storage.ts
                │ lib/seed.ts → lib/hooks/ (all three, stubbed)
                │ app/layout.tsx + Navbar (3 links)

⏱ 0:08 – 0:20  │ STUDENTS MODULE  (most complex — ID validation)
                │ useStudents.ts (complete)
                │ StudentForm.tsx (RHF + Zod + ID validation)
                │ StudentsTable.tsx (TanStack, search + sort + pagination)
                │ StudentsPage.tsx + app/students/page.tsx

⏱ 0:20 – 0:30  │ ENROLLMENTS MODULE  (copy/adapt Students pattern)
                │ useEnrollments.ts
                │ EnrollmentForm.tsx
                │ EnrollmentsTable.tsx + page wiring

⏱ 0:30 – 0:40  │ GRADES MODULE  (copy/adapt, add grade range validation)
                │ useGrades.ts
                │ GradeForm.tsx (grade 0–100 validation)
                │ GradesTable.tsx + page wiring

⏱ 0:40 – 0:45  │ POLISH + BONUS
                │ Verify seed bootstrap works on refresh
                │ URL state sync (if time allows — students table first)
                │ Basic Tailwind polish on forms/table
                │ Test edge cases: duplicate ID, grade > 100
```

---

### 8. Risk Areas & Common Mistakes

**🔴 High Risk**

| Risk | Mitigation |
|---|---|
| `useSearchParams` without `<Suspense>` → build fails | Always wrap in Suspense from the start |
| localStorage read during SSR → `window is not defined` | All reads inside `useEffect`, never on render |
| TanStack Table: forgetting `getCoreRowModel()` → silent empty table | It's required; add it first |
| RHF `register` vs `Controller` confusion for selects | Use `Controller` for all `<select>` elements |
| Seed re-applying on every refresh → duplicates | Gate seed on key absence, not data length |

**🟡 Medium Risk**

| Risk | Mitigation |
|---|---|
| `zodResolver` type mismatch (string vs number for grade) | Use `z.coerce.number()` for numeric inputs |
| Pagination breaking when search reduces total rows | Reset to page 0 on filter change |
| Student ID year hardcoded vs dynamic | Use `new Date().getFullYear() % 100` |
| `app/page.tsx` showing blank | Add `redirect('/students')` immediately |

**🟢 Time-waster traps to avoid**

- Don't build edit/delete — the spec forbids it, don't spend a second on it
- Don't over-engineer storage — no class, no event emitter, just typed functions
- Don't build a shared form component across all three — forms differ enough to hurt you
- Don't start with styling — functionality first, Tailwind classes last

---

### 9. Implementation Phases

```
Phase 1 — Data Layer (build once, never revisit)
  Types → Zod schemas → localStorage helpers → seed data → custom hooks
  Output: The entire data layer is done. No component touches localStorage directly.

Phase 2 — Shell (10 minutes)
  layout.tsx → Navbar → page.tsx files (all three, thin shells)
  Output: Navigation works. Pages render placeholders.

Phase 3 — Feature Modules (build in order: Students → Enrollments → Grades)
  Each module = Form + Table + Page wrapper
  Students first because it has the most validation logic.
  Enrollments and Grades are structurally identical — copy and adapt.

Phase 4 — Table Features
  Add search, sort, pagination to all three tables using the generic DataTable.
  Since all three tables use the same wrapper, this is done once and applied three times.

Phase 5 — Bonus (time permitting)
  URL state sync: Students table first (highest value), then others if time allows.
  Only add this after Phases 1–4 are confirmed working.
```

---

**Ready to proceed?** Once you confirm this plan, I'll generate all the code in a clean build order starting with the data layer. If you have the seed data to include, paste it now and I'll integrate it into `lib/seed.ts` directly.

## User:

> 6/16/2026 20:48:52

Generate Phase 1 only.
Create the complete code for:

1. types/index.ts
2. lib/seed.ts
3. lib/storage.ts
4. lib/schemas.ts
Requirements:

* Use the exact seed dataset provided in the assessment
* TypeScript strict mode
* Browser localStorage only
* No Server Actions
* No API routes
* No database
* No placeholders
* No pseudocode
For storage.ts provide:

* initializeStorage()
* getStudents()
* getEnrollments()
* getGrades()
* addStudent()
* addEnrollment()
* addGrade()
All functions should be fully implemented and production-ready.
After generating the code, explain where each file should be created.

## Assistant:

> 6/16/2026 20:51:55

> Ran command
>
> - Create directory structure for Phase 1 files
> - **Done**

Powered by Claude Exporter (https://www.ai-chat-exporter.net)
