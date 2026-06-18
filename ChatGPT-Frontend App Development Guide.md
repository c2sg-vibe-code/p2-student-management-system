**You:**

You are asked to build a small frontend application using any tools, technologies, AI assistants, documentation, or online resources you normally use during software development.

You may use ChatGPT, Claude, Cursor, Copilot, Stack Overflow, official documentation, Google searches, or any other resources.

We are interested in understanding your normal development workflow.

Time Limit
45 Minutes

Scenario
You have been asked to create a simple Student Management System prototype for a university department.

The application should use browser localStorage for persistence.

A seed dataset will be provided.

Requirements
1. Student Registration
Create a form that allows users to add new students.

Required fields:

Student ID
Name
Email
Program
Academic Year
Display all students in a table.

2. Course Enrollment
Create a form that allows enrolling a student into a course.

Required fields:

Enrollment ID
Student ID
Course ID
Enrollment Date
Display all enrollments in a table.

3. Grade Submission
Create a form that allows submitting grades.

Required fields:

Grade ID
Student ID
Course ID
Grade
Semester
Display all submitted grades in a table.

Validation Requirement:

Grade must be between 0 and 100.
Display all grades in a table.

Data Persistence
Use localStorage to store newly added records.

The application should load the provided seed data on first run.

Validation
Use any form validation library of your choice.

Bonus Features (Optional)
Student ID Validation

Validate the student ID format: 2620009

Format breakdown:

First 2 digits (26) → Admission year
Third digit (2) → Shift (1 = Morning, 2 = Day)
Last 4 digits (0009) → Student serial number
Student registration will only be for the current year. Show an appropriate validation error if the format is invalid.

User-Friendly Error Handling

Display validation errors near the corresponding input fields.
Optionally use toast notifications for form submission success/failure messages.
Table Features

Implement searching/filtering for table data.
Implement sorting on relevant columns.
Implement pagination for large datasets.
URL State Persistence

Persist table state in the URL, including:

Search query
Sort field and direction
Current page
Applied filters
Refreshing or sharing the URL should preserve the current table view.

Example URL
/students?page=2&search=ayesha&sort=name&order=asc
Seed Data
export const seedData = {
  students: [
    {
      id: "2620001",
      name: "Ayesha Rahman",
      email: "ayesha@uni.edu",
      program: "CSE",
      year: 2,
    },
    {
      id: "2620002",
      name: "Tanvir Hossain",
      email: "tanvir@uni.edu",
      program: "EEE",
      year: 3,
    },
    {
      id: "2620003",
      name: "Priya Das",
      email: "priya@uni.edu",
      program: "CSE",
      year: 1,
    },
  ],

  courses: [
    {
      id: "C101",
      name: "Data Structures",
      credits: 3,
      instructor: "Dr. Karim",
      seats: 30,
    },
    {
      id: "C102",
      name: "Web Engineering",
      credits: 3,
      instructor: "Dr. Noor",
      seats: 25,
    },
    {
      id: "C103",
      name: "Database Systems",
      credits: 4,
      instructor: "Dr. Islam",
      seats: 20,
    },
  ],

  enrollments: [
    {
      id: "E001",
      studentId: "2620001",
      courseId: "C101",
      date: "2026-01-15",
    },
    {
      id: "E002",
      studentId: "2620002",
      courseId: "C102",
      date: "2026-01-16",
    },
  ],

  grades: [
    {
      id: "G001",
      studentId: "2620001",
      courseId: "C101",
      grade: 88,
      semester: "Spring 2026",
    },
  ],
};
Freedom of Implementation
You are free to:

Choose any framework
Choose any UI library
Organize files however you want
Use any AI tools you normally use
Use any coding workflow you prefer
We are interested in observing your natural development process.


This is a web application I need to build 
here is a detailed overview of the charectarisitics 
now first break down the objective to me point by point 
and secondly I want to build it on free hosting of vercel 
and push codes to github 

based on my previous works
tell what's the best way to do it

I want claude to do the code works and how should I use cursor for the best output
suggest me the proper way to complete the task to it's best

* * *

**ChatGPT:**

Given your background, this is exactly the kind of project you should build with a modern stack rather than plain HTML/CSS/JS.

You already have:

* GitHub experience
* Vercel deployed projects
* VS Code installed
* Experience building the Dhaka Poshur Hat project
* Basic programming knowledge

So don't overcomplicate it.

# 1. What is the actual objective?

The evaluator wants to see:

### A. Can you build CRUD-style forms?

Three modules:

| Module | Add Data | Show Data |
| --- | --- | --- |
| Students | Student Registration Form | Students Table |
| Enrollments | Enrollment Form | Enrollment Table |
| Grades | Grade Submission Form | Grades Table |

---

### B. Can you validate data?

Examples:

* Email format valid?
* Student ID valid?
* Grade between 0–100?

---

### C. Can you store data?

Using:

```JavaScript
localStorage
```

No backend required.

---

### D. Can you display data nicely?

Tables with:

* Search
* Sort
* Pagination

---

### E. Can you organize a project properly?

Good folder structure.

Clean code.

Reusable components.

---

### F. Bonus

URL state:

```
/students?page=2&search=ayesha&sort=name&order=asc
```

This is the most advanced requirement.

Not mandatory but impressive.

---

# 2. What stack should YOU use?

For a 45-minute assessment:

### Use:

* Next.js
* TypeScript
* Tailwind CSS
* shadcn/ui
* React Hook Form
* Zod
* TanStack Table

This stack is extremely common today.

---

# 3. Architecture I would choose

```
src/

components/
    StudentForm.tsx
    EnrollmentForm.tsx
    GradeForm.tsx

    StudentTable.tsx
    EnrollmentTable.tsx
    GradeTable.tsx

lib/
    storage.ts
    seedData.ts
    validation.ts

app/
    page.tsx
```

Simple.

No need for complicated routing.

---

# 4. How localStorage should work

On first load:

```JavaScript
if (!localStorage.getItem("students")) {
    localStorage.setItem(
        "students",
        JSON.stringify(seedData.students)
    );
}
```

