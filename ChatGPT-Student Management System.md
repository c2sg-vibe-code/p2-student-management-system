**You:**

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

* * *

**ChatGPT:**

For a **45-minute assessment**, I would deliberately keep it extremely simple:

### Tech Stack

* Next.js App Router
* TypeScript
* Tailwind
* localStorage
* React Hook Form + Zod (simple validation)

### Structure

```
app/
 └── page.tsx

package.json
tailwind.config.ts
```

No custom hooks.

No reusable generic components.

No context.

No server actions.

No multiple pages.

Everything in `app/page.tsx`.

---

# Development Plan

## Step 1: Define Types

At the top of `page.tsx`

```TypeScript
type Student = {
  id: string;
  name: string;
  email: string;
  program: string;
  year: number;
};

type Enrollment = {
  id: string;
  studentId: string;
  courseId: string;
  date: string;
};

type GradeRecord = {
  id: string;
  studentId: string;
  courseId: string;
  grade: number;
  semester: string;
};

type Course = {
  id: string;
  name: string;
  credits: number;
  instructor: string;
  seats: number;
};
```

---

# Step 2: Seed Data

```TypeScript
const seedData = {
  students: [...],
  courses: [...],
  enrollments: [...],
  grades: [...]
};
```

Copy exactly from the task.

---

# Step 3: State

```TypeScript
const [students, setStudents] = useState<Student[]>([]);
const [enrollments, setEnrollments] = useState<Enrollment[]>([]);
const [grades, setGrades] = useState<GradeRecord[]>([]);
const [courses] = useState<Course[]>(seedData.courses);
```

---

# Step 4: Load Seed Data Once

```TypeScript
useEffect(() => {
  const studentsData = localStorage.getItem("students");
  const enrollmentsData = localStorage.getItem("enrollments");
  const gradesData = localStorage.getItem("grades");

  if (!studentsData) {
    localStorage.setItem(
      "students",
      JSON.stringify(seedData.students)
    );
  }

  if (!enrollmentsData) {
    localStorage.setItem(
      "enrollments",
      JSON.stringify(seedData.enrollments)
    );
  }

  if (!gradesData) {
    localStorage.setItem(
      "grades",
      JSON.stringify(seedData.grades)
    );
  }

  setStudents(
    JSON.parse(
      localStorage.getItem("students") || "[]"
    )
  );

  setEnrollments(
    JSON.parse(
      localStorage.getItem("enrollments") || "[]"
    )
  );

  setGrades(
    JSON.parse(
      localStorage.getItem("grades") || "[]"
    )
  );
}, []);
```

---

# Step 5: Validation

Install:

```Bash
npm install react-hook-form zod @hookform/resolvers
```

---

## Student Schema

```TypeScript
const studentSchema = z.object({
  id: z
    .string()
    .regex(
      /^26[12]\d{4}$/,
      "Format should be like 2620009"
    ),

  name: z.string().min(2),

  email: z.email(),

  program: z.string().min(1),

  year: z.coerce.number().min(1).max(4),
});
```

---

## Grade Schema

```TypeScript
const gradeSchema = z.object({
  id: z.string().min(1),
  studentId: z.string().min(1),
  courseId: z.string().min(1),

  grade: z.coerce
    .number()
    .min(0)
    .max(100),

  semester: z.string().min(1),
});
```

---

## Enrollment Schema

```TypeScript
const enrollmentSchema = z.object({
  id: z.string().min(1),
  studentId: z.string().min(1),
  courseId: z.string().min(1),
  date: z.string().min(1),
});
```

---

# Step 6: Student Form

```TypeScript
const studentForm = useForm({
  resolver: zodResolver(studentSchema),
});
```

Submit:

```TypeScript
const addStudent = (data: Student) => {
  const updated = [...students, data];

  setStudents(updated);

  localStorage.setItem(
    "students",
    JSON.stringify(updated)
  );

  studentForm.reset();
};
```

---

# Step 7: Enrollment Form

```TypeScript
const addEnrollment = (data: Enrollment) => {
  const updated = [...enrollments, data];

  setEnrollments(updated);

  localStorage.setItem(
    "enrollments",
    JSON.stringify(updated)
  );

  enrollmentForm.reset();
};
```

