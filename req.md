# How to Add Results — KloudCampus

A practical guide for Admins and Exam Controllers on entering, approving and
publishing student results.

> **TL;DR** — Go to **Results** in the sidebar. Add marks one of three ways
> (manual grid, CSV upload, or generate from exam marks), then **Approve** and
> **Publish**. Published results appear on the student dashboard and SGPA/CGPA
> are calculated automatically.

---

## 1. The results lifecycle

```
Faculty enter exam marks ─┐
                          ├─►  Result (pending) ──► Approved ──► Published
Admin / Exam Controller ──┘                                          │
   (manual or CSV entry)                                             ▼
                                              Student dashboard + SGPA/CGPA
```

Every result has a **status**:

| Status | Meaning |
|---|---|
| `pending` | Entered/generated, awaiting review |
| `approved` | Reviewed and approved, ready to publish |
| `published` | Visible to students; counts toward SGPA/CGPA |
| `withheld` | Rejected / held back |

---

## 2. Who can do what

| Action | Required permission | Roles that have it |
|---|---|---|
| Enter / import marks | `grades:approve` (or `grades:submit`) | Admin, Exam Controller, Principal |
| Approve results | `grades:approve` | Admin, Exam Controller, HOD, Principal |
| Publish results | `grades:publish` | Admin, Exam Controller, Principal |
| Submit exam marks | `grades:submit` | Faculty, Exam Controller, Admin |

The **Admin** has all of the above.

---

## 3. Where to go

Sidebar → **Results**. The page has four tabs:

- **Result Entry** — enter marks in a grid for a class
- **Manage Results** — approve, publish, generate, edit, calculate GPA, export
- **Analytics** — pass %, grade distribution, KPIs
- **Student Lookup** — view a single student's full result history

Plus an **Upload Results** button (top‑right) for CSV import.

---

## 4. Method A — Manual entry (one class at a time)

Best for: entering a single subject/semester for a current class.

1. Open **Results → Result Entry**.
2. Choose the filters: **Academic Year**, **Department**, **Semester**
   (optionally **Section** and **Course**).
3. Click **Load Students**. The grid lists the students with a marks column per
   subject.
4. Type each student's marks. Leave blank or mark **Absent** as needed.
5. **Save** the row/grid. Results are created with status **pending**.
6. Go to **Manage Results** to **Approve** and then **Publish** (see §7).

> ⚠️ **Important — the manual grid loads students by their *current* semester.**
> It shows only students whose current semester equals the one you selected. So
> you **cannot** hand‑enter a semester a student has already completed (e.g. a
> Semester‑3 backlog for someone now in Semester 5). For previous
> year/semester results, use **Method B (CSV upload)** or **Method C (generate)**
> below, which have no such restriction.

---

## 5. Method B — Bulk upload (CSV) — works for any semester/year

Best for: large batches, **previous‑year / previous‑semester / backlog /
supplementary** results.

1. Open **Results** → click **Upload Results**.
2. Select the **Semester** and **Course** the marks belong to (any semester,
   past or present).
3. Click **Download Template** to get the CSV format, or paste CSV directly.

   **CSV columns:**

   ```csv
   rollNumber,marksObtained,isAbsent,remarks
   CS-001,85,false,
   CS-002,,true,Absent
   CS-003,72,false,Grace marks applied
   ```

   - `rollNumber` — student's roll number (required, must match an existing student)
   - `marksObtained` — number; leave empty if absent
   - `isAbsent` — `true` / `false`
   - `remarks` — optional note

4. Click to **parse/preview**. Any bad rows (unknown roll number, marks over
   the max, duplicates) are listed so you can fix them.
5. **Import**. Matching students get a result (status **pending**) for that
   semester + course. Existing results for the same student/course/semester are
   updated (upsert).
6. Go to **Manage Results** to **Approve** and **Publish**.

> Because upload matches purely by **roll number**, it ignores the student's
> current semester — this is the recommended way to add historical results.