Same for:

```JavaScript
enrollments
grades
courses
```

After that:

```JavaScript
getStudents()
addStudent()
saveStudents()
```

Create utility functions.

---

# 5. Student ID Validation

The requirement says:

```
2620009
```

Meaning:

```
26 = admission year

2 = shift

0009 = serial
```

Current year = 2026

So:

```JavaScript
const currentYear = "26";
```

Validation:

```JavaScript
id.startsWith("26")
```

Shift:

```JavaScript
1 or 2
```

Regex:

```JavaScript
/^26[12]\d{4}$/
```

This will impress evaluators.

---

# 6. Grade Validation

Using Zod:

```TypeScript
grade: z.number()
.min(0)
.max(100)
```

Simple.

---

# 7. Tables

Use:

```Bash
npm install @tanstack/react-table
```

Features:

✅ Search

✅ Sort

✅ Pagination

This covers bonus requirements.

---

# 8. URL State Persistence

Example:

```
?page=2
&search=ayesha
&sort=name
```

Use:

```TypeScript
useSearchParams()
useRouter()
```

from Next.js.

Store:

```TypeScript
search
page
sort
order
```

inside URL.

This is a strong bonus feature.

---

# 9. What I would build in 45 minutes

### Priority 1 (Must Have)

Student Form

Enrollment Form

Grade Form

Local Storage

Tables

Validation

---

### Priority 2

Search

Sorting

---

### Priority 3

Pagination

---

### Priority 4

URL Persistence

---

# 10. How should you use Claude?

Don't ask Claude:

> Build this project.

Instead work in phases.

### Prompt 1

```
I am building a Student Management System assessment.

Stack:
Next.js 15
TypeScript
Tailwind
shadcn/ui
React Hook Form
Zod

Generate the complete folder structure and implementation plan.
Keep the architecture simple and assessment-friendly.
```

---

### Prompt 2

```
Generate seedData.ts using the following dataset.
```

Paste dataset.

---

### Prompt 3

```
Generate localStorage utility functions for:

students
courses
enrollments
grades

Requirements:
- Load seed data on first run
- TypeScript
- Reusable functions
```

---

### Prompt 4

```
Generate StudentForm component.

Requirements:
- React Hook Form
- Zod
- Student ID validation
- Tailwind styling
- Call onSubmit callback
```

Repeat for Enrollment and Grade.

---

### Prompt 5

```
Generate TanStack Table component with:
- search
- sorting
- pagination

TypeScript
```

---

### Prompt 6

```
Generate URL state persistence for search, sort and pagination using Next.js App Router.
```

---

# 11. How should you use Cursor?

Cursor should become your "project manager".

### Step 1

Create project

```Bash
npx create-next-app@latest
```

---

### Step 2

Open in Cursor

---

### Step 3

Create a file

```
PROJECT_SPEC.md
```

Paste the whole assignment.

---

### Step 4

Use Cursor Agent:

```
Read PROJECT_SPEC.md.

Build this project incrementally.

Phase 1:
Create data types.
Create seed data.
Create localStorage utilities.

Do not modify unrelated files.
```

---

### Step 5

After reviewing:

```
Phase 2:
Create forms using React Hook Form and Zod.
```

---

### Step 6

```
Phase 3:
Create tables using TanStack Table.
```

---

### Step 7

```
Phase 4:
Implement URL state persistence.
```

This produces much cleaner code than one giant prompt.

---

# 12. Deployment Strategy

GitHub:

```Bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <repo>
git push -u origin main
```

Then connect the repository to:

Vercel

Deployment becomes automatic.

Every push:

```Bash
git add .
git commit -m "feature: grade validation"
git push
```

Vercel redeploys.

---

For your current skill level as a BRAC CSE student who has already built and deployed a real Next.js project, the strongest submission would be:

**Next.js + TypeScript + Tailwind + React Hook Form + Zod + TanStack Table + localStorage + Vercel deployment + GitHub repository.**

That gives you a professional-looking solution while still being realistic to complete within the assessment time.

* * *

**You:**

Let's use claude together 
step by step 
I have provided the first prompt

* * *

**ChatGPT:**

Good approach. Don't ask Claude to generate the entire application at once. Treat Claude as a senior developer and yourself as the reviewer.

After you've sent:

> I am building a Student Management System assessment...  
> Generate the complete folder structure and implementation plan...

wait for Claude's response and **review the architecture first**.

### While Claude is generating the architecture, create the project

```Bash
npx create-next-app@latest student-management-system
```

Recommended choices:

```
✔ TypeScript → Yes
✔ ESLint → Yes
✔ Tailwind CSS → Yes
✔ src/ directory → Yes
✔ App Router → Yes
✔ Turbopack → Yes
✔ Import alias → Yes
```

Then:

```Bash
cd student-management-system
```

Install the required packages:

```Bash
npm install react-hook-form zod @hookform/resolvers
npm install @tanstack/react-table
npm install sonner
```

---

## What to ask Claude next

Once Claude returns the architecture, your second prompt should be:

```
Based on the architecture you proposed, generate:

1. TypeScript interfaces/types for:
   - Student
   - Course
   - Enrollment
   - Grade

2. seedData.ts using the exact dataset below.

3. localStorage utility functions that:
   - Initialize seed data on first load
   - Read data
   - Add records
   - Persist records

Requirements:
- TypeScript
- Clean reusable code
- Next.js App Router compatible
- Keep it assessment friendly
```

Then paste the provided seed dataset.

---

## Why this order?

Because the application is basically:

```
Data Models
    ↓
Seed Data
    ↓
Local Storage
    ↓
Forms
    ↓
Tables
    ↓
Search/Sort
    ↓
URL Persistence
```

If the foundation is correct, everything else becomes easy.

---

## Your workflow with Claude

For every response Claude gives:

1. Paste the generated code into Cursor.
2. Let Cursor create/update the files.
3. Read the code briefly.
4. Run:

```Bash
npm run dev
```

5. Confirm there are no TypeScript errors.
6. Only then move to the next prompt.

