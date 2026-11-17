# Skedge 🗓️

A browser-based schedule builder for a Jewish day high school (Stella K. Abraham High School for Girls). It builds the school-wide master schedule, staffs open sections, places students into their tracked classes, and flags conflicts automatically. It also includes an AI scheduling assistant powered by Google's Gemini API.

**Live demo:** https://rkamine2-sys.github.io/Skedge/

> **Privacy note:** All faculty names in this public version have been replaced with placeholders (Teacher01, Teacher02, …). No student data is stored in the code: students are added in the app or imported from a file, and saved only in the user's own browser. No API key is included. Each user supplies their own Gemini key.

---

## Features

- **Faculty directory** with an availability editor (per day and period), room and notes, plus undo
- **Student enrollment** by grade: paste a list of names, and required classes fill in by grade
- **Tracked subjects:** Chumash, Navi, Ivrit, Halacha (by section), math tracks for grades 9–11, AP choices, and 12th-grade electives
- **Schedule grid** viewable by day and by grade, with drag-and-drop to move a class to another period
- **Class editor:** change a class's teacher, room, section or students
- **Automatic conflict detection** with a live badge (⚠ conflicts / ✓ no conflicts)
- **Undo / redo** (Ctrl+Z / Ctrl+Y), up to 50 steps
- **Auto-save** to the browser, plus **export/import** of all data as a JSON file
- **PDF export** of the schedule
- **Gemini AI assistant** that answers questions about the live schedule in plain English

## How it works

### Building the schedule
1. Class slots are created from the school's master schedule template (period, grade, course, section, room, days, teacher).
2. **Auto-staffing:** any class without a teacher gets the first teacher who is in the right department, isn't already teaching that period on an overlapping day, and is marked available (a greedy approach).
3. **Student placement:** students are placed into sections by grade and track (e.g. their Chumash section or math track), AP choices and electives.
4. **Validation:** the schedule is checked for conflicts and the result is shown immediately.

### Conflict detection
- **Teacher double-booking:** the same teacher in the same period on a shared day
- **Room double-booking:** the same room in the same period on a shared day (shared spaces like the auditorium and gym are excluded)
- **Missing lunch:** any student with class during both lunch periods (5A and 5B) on the same day
- **Student double-booking:** when a student is added to a class by hand, the app warns if she's already booked that period and offers to transfer her

### Saving
Changes auto-save to the browser's `localStorage` (debounced, so rapid edits are batched into one save), with a status indicator (Saving… / Saved / Save failed). Data is also saved when the tab closes. Use **Export** to download a backup or move data to another computer, and **Import** to restore it.

### AI assistant
Each question is sent to the Gemini REST API with a role description, rules, and a snapshot of the live data: faculty, the schedule, teacher availability, and the conflicts already detected by the app's own code. The model isn't trained or fine-tuned. Its answers are grounded in the current data.

## Tech
HTML · CSS · Vanilla JavaScript · Browser localStorage · Google Gemini API

Single file, no frameworks, no server. Download `index.html` and open it in any modern browser.

## Using the AI assistant
Open the chat panel, click **Set key**, and paste your own Gemini API key (free from Google AI Studio). The key is stored only in your browser.

## Roadmap
- A full constraint-satisfaction scheduler that builds the timetable from scratch (with backtracking)
- Student double-booking checks in the main conflict scan
- Gemini function calling, so the assistant can query the app's own conflict checker directly
- A backend with a cloud database for shared, multi-user schedules, backups and an audit trail, which would also keep the API key off the browser
- Sending anonymized IDs instead of names to the AI

## About
Designed and built by [rkamine2-sys](https://github.com/rkamine2-sys) using AI-assisted development.