---

# Step 8: Grade Form

```TypeScript
const addGrade = (data: GradeRecord) => {
  const updated = [...grades, data];

  setGrades(updated);

  localStorage.setItem(
    "grades",
    JSON.stringify(updated)
  );

  gradeForm.reset();
};
```

---

# Step 9: Layout

Single page.

```
----------------------------------
Student Registration
[Form]

Students Table

----------------------------------
Course Enrollment
[Form]

Enrollments Table

----------------------------------
Grade Submission
[Form]

Grades Table
----------------------------------
```

Use:

```TypeScript
<div className="max-w-7xl mx-auto p-8 space-y-12">
```

---

# Student Table

```TypeScript
<table className="w-full border">
```

Columns:

```
ID
Name
Email
Program
Year
```

Render:

```TypeScript
<tbody>
  {students.map((student) => (
    <tr key={student.id}>
      <td>{student.id}</td>
      <td>{student.name}</td>
      <td>{student.email}</td>
      <td>{student.program}</td>
      <td>{student.year}</td>
    </tr>
  ))}
</tbody>
```

---

# Enrollment Table

Columns:

```
Enrollment ID
Student ID
Course ID
Date
```

---

# Grades Table

Columns:

```
Grade ID
Student ID
Course ID
Grade
Semester
```

---

# Bonus (Easy Wins)

These are small additions that score points.

## Search Students

```TypeScript
const [search, setSearch] = useState("");
```

```TypeScript
const filteredStudents = students.filter(
  (student) =>
    student.name
      .toLowerCase()
      .includes(search.toLowerCase()) ||
    student.id.includes(search)
);
```

Input:

```TypeScript
<input
  value={search}
  onChange={(e) => setSearch(e.target.value)}
/>
```

---

## Sort Students

```TypeScript
const sortedStudents = [...filteredStudents].sort(
  (a, b) => a.name.localeCompare(b.name)
);
```

---

## URL Persistence

Using Next.js:

```TypeScript
const router = useRouter();
const searchParams = useSearchParams();
```

Update URL:

```TypeScript
router.replace(
  `?search=${search}`
);
```

Load initial value:

```TypeScript
const initialSearch =
  searchParams.get("search") || "";
```

This satisfies the URL state requirement without creating extra pages.

---

# What I Would Submit

A single `app/page.tsx` containing:

* seed data
* types
* localStorage initialization
* 3 forms
* 3 tables
* React Hook Form
* Zod validation
* grade validation (0–100)
* student ID validation
* search
* simple sorting
* URL search persistence

This is realistic to finish comfortably within the 45-minute limit and meets all required functionality without overengineering.

* * *

**You:**

How do I take it into reality 
like build it push it and make it live with vercel and github

* * *

**ChatGPT:**

If you're starting from scratch, this is the fastest workflow:

## 1. Create the Next.js Project

Open terminal:

```Bash
npx create-next-app@latest student-management-system
```

Choose:

```
✔ TypeScript? Yes
✔ ESLint? Yes
✔ Tailwind CSS? Yes
✔ src/ directory? No
✔ App Router? Yes
✔ Turbopack? Yes
✔ Customize import alias? No
```

Go into project:

```Bash
cd student-management-system
```

Install validation libraries:

```Bash
npm install react-hook-form zod @hookform/resolvers
```

Start development server:

```Bash
npm run dev
```

Open:

```
http://localhost:3000
```

---

## 2. Replace app/page.tsx

Open:

```
app/page.tsx
```

Delete everything.

Paste your entire Student Management System code into this file.

Save.

Your browser should auto-refresh.

---

## 3. Test Everything

Verify:

### Students

* Add a student
* Refresh page
* Data still exists

### Enrollment

* Add enrollment
* Refresh page
* Data still exists

### Grades

* Add grade
* Test:

```
101
```

Should fail.

Test:

```
88
```

Should pass.

---

## 4. Create GitHub Repository

Go to:

GitHub

Create repository:

```
student-management-system
```

Do NOT add README.

---

## 5. Initialize Git Locally

Inside project:

```Bash
git init
```

```Bash
git add .
```

```Bash
git commit -m "Initial commit"
```

Connect repository:

```Bash
git remote add origin https://github.com/YOUR_USERNAME/student-management-system.git
```

Push:

```Bash
git branch -M main
```

```Bash
git push -u origin main
```

Refresh GitHub.

Your code should be visible.

---

## 6. Deploy to Vercel

Go to:

Vercel

Sign in with GitHub.

Click:

```
Add New Project
```

Import:

```
student-management-system
```

Leave defaults.

Click:

```
Deploy
```

Wait ~30 seconds.

You will get a URL like:

```
https://student-management-system.vercel.app
```

Now your project is live.

---

## 7. Future Updates

Make code changes.

Then:

```Bash
git add .
```

```Bash
git commit -m "Added search feature"
```

```Bash
git push
```

Vercel automatically redeploys.

No extra steps.

---

## Typical Interview Workflow

If I had this assignment, my actual workflow would be:

```
1. Create Next.js project
2. Paste seed data
3. Build Student CRUD
4. Add localStorage
5. Build Enrollment form
6. Build Grade form
7. Add Zod validation
8. Add search
9. Test refresh persistence
10. Push to GitHub
11. Deploy to Vercel
12. Submit:
    - GitHub URL
    - Vercel URL
```

That's the exact workflow many frontend developers use for take-home assessments and prototypes.

* * *

**You:**

what terminal 
tell me step by step with details

* * *

**ChatGPT:**

If you've never built and deployed a Next.js project before, here's the complete process from zero.

# Step 1: Install Required Software

### Install Node.js

Go to:

Node.js Download

Download the **LTS version** and install it.

After installation, verify it:

### Windows

Press:

```
Windows + R
```

Type:

```
cmd
```

Press Enter.

A black window opens. This is called the **Command Prompt** (a terminal).

Check Node:

```Bash
node -v
```

Example:

```
v24.1.0
```

Check npm:

```Bash
npm -v
```

Example:

```
11.4.2
```

If both work, you're ready.

---

# Step 2: Install VS Code

Go to:

Visual Studio Code

Install it.

Open VS Code.

---

# Step 3: Create a Folder for Projects

Example:

```
D:\Projects
```

or

```
C:\Projects
```

Create it manually in File Explorer.

---

# Step 4: Open Terminal in VS Code

Open VS Code.

Click:

```
File
→ Open Folder
```

Select:

```
D:\Projects
```

Now open the terminal:

```
Terminal
→ New Terminal
```

or press:

```
Ctrl + `
```

(The key below Esc.)

A terminal appears at the bottom.

Example:

```
PS D:\Projects>
```

This is where you'll type commands.

---

# Step 5: Create the Next.js App

In the terminal, run:

```Bash
npx create-next-app@latest student-management-system
```

You'll see questions.

Choose:

```
Would you like to use TypeScript? → Yes
Would you like to use ESLint? → Yes
Would you like Tailwind CSS? → Yes
Would you like your code inside a src directory? → No
Would you like to use App Router? → Yes
Would you like to use Turbopack? → Yes
Would you like to customize the import alias? → No
```

Wait a minute or two.

When finished:

```
Success! Created student-management-system
```

---

# Step 6: Enter the Project

In terminal:

```Bash
cd student-management-system
```

Now you should see:

```
PS D:\Projects\student-management-system>
```

---

# Step 7: Open Project in VS Code

If you created it from VS Code terminal:

```Bash
code .
```

This opens the project folder.

You should see:

```
app/
public/
package.json
tailwind.config.ts
next.config.ts
```

---

# Step 8: Run the App

In terminal:

```Bash
npm run dev
```

You should see:

```
Ready in 2s
Local: http://localhost:3000
```

Open:

```
http://localhost:3000
```

in Chrome.

You now have a running Next.js website.

---

# Step 9: Edit page.tsx

In VS Code:

Open:

```
app
 └── page.tsx