Never generate 2000 lines at once.

---

After Claude replies to Prompt #1, paste its response here and I'll help you review it before moving to Prompt #2.

* * *

**You:**

And it gives me a complete plan 

summary
"Key assessment checkpoints
The assessment likely looks for these specifically:

Zod + RHF integration — zodResolver in useForm, field-level error display
Server Actions — "use server" directive, revalidatePath, redirect
Dynamic routes — [id] segment, params.id prop
TypeScript strictness — no any, proper interface types, inferred Zod types
shadcn/ui usage — at least Button, Input, Table, Dialog (for delete confirm)
Search/filter — client-side SearchBar or URL-param based filtering on the list page

Start with the types and validation (Phase 2–3), then actions (Phase 4), then wire up the form (Phase 5) — the pages in Phase 6 are thin wrappers once those three layers exist."

* * *

**ChatGPT:**

That response suggests Claude may be assuming this is a **general Next.js assessment**, not the assignment you actually received.

A few red flags:

❌ **Server Actions**

* Your assignment never requires Server Actions.
* You're using **localStorage**.
* Server Actions run on the server and cannot access localStorage.

❌ **revalidatePath()**

* Only relevant when data is stored on the server.
* Not needed here.

❌ **redirect()**

* Not required.

❌ **Dynamic routes `[id]`**

* Not mentioned in the assignment.
* Could be added, but it's unnecessary for a 45-minute prototype.

---

### What the evaluator actually cares about

Priority order:

#### 1. localStorage persistence

Most important.

```
Load seed data
Add student
Add enrollment
Add grade
Persist after refresh
```

---

#### 2. Form validation

Student Form

Enrollment Form

Grade Form

Especially:

```
Grade: 0-100
```

and optionally:

```
Student ID: 26[1|2]xxxx
```

---

#### 3. Data tables

Display:

* Students
* Enrollments
* Grades

---

#### 4. Search

Bonus.

---

#### 5. Sorting

Bonus.

---

#### 6. Pagination

Bonus.

---

#### 7. URL state persistence

Advanced bonus.

---

### What I would tell Claude now

Use this prompt:

```
You are helping me complete a frontend assessment.

Important constraints:

- No backend
- No database
- No server actions
- All persistence must use browser localStorage
- Seed data must be loaded on first run
- Next.js App Router
- TypeScript
- Tailwind
- React Hook Form
- Zod
- TanStack Table

The assessment requirements are:

1. Student Registration
2. Course Enrollment
3. Grade Submission
4. localStorage persistence
5. Validation
6. Search, sorting and pagination
7. Optional URL state persistence

Generate the exact folder structure for this assessment only.

For each file explain:
- Purpose
- Responsibilities
- Dependencies

Keep the architecture simple enough to finish in 45 minutes.
```

---

### The architecture I expect Claude to arrive at

```
src/

app/
  page.tsx

components/
  students/
    StudentForm.tsx
    StudentTable.tsx

  enrollments/
    EnrollmentForm.tsx
    EnrollmentTable.tsx

  grades/
    GradeForm.tsx
    GradeTable.tsx

lib/
  seedData.ts
  storage.ts
  validations.ts

types/
  index.ts
```

That's enough.

Don't let Claude drag you into:

* Server Actions
* API routes
* Prisma
* Supabase
* Dynamic routes
* Authentication
* Backend architecture

Those are distractions for this specific assessment.

Now ask Claude for **Phase 2: Types + Seed Data + localStorage utilities**. That's the correct next step.

* * *

**You:**

Here's the complete plan.

---

## Folder structure

sms/
├── app/
│   ├── layout.tsx                  # Root layout, font, Toaster
│   ├── page.tsx                    # Redirect → /students
│   ├── not-found.tsx
│   ├── error.tsx
│   └── students/
│       ├── page.tsx                # List + search
│       ├── new/
│       │   └── page.tsx            # Create form
│       └── [id]/
│           ├── page.tsx            # Detail view
│           └── edit/
│               └── page.tsx        # Edit form
├── components/
│   ├── ui/                         # shadcn-generated (Button, Input, …)
│   ├── StudentTable.tsx
│   ├── StudentForm.tsx             # Shared create/edit form
│   ├── StudentCard.tsx
│   ├── DeleteDialog.tsx
│   └── SearchBar.tsx
├── lib/
│   ├── actions/
│   │   └── students.ts             # Server Actions (CRUD)
│   ├── data/
│   │   └── store.ts                # In-memory Map (swap for DB later)
│   └── validations/
│       └── student.ts              # Zod schema
├── types/
│   └── student.ts                  # Student interface
└── ... (next.config.ts, tailwind.config.ts, etc.)

---

## Implementation plan

### Phase 1 — Project setup

bash
npx create-next-app@latest sms --typescript --tailwind --app --src-dir=no
cd sms
npx shadcn@latest init
npx shadcn@latest add button input label select table dialog badge toast card
npm install react-hook-form @hookform/resolvers zod

### Phase 2 — Types and validation

**types/student.ts**
ts
export interface Student {
  id: string
  name: string
  email: string
  studentId: string
  department: string
  year: 1 | 2 | 3 | 4
  gpa: number
  createdAt: Date
}

**lib/validations/student.ts**
ts
import { z } from "zod"

export const studentSchema = z.object({
  name:       z.string().min(2, "Name must be at least 2 characters"),
  email:      z.string().email("Invalid email address"),
  studentId:  z.string().min(4, "Student ID required"),
  department: z.string().min(1, "Department required"),
  year:       z.coerce.number().int().min(1).max(4),
  gpa:        z.coerce.number().min(0).max(4.0),
})

export type StudentFormData = z.infer<typeof studentSchema>

### Phase 3 — Data store

**lib/data/store.ts** — a simple server-side Map that acts as your database during the assessment. Easy to swap for Prisma/Drizzle later.

ts
import { Student } from "@/types/student"

const students = new Map<string, Student>()

