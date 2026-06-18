# Student Management System architecture setup

**Created:** 6/16/2026 21:20:22  
**Updated:** 6/16/2026 21:20:22  
**Exported:** 6/18/2026 14:51:33  
**Link:** [https://claude.ai/share/8da5b4c0-9042-409f-a3f0-0222049e48e0](https://claude.ai/share/8da5b4c0-9042-409f-a3f0-0222049e48e0)  

## User:

> 6/16/2026 20:40:49

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

## Assistant:

> 6/16/2026 20:41:53

> [Interactive Widget: sms_architecture_diagram]

Here's the complete plan.

---

## Folder structure

```
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
```

---

## Implementation plan

### Phase 1 — Project setup

```bash
npx create-next-app@latest sms --typescript --tailwind --app --src-dir=no
cd sms
npx shadcn@latest init
npx shadcn@latest add button input label select table dialog badge toast card
npm install react-hook-form @hookform/resolvers zod
```

### Phase 2 — Types and validation

**`types/student.ts`**
```ts
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
```

**`lib/validations/student.ts`**
```ts
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
```

### Phase 3 — Data store

**`lib/data/store.ts`** — a simple server-side `Map` that acts as your database during the assessment. Easy to swap for Prisma/Drizzle later.

```ts
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
```

### Phase 4 — Server Actions

**`lib/actions/students.ts`**
```ts
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
```

### Phase 5 — Shared form component

This is the most important component — reused by both `/students/new` and `/students/[id]/edit`.

```tsx
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
```

### Phase 6 — Pages

| Route | File | What it renders |
|---|---|---|
| `/students` | `app/students/page.tsx` | `<SearchBar>` + `<StudentTable>` (server component, calls `getStudents`) |
| `/students/new` | `app/students/new/page.tsx` | `<StudentForm>` wired to `createStudent` |
| `/students/[id]` | `app/students/[id]/page.tsx` | Read-only detail card + Edit/Delete buttons |
| `/students/[id]/edit` | `app/students/[id]/edit/page.tsx` | `<StudentForm>` pre-filled, wired to `updateStudent` |

---

## Key assessment checkpoints

The assessment likely looks for these specifically:

- **Zod + RHF integration** — `zodResolver` in `useForm`, field-level error display
- **Server Actions** — `"use server"` directive, `revalidatePath`, `redirect`
- **Dynamic routes** — `[id]` segment, `params.id` prop
- **TypeScript strictness** — no `any`, proper interface types, inferred Zod types
- **shadcn/ui usage** — at least `Button`, `Input`, `Table`, `Dialog` (for delete confirm)
- **Search/filter** — client-side `SearchBar` or URL-param based filtering on the list page

Start with the types and validation (Phase 2–3), then actions (Phase 4), then wire up the form (Phase 5) — the pages in Phase 6 are thin wrappers once those three layers exist.

## User:

> 6/16/2026 20:44:17

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

## Assistant:

> 6/16/2026 20:44:18

Powered by Claude Exporter (https://www.ai-chat-exporter.net)