```

Delete everything inside.

Paste your Student Management System code.

Save:

```
Ctrl + S
```

The browser automatically refreshes.

---

# Step 10: Install Validation Libraries

Stop nothing; open another terminal or use the same one.

Run:

```Bash
npm install react-hook-form zod @hookform/resolvers
```

Wait until installation finishes.

---

# Step 11: Test Local Storage

Open website.

Add:

```
Student ID: 2620009
Name: Mahir
Email: mahir@uni.edu
Program: CSE
Year: 2
```

Submit.

Refresh browser.

If the student is still there, localStorage works.

---

# Step 12: Create GitHub Account

If you don't have one:

GitHub Sign Up

Create account.

---

# Step 13: Install Git

Download:

Git for Windows

Install with default settings.

Verify:

```Bash
git --version
```

Example:

```
git version 2.50.0
```

---

# Step 14: Create Repository

Go to GitHub.

Click:

```
New Repository
```

Repository name:

```
student-management-system
```

Click:

```
Create Repository
```

---

# Step 15: Push Code to GitHub

Inside project terminal:

```Bash
git init
```

```Bash
git add .
```

```Bash
git commit -m "Initial version"
```

Connect your repository:

```Bash
git remote add origin https://github.com/YOUR_USERNAME/student-management-system.git
```

Replace:

```
YOUR_USERNAME
```

with your GitHub username.

Push:

```Bash
git branch -M main
```

```Bash
git push -u origin main
```

Refresh GitHub.

Your code should now be online.

---

# Step 16: Deploy to Vercel

Go to:

Vercel

Click:

```
Continue with GitHub
```

Authorize GitHub.

---

# Step 17: Import Project

Inside Vercel:

```
Add New Project
```

Select:

```
student-management-system
```

Click:

```
Import
```

Then:

```
Deploy
```

Wait about 30–60 seconds.

---

# Step 18: Live Website

Vercel gives a URL like:

```
https://student-management-system.vercel.app
```

Anyone can visit it.

You now have:

✅ Local development environment  
✅ Next.js app  
✅ GitHub repository  
✅ Live Vercel deployment

That's the complete beginner-to-live workflow used by many frontend developers.

* * *

**You:**

npx : File C:\Program Files\nodejs\npx.ps1 cannot be loaded because running scripts is disabled on this 
system. For more information, see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ npx create-next-app@latest student-management-system
+ ~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess

what's the problem

* * *

**ChatGPT:**

This is a **Windows PowerShell security policy issue**, not a Next.js problem.

PowerShell is blocking `.ps1` scripts, and `npx` uses `npx.ps1`.

## Quick Fix (Recommended)

Open **PowerShell as Administrator**:

1. Press **Windows**
2. Search:

```
PowerShell
```

3. Right-click **Windows PowerShell**
4. Click:

```
Run as Administrator
```

Run:

```PowerShell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

It will ask:

```
[Y] Yes
[N] No
```

Type:

```
Y
```

Press Enter.

Close PowerShell.

Open VS Code again and try:

```Bash
npx create-next-app@latest student-management-system
```

---

## Verify Policy

Run:

```PowerShell
Get-ExecutionPolicy
```

You should see:

```
RemoteSigned
```

or

```
Bypass
```

instead of:

```
Restricted
```

---

## Alternative (No Admin Needed)

Use **Command Prompt (CMD)** instead of PowerShell.

Press:

```
Windows + R
```

Type:

```
cmd
```

Then run:

```Bash
npx create-next-app@latest student-management-system
```

CMD usually bypasses this PowerShell restriction.

---

## Check Node Installation

Run:

```Bash
node -v
```

and

```Bash
npm -v
```

Tell me the output of both commands.

Also tell me whether you're using:

* PowerShell
* CMD
* VS Code Terminal

and I'll guide you through the next step.

* * *

**You:**

Using npm.

Initializing project with template: app-tw 


Installing dependencies:
- next
- react
- react-dom

Installing devDependencies:
- @tailwindcss/postcss
- @types/node
- @types/react
- @types/react-dom
- eslint
- eslint-config-next
- tailwindcss
- typescript


added 356 packages, and audited 357 packages in 2m

145 packages are looking for funding
  run npm fund for details

2 moderate severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run npm audit for details.

Generating route types...
✓ Types generated successfully

Initialized a git repository.

Success! Created student-management-system at D:\Projects\student-management-system

npm notice
npm notice New minor version of npm available! 11.13.0 -> 11.17.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.17.0
npm notice To update run: npm install -g npm@11.17.0
npm notice

* * *

**ChatGPT:**

Perfect. 🎉