// seed a few rows so the list isn't empty on first load
const seed: Student[] = [
  { id: "1", name: "Rafi Islam",   email: "rafi@bracu.ac.bd",
    studentId: "22101001", department: "CSE", year: 3, gpa: 3.75, createdAt: new Date() },
  { id: "2", name: "Nusrat Jahan", email: "nusrat@bracu.ac.bd",
    studentId: "22101002", department: "EEE", year: 2, gpa: 3.50, createdAt: new Date() },
]
seed.forEach(s => students.set(s.id, s))

export { students }

### Phase 4 — Server Actions

**lib/actions/students.ts**
ts
"use server"
import { revalidatePath } from "next/cache"
import { redirect }       from "next/navigation"
import { students }       from "@/lib/data/store"
import { studentSchema }  from "@/lib/validations/student"
import { Student }        from "@/types/student"

export async function getStudents(query = "") {
  const all = Array.from(students.values())
  if (!query) return all
  const q = query.toLowerCase()
  return all.filter(s =>
    s.name.toLowerCase().includes(q) ||
    s.studentId.includes(q) ||
    s.department.toLowerCase().includes(q)
  )
}

export async function getStudent(id: string) {
  return students.get(id) ?? null
}

export async function createStudent(formData: unknown) {
  const parsed = studentSchema.safeParse(formData)
  if (!parsed.success) return { error: parsed.error.flatten() }
  const student: Student = {
    ...parsed.data, year: parsed.data.year as 1|2|3|4,
    id: crypto.randomUUID(), createdAt: new Date(),
  }
  students.set(student.id, student)
  revalidatePath("/students")
  redirect("/students")
}

export async function updateStudent(id: string, formData: unknown) {
  const existing = students.get(id)
  if (!existing) return { error: "Not found" }
  const parsed = studentSchema.safeParse(formData)
  if (!parsed.success) return { error: parsed.error.flatten() }
  students.set(id, { ...existing, ...parsed.data, year: parsed.data.year as 1|2|3|4 })
  revalidatePath("/students")
  redirect(`/students/${id}`)
}

export async function deleteStudent(id: string) {
  students.delete(id)
  revalidatePath("/students")
  redirect("/students")
}

### Phase 5 — Shared form component

This is the most important component — reused by both /students/new and /students/[id]/edit.

tsx
// components/StudentForm.tsx
"use client"
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { studentSchema, StudentFormData } from "@/lib/validations/student"
import { Student } from "@/types/student"
import { Button } from "@/components/ui/button"
import { Input }  from "@/components/ui/input"
import { Label }  from "@/components/ui/label"

interface Props {
  defaultValues?: Partial<Student>
  onSubmit: (data: StudentFormData) => Promise<void>
  submitLabel?: string
}

export default function StudentForm({ defaultValues, onSubmit, submitLabel = "Save" }: Props) {
  const { register, handleSubmit, formState: { errors, isSubmitting } } = useForm<StudentFormData>({
    resolver: zodResolver(studentSchema),
    defaultValues,
  })

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4 max-w-lg">
      <div>
        <Label htmlFor="name">Full name</Label>
        <Input id="name" {...register("name")} />
        {errors.name && <p className="text-destructive text-sm mt-1">{errors.name.message}</p>}
      </div>
      <div>
        <Label htmlFor="email">Email</Label>
        <Input id="email" type="email" {...register("email")} />
        {errors.email && <p className="text-destructive text-sm mt-1">{errors.email.message}</p>}
      </div>
      {/* studentId, department, year, gpa — same pattern */}
      <Button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Saving…" : submitLabel}
      </Button>
    </form>
  )
}

### Phase 6 — Pages

| Route | File | What it renders |
|---|---|---|
| /students | app/students/page.tsx | <SearchBar> + <StudentTable> (server component, calls getStudents) |
| /students/new | app/students/new/page.tsx | <StudentForm> wired to createStudent |
| /students/[id] | app/students/[id]/page.tsx | Read-only detail card + Edit/Delete buttons |
| /students/[id]/edit | app/students/[id]/edit/page.tsx | <StudentForm> pre-filled, wired to updateStudent |

---

## Key assessment checkpoints

The assessment likely looks for these specifically:

- **Zod + RHF integration** — zodResolver in useForm, field-level error display
- **Server Actions** — "use server" directive, revalidatePath, redirect
- **Dynamic routes** — [id] segment, params.id prop
- **TypeScript strictness** — no any, proper interface types, inferred Zod types
- **shadcn/ui usage** — at least Button, Input, Table, Dialog (for delete confirm)
- **Search/filter** — client-side SearchBar or URL-param based filtering on the list page

Start with the types and validation (Phase 2–3), then actions (Phase 4), then wire up the form (Phase 5) — the pages in Phase 6 are thin wrappers once those three layers exist.

* * *

**ChatGPT:**

This plan is technically good, but it's solving the **wrong problem**.

Look carefully at your assignment:

### Assignment says

✅ Student Registration

✅ Course Enrollment

✅ Grade Submission

✅ localStorage

✅ Seed Data

✅ Search

✅ Sorting

✅ Pagination

✅ URL Persistence (bonus)

---

### Claude's plan builds

❌ Student CRUD application

❌ Server Actions

❌ Dynamic Routes

❌ Student Detail Page

❌ Edit Student Page

❌ Delete Student Dialog

❌ Server-side Map Database

---

The biggest issue:

```TypeScript
const students = new Map<string, Student>()
```

This directly violates:

> Use browser localStorage for persistence.

If you deploy this to Vercel:

* Memory resets
* Data disappears
* Doesn't meet requirements

---

# What I would do

Forget everything after Phase 2.

Keep:

✅ Next.js

✅ TypeScript

✅ Tailwind

✅ Zod

✅ React Hook Form

---

Discard:

❌ Server Actions

❌ Dynamic Routes

❌ Student Details Page

❌ Edit Page

❌ Delete Dialog

❌ Server-side Store

---

# The application should actually look like this