---

## 6. Method C — Generate from submitted exam marks

Best for: when faculty have already entered internal/exam marks and you want the
system to compute the consolidated result.

1. Make sure faculty have submitted exam marks and they are **approved**.
2. Open **Results → Manage Results**.
3. Use **Generate Results** for the chosen **Semester + Course**.
4. The system aggregates the approved exam marks (applying each exam's
   weightage), computes percentage, grade and grade points, and creates results
   with status **pending**.
5. **Approve** and **Publish**.

---

## 7. Approve & Publish

In **Results → Manage Results**:

1. Filter by status **pending**. Review the entries.
2. Select rows (or all) and click **Approve** → status becomes **approved**.
3. Select approved rows and click **Publish** (or use the Publish dialog scoped
   by semester/course) → status becomes **published**.
4. On publish:
   - Students can see the results on their dashboard.
   - Students receive a notification.
   - **SGPA/CGPA** are released (use **Calculate GPA** for the semester if you
     need to (re)compute them).

To correct a published result: **Unpublish** → edit/re‑import → **Approve** →
**Publish** again.

---

## 8. Grade scale (10‑point)

| Percentage | Grade | Grade points |
|---|---|---|
| ≥ 90 | O | 10 |
| ≥ 80 | A+ | 9 |
| ≥ 70 | A | 8 |
| ≥ 60 | B+ | 7 |
| ≥ 55 | B | 6 |
| ≥ 50 | C | 5 |
| ≥ 40 | P | 4 |
| < 40 | F | 0 |
| Absent | AB | 0 |

- **SGPA** = Σ(grade points × course credits) ÷ Σ(credits) for the semester.
- **CGPA** = cumulative SGPA across completed semesters.

---

## 9. What students see after publishing

On the student dashboard / **Examinations → My Results**:

- Subject‑wise marks, percentage and grade
- Semester result with **SGPA** and overall **CGPA**
- **Download Marks Memo** (CSV)
- Published exam schedule

---

## 10. Alternative: the Exam Controller module

Exam Controllers can do the same review/publish from **Examinations → Results**:

- Filter by status, select results, then **Approve**, **Send Back** (for
  correction), **Reject**, **Publish** or **Unpublish**.
- **Examinations → Notifications** can send a "Results Published" notice to all
  students, a batch, or a semester.

Marks entry itself (Methods A–C) is done on the **Results** page as above.

---

## 11. Adding previous‑year / previous‑semester results — summary

| Method | Works for past semesters? | Notes |
|---|---|---|
| **A. Manual grid** | ❌ No | Loads only students whose *current* semester matches |
| **B. CSV upload** | ✅ Yes | Matches by roll number; recommended for backlog/old results |
| **C. Generate from marks** | ✅ Yes | Needs approved exam marks for that semester |

---

## 12. Troubleshooting

| Problem | Cause / fix |
|---|---|
| Student not in the entry grid | They've moved past that semester — use CSV upload (Method B) |
| "Student not found" on upload | Roll number doesn't match — check exact roll number |
| "Marks exceed max" | Marks value is higher than the course's max marks |
| Results not visible to student | They're not **published** yet — publish them in Manage Results |
| SGPA/CGPA missing | Run **Calculate GPA** for the semester after publishing |

---

## Appendix — API endpoints (for developers)

| Action | Endpoint |
|---|---|
| Load class for entry | `POST /api/v1/results/load-students` |
| Bulk import (CSV) | `POST /api/v1/results/bulk-import` |
| Generate from marks | `POST /api/v1/results/generate` |
| Approve | `POST /api/v1/results/approve` |
| Publish | `POST /api/v1/results/publish` |
| Calculate SGPA/CGPA | `POST /api/v1/results/gpa/calculate` |
| Publish/unpublish selected (Exam Controller) | `POST /api/v1/exam-control/results/publish-ids` · `/unpublish-ids` |

All write actions are audit‑logged and tenant‑scoped.