Your Next.js project was created successfully.

The important line is:

```
Success! Created student-management-system
```

The security issue is gone and Next.js is installed correctly.

## Next Step

Open a terminal inside the project folder.

You are currently here:

```
D:\Projects\student-management-system
```

Run:

```Bash
cd D:\Projects\student-management-system
```

Then:

```Bash
npm run dev
```

You should see something similar to:

```
▲ Next.js 16.x.x
- Local: http://localhost:3000

✓ Ready in 2.3s
```

---

## Open the Website

Open your browser and visit:

```
http://localhost:3000
```

You should see the default Next.js page.

---

## Open the Project in VS Code

If VS Code isn't already showing the project:

```Bash
code .
```

If that doesn't work, simply:

1. Open VS Code
2. File → Open Folder
3. Select:

```
D:\Projects\student-management-system
```

---

## Check the Project Structure

You should see something like:

```
student-management-system/
│
├── app/
│   ├── favicon.ico
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
│
├── public/
│
├── package.json
├── tsconfig.json
├── next.config.ts
└── ...
```

---

## Install Form Libraries

Stop nothing. Open another terminal in VS Code and run:

```Bash
npm install react-hook-form zod @hookform/resolvers
```

Wait until it finishes.

---

## Then Tell Me

1. Does `npm run dev` start successfully?
2. Can you open `http://localhost:3000`?
3. Can you see the file:

```
app/page.tsx
```

Once you confirm those three things, I'll give you the **first working version of the entire Student Management System** that you can paste directly into `app/page.tsx` and run immediately.

* * *

**You:**

Success! Created student-management-system at D:\Projects\student-management-system

npm notice
npm notice New minor version of npm available! 11.13.0 -> 11.17.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.17.0
npm notice To update run: npm install -g npm@11.17.0
npm notice
PS D:\Projects> cd D:\Projects\student-management-system
PS D:\Projects\student-management-system> npm run dev

> student-management-system@0.1.0 dev
> next dev

▲ Next.js 16.2.9 (Turbopack)
- Local:         http://localhost:3000
- Network:       http://192.168.0.158:3000
✓ Ready in 347ms
Attention: Next.js now collects completely anonymous telemetry regarding usage.
This information is used to shape Next.js' roadmap and prioritize features.
You can learn more, including how to opt-out if you'd not like to participate in this anonymous program, by visiting the following URL:
https://nextjs.org/telemetry

* * *

**ChatGPT:**

Excellent. You're now at the point where a real developer would start building.

Current status:

✅ Node.js installed  
✅ Next.js created  
✅ Project running  
✅ Local server working at `http://localhost:3000`

---

## Step 1: Open the Project in VS Code

In a **new terminal** (keep `npm run dev` running), run:

```Bash
code .
```

If VS Code opens, great.

If not:

1. Open VS Code manually
2. File → Open Folder
3. Select:

```
D:\Projects\student-management-system
```

---

## Step 2: Install the Form Libraries

Open another terminal inside VS Code:

```Bash
npm install react-hook-form zod @hookform/resolvers
```

Wait until it finishes.

---

## Step 3: Open page.tsx

In VS Code:

```
app
└── page.tsx
```

Open it.

You'll see some default Next.js starter code.

---

## Step 4: Replace the Contents

Delete everything inside `app/page.tsx`.

Replace it with this minimal test first:

```TypeScript
export default function Home() {
  return (
    <main className="p-10">
      <h1 className="text-4xl font-bold">
        Student Management System
      </h1>
    </main>
  );
}
```

Save (`Ctrl + S`).

---

## Step 5: Verify Hot Reload

Go to:

```
http://localhost:3000
```

You should now see:

```
Student Management System
```

If you see that text, your development environment is working perfectly.

---

## Step 6: Next

Once you confirm that the page changed successfully, I'll give you:

### Version 1

A complete working `app/page.tsx` containing:

* Seed data
* localStorage initialization
* Student Registration form
* Student table
* Add Student functionality

Then we'll add:

### Version 2

* Enrollment form
* Enrollment table

Then:

### Version 3

* Grade form
* Grade validation (0–100)

Then:

### Version 4

* Search
* Sorting
* URL persistence