```
-------------------------------------------------
Student Management System
-------------------------------------------------

[ Students Tab ]

Student Registration Form

Student Table
(Search)
(Sort)
(Pagination)

-------------------------------------------------

[ Enrollments Tab ]

Enrollment Form

Enrollment Table

-------------------------------------------------

[ Grades Tab ]

Grade Form

Grade Table

-------------------------------------------------
```

Single-page dashboard style.

Much faster.

Much closer to requirements.

---

# What Claude should generate next

Ask Claude:

The previous architecture is too focused on CRUD pages and Server Actions.

Please redesign the project according to these constraints:

* Browser localStorage only
* No backend
* No Server Actions
* No API routes
* No database
* Single-page dashboard layout
* Student Registration section
* Enrollment section
* Grade Submission section
* Seed data loaded on first run
* React Hook Form + Zod validation
* TanStack Table for search, sorting and pagination

Generate:

1. TypeScript interfaces
2. seedData.ts
3. storage.ts utility for localStorage
4. Recommended folder structure

Do not generate dynamic routes, detail pages, edit pages, delete pages or server-side storage.

---

If I were doing this assessment in 45 minutes, my build order would be:

### Phase 1

Project setup

### Phase 2

```
types/
seedData.ts
storage.ts
```

### Phase 3

```
StudentForm
EnrollmentForm
GradeForm
```

### Phase 4

```
StudentsTable
EnrollmentsTable
GradesTable
```

### Phase 5

Search + Sort

### Phase 6

Pagination

### Phase 7

URL state persistence (if time remains)

That order gives you a working submission even if time runs out halfway through.

* * *

**You:**

one of my accounts is out of limit
opened another claude account 
give me a detailed prompt so that it can build efficiently and correctly on the first apporach

* * *

**ChatGPT:**

Since you're starting fresh with a new Claude chat, don't ask it for code immediately.

First, make Claude understand the assignment, constraints, tech stack, and implementation strategy. That will significantly improve the quality of all future code generation.

Use this as your first prompt:

You are acting as a senior frontend engineer helping me complete a timed frontend assessment.

Read everything carefully before proposing a solution.

# Assessment Context

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

---

# Functional Requirements

## 1. Student Registration

Create a form that allows adding students.

Fields:

* Student ID
* Name
* Email
* Program
* Academic Year

Display all students in a table.

---

## 2. Course Enrollment

Create a form that allows enrolling students.

Fields:

* Enrollment ID
* Student ID
* Course ID
* Enrollment Date

Display all enrollments in a table.

---

## 3. Grade Submission

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

---

# Data Persistence

Use browser localStorage.

The application must:

* Load seed data on first run.
* Persist newly created records.
* Restore data after refresh.

---

# Bonus Features

## Student ID Validation

Format:

2620009

Meaning:

* First 2 digits = admission year
* Third digit = shift (1 = Morning, 2 = Day)
* Last 4 digits = student serial

Registration is only allowed for the current year.

Show validation errors if invalid.

---

## Table Features

Implement:

* Search
* Sorting
* Pagination

---

## URL State Persistence (Optional)

Persist:

* Search query
* Sort field
* Sort direction
* Current page

Example:

/students?page=2&search=ayesha&sort=name&order=asc

---

# Seed Data

(Insert the full seed dataset here)

---

# Important Constraints

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

---

# What I Want From You

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

---

After Claude gives the architecture, your second prompt should be:

> Generate Phase 1 only:
> 
> * TypeScript interfaces
> * seedData.ts
> * localStorage utility functions
> 
> Do not generate forms or UI yet.

This phased approach usually produces much cleaner results than asking Claude to build everything at once.

* * *

**You:**

## Student Management System — Architecture & Planning Document

---

### 1. Recommended Folder Structure

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