This is exactly how I'd build the assessment in a real interview: get something working quickly, then add features incrementally.

* * *

**You:**

I have only worked with the things of vs code 
Im yet to do of ndoe js

* * *

**ChatGPT:**

That's completely fine. What you've done so far is actually the hardest setup part.

You already have:

✅ Node.js installed  
✅ Next.js project created  
✅ Development server running  
✅ Terminal working

You don't need prior Node.js experience to complete this assessment.

## Think of It Like This

### Before (HTML/CSS/JS)

You might have had:

```
index.html
style.css
script.js
```

and opened `index.html` in the browser.

### With Next.js

Instead of editing `index.html`, you edit:

```
app/page.tsx
```

When you save, the browser updates automatically.

That's the main difference for this project.

---

# What is Running Right Now?

You currently have:

```
D:\Projects\student-management-system
```

and

```Bash
npm run dev
```

running.

That command starts a local website on:

```
http://localhost:3000
```

So your computer is acting like a small web server.

---

# What You Need To Learn Today

For this assignment, you only need 3 things:

## 1. React JSX

Example:

```TypeScript
export default function Home() {
  return (
    <div>
      Hello World
    </div>
  );
}
```

This creates:

```HTML
<div>Hello World</div>
```

---

## 2. State

Example:

```TypeScript
const [count, setCount] = useState(0);
```

Think:

```
count = current value

setCount = change value
```

Button:

```TypeScript
<button onClick={() => setCount(count + 1)}>
  Add
</button>
```

---

## 3. localStorage

Stores data in the browser.

Example:

```TypeScript
localStorage.setItem("name", "Mahir");
```

Read:

```TypeScript
localStorage.getItem("name");
```

Refresh page:

```
Still exists
```

This is exactly how the Student Management System saves students.

---

# What Will Happen In The Assessment?

User enters:

```
ID: 2620009
Name: Mahir
Email: mahir@uni.edu
Program: CSE
Year: 2
```

Click:

```
Add Student
```

Code:

```TypeScript
setStudents([...students, newStudent]);
```

updates the screen.

Then:

```TypeScript
localStorage.setItem(...)
```

saves it.

Refresh:

```
Student still there
```

Done.

---

# Realistically

If I were helping a beginner complete this assignment, I would not worry about:

❌ Server Components  
❌ API Routes  
❌ Middleware  
❌ Authentication  
❌ Database  
❌ Docker  
❌ Redux  
❌ Custom Hooks  
❌ Advanced Folder Structure

The requirements specifically say:

> Build it in the simplest possible way

So we will keep **everything inside `app/page.tsx`**.

---

# Your Next Immediate Task

Open:

```
app/page.tsx
```

Tell me what you see.

You will likely see something like:

```TypeScript
export default function Home() {
  return (
    ...
  );
}
```

Paste the contents here (or a screenshot), and I'll walk you through replacing it with your first working Student Management System screen.

* * *

**You:**

Now what do I do?

* * *

**ChatGPT:**

You're at the point where you start coding.

### 1. Open the project in VS Code

If VS Code is not already showing the project:

```Bash
code .
```

(or open VS Code → File → Open Folder → `D:\Projects\student-management-system`)

---

### 2. Open `app/page.tsx`

In the left sidebar, expand:

```
app
└── page.tsx
```

Click `page.tsx`.

---

### 3. Replace the starter code

Delete everything in that file and paste:

```TypeScript
export default function Home() {
  return (
    <main className="min-h-screen p-10">
      <h1 className="text-4xl font-bold">
        Student Management System
      </h1>

      <p className="mt-4">
        My Next.js app is working 🎉
      </p>
    </main>
  );
}
```

Save with:

```
Ctrl + S
```

---

### 4. Check the browser

Go to:

```
http://localhost:3000
```

You should see:

```
Student Management System
My Next.js app is working 🎉
```

If you see that, then:

* Next.js is working
* TypeScript is working
* Tailwind is working
* Hot reload is working

and you're ready to build the actual assignment.

---

### 5. Tell me what happened

Did the page change successfully?

If yes, I'll give you the **next chunk of code**: the Student Registration form with localStorage and seed data, and we'll build the whole assignment step-by-step instead of dumping 500+ lines at once.