**Key rule:** Every file that touches window, hooks, or forms gets "use client". The app/*/page.tsx files stay as thin server shells — they just import and render the corresponding *Page.tsx client component. This is the cleanest App Router pattern.

---

### 2. High-Level Architecture

app/students/page.tsx  (Server Component — zero JS)
        │
        └── <StudentsPage />  ← "use client"
                │
                ├── useStudents()           ← reads/writes localStorage
                │       └── useState([])   ← source of truth in memory
                │
                ├── <StudentForm />         ← RHF + Zod, calls hook.add()
                └── <StudentsTable />       ← TanStack Table, URL state sync

All three modules (Students, Enrollments, Grades) follow **identical structure**. This repetition is intentional — it's fast to build and easy to reason about under time pressure.

---

### 3. State Management Approach

**No global state manager.** Use three independent custom hooks, one per domain:

useStudents()     →  { students, addStudent }
useEnrollments()  →  { enrollments, addEnrollment }
useGrades()       →  { grades, addGrade }

Each hook:
- Initializes from localStorage on mount (inside useEffect — never on render)
- Holds data in useState
- On every mutation, updates both useState and localStorage atomically

This is intentionally simple. No Context, no Zustand, no Jotai. The scope doesn't justify it and it costs build time.

---

### 4. localStorage Strategy

**Three keys, one responsibility each:**

| Key | Value |
|---|---|
| sms_students | Student[] |
| sms_enrollments | Enrollment[] |
| sms_grades | Grade[] |

**Bootstrap logic** lives in lib/seed.ts:

On first run:
  → localStorage key missing or null
  → write seed data to all three keys
  → hook reads and returns that data

On subsequent runs:
  → key exists
  → parse and return existing data (seed is never re-applied)

**SSR guard** — wrap all localStorage access:

ts
// lib/storage.ts
const isBrowser = typeof window !== 'undefined'

All reads/writes go through typed helpers in storage.ts. Never access localStorage directly in components.

**⚠️ Seed data note:** You wrote (Insert the full seed dataset here) — make sure you have at least 15–20 students, 10+ enrollments, and 10+ grades for pagination to be demonstrable. I'll generate realistic BRAC-style seed data when we enter the build phase.

---

### 5. Validation Strategy

All schemas in a single file: lib/schemas.ts

**Student ID bonus validation** (current year = 2026, so prefix = 26):

Format:  [YY][S][SSSS]
         26   1  0009   → Morning shift, serial 0009

Regex:   ^26[12]\d{4}$

Rules enforced via Zod .refine():
  - Must be exactly 7 digits
  - First 2 digits must equal current year's last 2 ("26")
  - 3rd digit must be 1 (Morning) or 2 (Day)
  - Remaining 4 digits are the serial
  - Show shift label in success/error message

**Grade validation:**

z.number().min(0).max(100)

**Duplicate ID check** — handled at the hook level (not Zod), since Zod has no access to existing records. The addStudent function checks students.some(s => s.studentId === newId) before appending.

**Form wiring:** zodResolver from @hookform/resolvers/zod on every form. No manual register parsing — use the resolver, extract formState.errors, done.

---

### 6. Table Strategy

Use **TanStack Table v8** (@tanstack/react-table).

Build one **generic <DataTable />** component that accepts:
- columns definition
- data array
- searchKey (column to filter on)

This handles all three tables without repetition.

**Features per table:**

| Feature | Implementation |
|---|---|
| Search | globalFilterFn or column filter on useReactTable |
| Sorting | getSortedRowModel() — click column headers |
| Pagination | getPaginationRowModel() — page size 10 |
| URL state (bonus) | useSearchParams + useRouter synced to table state |

**URL state gotcha (critical):** useSearchParams() in App Router **requires a <Suspense> boundary**. Wrap the table component:

tsx
// in StudentsPage.tsx
<Suspense fallback={<TableSkeleton />}>
  <StudentsTable ... />
</Suspense>

Forgetting this causes a build-breaking error that's hard to debug under time pressure.

---

### 7. Build Order (45-Minute Assessment)

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

---

### 8. Risk Areas & Common Mistakes

**🔴 High Risk**

| Risk | Mitigation |
|---|---|
| useSearchParams without <Suspense> → build fails | Always wrap in Suspense from the start |
| localStorage read during SSR → window is not defined | All reads inside useEffect, never on render |
| TanStack Table: forgetting getCoreRowModel() → silent empty table | It's required; add it first |
| RHF register vs Controller confusion for selects | Use Controller for all <select> elements |
| Seed re-applying on every refresh → duplicates | Gate seed on key absence, not data length |

**🟡 Medium Risk**

| Risk | Mitigation |
|---|---|
| zodResolver type mismatch (string vs number for grade) | Use z.coerce.number() for numeric inputs |
| Pagination breaking when search reduces total rows | Reset to page 0 on filter change |
| Student ID year hardcoded vs dynamic | Use new Date().getFullYear() % 100 |
| app/page.tsx showing blank | Add redirect('/students') immediately |

**🟢 Time-waster traps to avoid**

- Don't build edit/delete — the spec forbids it, don't spend a second on it
- Don't over-engineer storage — no class, no event emitter, just typed functions
- Don't build a shared form component across all three — forms differ enough to hurt you
- Don't start with styling — functionality first, Tailwind classes last

---

### 9. Implementation Phases

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

---

**Ready to proceed?** Once you confirm this plan, I'll generate all the code in a clean build order starting with the data layer. If you have the seed data to include, paste it now and I'll integrate it into lib/seed.ts directly.

* * *

**ChatGPT:**

This plan is **much better**. If I were reviewing a candidate's submission, I'd be happy with this architecture.

A few comments before you start coding:

### Keep

✅ Next.js App Router

✅ TypeScript

✅ RHF + Zod

✅ TanStack Table

✅ localStorage abstraction

✅ Custom hooks

✅ Thin page components

✅ Search / Sort / Pagination

---

### Simplify slightly

For a 45-minute assessment, I would not create:

```
components/ui/InputField.tsx
components/ui/SelectField.tsx
```

at the beginning.

Start with regular inputs.

If everything works and you still have time, then refactor.

---

### Biggest correction

Claude wrote:

> make sure you have at least 15–20 students, 10+ enrollments, and 10+ grades

Do NOT invent new seed data.

The assignment explicitly provides seed data.

Use exactly the seed data given unless instructed otherwise.

You can always create additional records through the forms during the demo.

---

### One more improvement

Instead of:

```
/Students
/Enrollments
/Grades
```

I would consider a single dashboard page:

```
app/page.tsx
```

with:

```
Students Section
Enrollment Section
Grades Section
```

because the assignment never asks for multiple routes.

However, if you're comfortable with Next.js routing, the three-page approach is perfectly fine and looks more professional.

---

## What you should ask Claude next

Now move to implementation.

Use this exact prompt:

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

---

After Claude generates Phase 1, don't immediately ask for everything else.

Paste the Phase 1 code into Cursor, run:

```Bash
npm run dev
```

and verify:

* No TypeScript errors
* No import errors
* No lint errors

Only then move to:

**Phase 2 → Custom Hooks**

Then:

**Phase 3 → Student Module**

Then:

**Phase 4 → Enrollment Module**

Then:

**Phase 5 → Grade Module**

This incremental workflow is usually how experienced developers complete timed assessments with AI assistance while avoiding debugging huge generated codebases.

* * *

**You:**

The user wants me to generate Phase 1 code. Let me first check the frontend-design skill since this is a frontend project, but actually for Phase 1 it's just data layer files - no UI. So I don't need the skill for this phase.
Let me think about what I need to generate:

types/index.ts - TypeScript interfaces for Student, Enrollment, Grade
lib/seed.ts - Seed data + bootstrap logic
lib/storage.ts - localStorage utilities with all required functions
lib/schemas.ts - Zod schemas

The user mentioned "Use the exact seed dataset provided in the assessment" but in the original message they wrote "(Insert the full seed dataset here)" as a placeholder. So they didn't actually provide the seed data. I need to generate realistic seed data for a BRAC University context since they're based in Dhaka, Bangladesh working on a Dhaka-related app.
Wait, let me re-read: "Use the exact seed dataset provided in the assessment" - but no seed data was actually provided in the conversation. I should note this and generate realistic seed data based on the BRAC University context (since I know Mahir is a BRAC University student).
For Student ID format: 2620009

First 2 digits = admission year (26 = 2026)
Third digit = shift (1 = Morning, 2 = Day)
Last 4 digits = student serial

Since registration is only allowed for the current year (2026), seed data should use current year students. But wait - seed data represents existing students who might have enrolled in previous years too. Let me think... the constraint says "Registration is only allowed for the current year" meaning the FORM validation only allows adding new students with current year IDs. But seed data can have students from previous years.
Actually, let me reconsider. The seed data probably has students from the current year based on the assessment. Since no seed data was provided, I'll generate meaningful seed data.
Let me think about the programs for BRAC University:

…Show lessCreate directory structure for Phase 1 filesScriptDone


claude again ran out of limits
now tell me how do I continue wo claude?

* * *

**ChatGPT:**

At this point, you honestly don't need Claude anymore.

The architecture is already decided. Now the job is execution.

Also, Claude made one important mistake:

> "No seed data was provided"

But your assessment **did provide seed data**. The exact dataset with:

* Ayesha Rahman
* Tanvir Hossain
* Priya Das
* C101
* C102
* C103
* E001
* E002
* G001

So don't generate new BRAC-style data.

---

## Best approach now

Use **Cursor Agent** instead of Claude.

Create a file:

```
PROJECT_SPEC.md
```

Paste:

* The assessment requirements
* The seed dataset
* The architecture plan

Then tell Cursor:

Read PROJECT_SPEC.md completely.

We are building a frontend-only Student Management System.

Requirements:

* Next.js App Router
* TypeScript
* Tailwind
* React Hook Form
* Zod
* TanStack Table
* localStorage persistence

Important:

* Use the exact seed data from PROJECT_SPEC.md
* No backend
* No Server Actions
* No API routes
* No database

Phase 1 only:

Create:

* src/types/index.ts
* src/lib/seed.ts
* src/lib/storage.ts
* src/lib/schemas.ts

Generate complete production-ready code.

Do not create any forms or UI yet.

---

## Then review Phase 1

You should see:

```
src/
 ├─ types/
 ├─ lib/
 │   ├─ seed.ts
 │   ├─ storage.ts
 │   └─ schemas.ts
```

Run:

```Bash
npm run dev
```

If there are no errors:

```Bash
git add .
git commit -m "Phase 1 data layer"
```

---

## Then ask Cursor for Phase 2

Phase 2.

Create:

* src/lib/hooks/useStudents.ts
* src/lib/hooks/useEnrollments.ts
* src/lib/hooks/useGrades.ts

Requirements:

* Read from localStorage
* Use useState
* Use useEffect
* Support add operations
* Keep state synchronized with localStorage

Do not create UI components yet.

---

## Then Phase 3

Students module first.

Phase 3.

Create:

* StudentForm.tsx
* StudentsTable.tsx
* StudentsPage.tsx

Requirements:

* React Hook Form
* Zod validation
* Student ID validation
* TanStack Table
* Search
* Sorting
* Pagination

Use the existing hooks and storage utilities.

---

## Then Phase 4

Enrollments.

---

## Then Phase 5

Grades.

---

## Then Phase 6

URL persistence.

---

### What I would do if I were you

Because you already built and deployed the Dhaka Poshur Hat project, I would:

1. Create the Next.js project.
2. Put the assignment + seed data into `PROJECT_SPEC.md`.
3. Let Cursor generate one phase at a time.
4. After each phase:

```Bash
npm run dev
```

5. Commit after every working phase.

```Bash
git commit -m "phase 1 complete"
git commit -m "students module complete"
git commit -m "enrollments module complete"
```

That way, even if something breaks later, you can revert instantly.

You're past the planning stage now. The bottleneck is no longer Claude—it's building and testing incrementally.

* * *

**You:**

can't I use cursor for free?

* * *

**ChatGPT:**

Yes, you can use Cursor for free, but there are some limits.

### Cursor Free Plan

You get:

✅ VS Code-like editor

✅ AI chat

✅ AI code generation

✅ Codebase understanding

✅ Tab autocomplete

✅ Can generate entire files and components

---

### Limitations

❌ Limited number of premium model requests (Claude, GPT models)

❌ Heavy Agent usage can run out quickly

❌ Slower models may be used after limits are reached

The exact limits can change over time.

---

### For your assessment

You can absolutely complete this project on the free plan if you use it efficiently.

Instead of:

> Build the entire Student Management System

use smaller prompts like:

```
Create src/types/index.ts
based on the following requirements...
```

Then:

```
Create src/lib/storage.ts
using localStorage and the seed data.
```

Then:

```
Create StudentForm.tsx
using React Hook Form and Zod.
```

Smaller prompts:

* Use fewer AI credits
* Produce cleaner code
* Are easier to debug

---

### Alternative if Cursor limits are exhausted

You already have:

* ChatGPT
* VS Code
* GitHub
* Vercel

You can continue here.

I can generate:

* `types/index.ts`
* `seed.ts`
* `storage.ts`
* `schemas.ts`
* hooks
* forms
* tables

step by step exactly like Claude/Cursor would.

For this particular project, you do **not** need a paid Cursor subscription to finish it. A combination of ChatGPT + VS Code is more than enough. In fact, for a timed assessment, generating and testing one module at a time is often more reliable than letting an AI agent create the whole codebase in one shot.

* * *

**You:**

tell me how do I step by step

* * *

**ChatGPT:**

Based on your experience level and the fact that you've already built and deployed a Next.js project before, here's the workflow I'd follow.

# Step 1: Create the project

Open terminal:

```Bash
npx create-next-app@latest student-management-system
```

Choose:

```
✔ TypeScript → Yes
✔ ESLint → Yes
✔ Tailwind → Yes
✔ src directory → Yes
✔ App Router → Yes
✔ Turbopack → Yes
```

Then:

```Bash
cd student-management-system
```

---

# Step 2: Install required packages

```Bash
npm install react-hook-form
npm install zod
npm install @hookform/resolvers
npm install @tanstack/react-table
```

Start project:

```Bash
npm run dev
```

Open:

```
http://localhost:3000
```

---

# Step 3: Create GitHub repository immediately

```Bash
git init
git add .
git commit -m "Initial setup"
```

Create repo on GitHub.

Then:

```Bash
git remote add origin YOUR_REPO_URL
git push -u origin main
```

---

# Step 4: Connect to Vercel

Create a new project on:

[Vercel](https://vercel.com?utm_source=chatgpt.com)

Import GitHub repository.

Now every push will auto-deploy.

---

# Step 5: Create PROJECT_SPEC.md

In project root:

```
PROJECT_SPEC.md
```

Paste:

* Assignment requirements
* Seed data
* Architecture plan

This becomes your reference document.

---

# Step 6: Build Phase 1 (Data Layer)

Create:

```
src/types/index.ts

src/lib/
    seed.ts
    storage.ts
    schemas.ts
```

### types/index.ts

Contains:

```TypeScript
Student
Enrollment
Grade
Course
```

interfaces.

---

### seed.ts

Paste the exact seed dataset from the assignment.

---

### storage.ts

Functions:

```TypeScript
initializeStorage()

getStudents()
getEnrollments()
getGrades()

addStudent()
addEnrollment()
addGrade()
```

using localStorage.

---

### schemas.ts

Zod validation.

Student:

```TypeScript
studentId validation
```

Enrollment:

```TypeScript
required fields
```

Grade:

```TypeScript
0-100
```

---

# Step 7: Test

Run:

```Bash
npm run dev
```

Make sure:

✅ No red errors

✅ No TypeScript errors

Then:

```Bash
git add .
git commit -m "Phase 1 complete"
git push
```

---

# Step 8: Build Hooks

Create:

```
src/lib/hooks/

useStudents.ts
useEnrollments.ts
useGrades.ts
```

Each hook:

```TypeScript
useState()
useEffect()

load data
add data
sync localStorage
```

---

# Step 9: Test Again

```Bash
npm run dev
```

If clean:

```Bash
git commit -m "Hooks complete"
git push
```

---

# Step 10: Students Module

Create:

```
components/students/

StudentForm.tsx
StudentsTable.tsx
StudentsPage.tsx
```

StudentForm:

* React Hook Form
* Zod

StudentsTable:

* TanStack Table

Features:

```
Search
Sort
Pagination
```

StudentsPage:

```
Form on top
Table below
```

---

# Step 11: Test Again

Try:

```
Add student
Refresh page
Data remains
```

If yes:

```Bash
git commit -m "Students module complete"
git push
```

---

# Step 12: Enrollment Module

Repeat same pattern:

```
EnrollmentForm
EnrollmentsTable
EnrollmentsPage
```

Test.

Commit.

Push.

---

# Step 13: Grade Module

Create:

```
GradeForm
GradesTable
GradesPage
```

Validation:

```TypeScript
grade >= 0
grade <= 100
```

Test.

Commit.

Push.

---

# Step 14: Navigation

Create:

```
Students
Enrollments
Grades
```

links in navbar.

Pages:

```
/students
/enrollments
/grades
```

---

# Step 15: Bonus Features

Only after everything works.

### Search

Already in tables.

### Sorting

Already in tables.

### Pagination

Already in tables.

### URL State

Example:

```
/students?page=2&search=ayesha
```

Only do this if you have time.

---

# Your Goal

Don't think:

> Build Student Management System

Think:

```
Phase 1 → Data Layer
Phase 2 → Hooks
Phase 3 → Students
Phase 4 → Enrollments
Phase 5 → Grades
Phase 6 → Navigation
Phase 7 → Bonus
```

Complete one phase, run the app, commit to GitHub, then move on. This is exactly how I'd tackle a 45-minute assessment to maximize the chance of having a working submission at every stage.

* * *

**You:**

well I can't understand how to do these complex tasks
tell me how can I easily complete the task with less complex approach

* * *

**ChatGPT:**

Given your background as a first-year CSE student who is still getting comfortable with development, I would **not** build the architecture we discussed earlier.

That architecture is what an experienced React developer might build. Your goal is to **submit a working project**, not impress someone with complexity.

# Simplest Possible Solution

Build **one page only**.

```
Student Management System

--------------------------------
Student Registration Form
Student Table

--------------------------------
Enrollment Form
Enrollment Table

--------------------------------
Grade Form
Grade Table
```

No multiple routes.

No custom hooks.

No generic table components.

No URL persistence initially.

No fancy architecture.

---

# Use AI Like This

Open ChatGPT (or another Claude account later).

Ask:

I am a beginner.

Build this project in the simplest possible way.

Requirements:

* Next.js
* TypeScript
* Tailwind
* localStorage

Do NOT use:

* custom hooks
* generic components
* advanced architecture
* server actions
* multiple pages

Create everything inside app/page.tsx first.

The goal is to get a working project quickly.

---

# Then Build in 4 Steps

### Step 1

Ask AI:

```
Create the Student Form only.
```

Get it working.

---

### Step 2

Ask:

```
Add Student Table below the form.
Store students in localStorage.
```

Get it working.

---

### Step 3

Ask:

```
Add Enrollment Form and Enrollment Table.
```

Get it working.

---

### Step 4

Ask:

```
Add Grade Form and Grade Table.
Grade must be between 0 and 100.
```

Get it working.

---

# Folder Structure

Keep it tiny:

```
src/

app/
   page.tsx

lib/
   seedData.ts
```

That's enough.

---

# What Evaluators Care About

If your project can:

✅ Add students

✅ Add enrollments

✅ Add grades

✅ Validate grade

✅ Save to localStorage

✅ Load seed data

✅ Display tables

then you've already completed the core requirements.

---

# Ignore These Initially

Don't spend time on:

❌ URL persistence

❌ Dynamic routes

❌ Reusable hooks

❌ Generic components

❌ Perfect folder structure

❌ Advanced patterns

Those are bonus-level concerns.

---

If I were in your position and had to finish this assignment quickly, I would literally build **everything inside `app/page.tsx` first**, make sure it works, deploy it to GitHub and Vercel, and only then improve the structure if time remained. That's the fastest path to a working submission.