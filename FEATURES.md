# CV Portfolio Website — Feature Specification

This document describes every user-facing feature of the application, independent of any technology choices, so the project can be rebuilt from scratch on a new stack.

---

## Global Layout

- **Navigation bar** — sticky at the top of every page. Contains links to: Home, CV, Skills, Assistant.
- **Footer** — sticky at the bottom of every page. Displays: LinkedIn profile link, phone number, email link.
- **Content width** — capped at a maximum of ~750px, centered.
- **Responsive grid** — two-column layouts collapse to single column on small screens.

---

## Page: Home (`/`)

### Profile card (narrow left column)
- Circular portrait photo.
- Full name.
- Phone number (display only).
- "Mail me!" link (opens email client).
- LinkedIn external link.

### About Me (wide right column)
- Multi-paragraph biographical text describing professional background, location, employer history, and specialisation.
- Inline external links within the text (employer website, specialisation reference).

### Call to action
- Short intro text with inline links pointing to the CV, Skills, and Assistant pages.
- Full-width button to **download the PDF CV** (German, single file).

### Projects section
- Hierarchical bullet list of personal/professional projects grouped into categories:
  - Reports (with external links to published reports)
  - Interactive web apps (with external links to deployed apps)
  - Code packages (with link to source repository)
  - AI chatbot (with link to the Assistant page)
  - Dashboard (ongoing project, no external link)

### Technical background section
- Unordered list summarising the technologies used to build this website (informational only, no interactions).

---

## Page: CV (`/cv`)

### Timeline visualisation
- Interactive chart with time on the x-axis and career/education entries on the y-axis.
- Each entry belongs to a **Category** (Work, Education, Coding, Diverse); each category is rendered as a distinct coloured line/trace.
- Hovering over a data point displays a short description of that entry.
- Hover mode is unified (shows labels from all categories at the same time position).
- Zooming and panning are disabled; the axis ranges are fixed.

### CV data table
- Full data grid showing all CV entries.
- **Columns:** Category | Start date | End date | Place | Description.
- Per-column text filtering.
- Text wraps within cells (long descriptions are fully visible).
- Fixed height with vertical scrolling.

### Download
- Full-width button to download the same PDF CV as on the Home page.

---

## Page: Skills (`/skills`)

### Category selector
- A set of **radio buttons** (single-select) with four options: Technical, Social, Methodical, Language.
- Default selection: Technical.
- Selecting a different option immediately updates the chart without a page reload.

### Radar / polar chart
- Displays skill names around the perimeter of a radar polygon.
- Radial axis ranges from 0 to 5 (competency scale).
- Hovering shows the skill name and its value.

**Technical category** shows **two overlaid polygons**:
- **Experience** — filled polygon.
- **Interest** — outline-only polygon (same skills, different values).
- A legend distinguishes the two.

**Social / Methodical / Language categories** show **one polygon**:
- **Experience only** — filled polygon.

---

## Page: Assistant (`/assistant`)

### Introduction
- Heading and explanatory text explaining:
  - The assistant answers questions about the owner's CV.
  - It responds in the same language the user writes in.
  - It has access to the CV document for lookup.
  - Answers may occasionally be inaccurate (hallucination disclaimer).

### Chat interface
- **Text input** — single-line field, placeholder text "Ask something about my CV.", maximum 500 characters.
- **Submit button** — triggers the query.
- **Response area** — displays the assistant's answer as plain text. Shows a loading indicator while the response is being fetched.

### Interaction flow
1. User types a question and clicks Submit (or presses the equivalent keyboard action).
2. If the input is empty, the response area displays "Please enter a question." — no API call is made.
3. If the input is non-empty, a request is sent to the AI backend.
4. The response area shows a loading spinner until the answer arrives.
5. The answer is displayed as plain text; any internal citation markers are stripped before display.
6. Each submission is a fresh, independent query (no multi-turn conversation history).

---

## Data Model

### CV entries
Each entry has:
| Field | Type | Notes |
|---|---|---|
| Category | string | Work / Education / Coding / Diverse |
| Start | date | |
| End | date | |
| Order | integer | Sequence position for timeline display |
| Place | string | Employer / institution name |
| Description | string | Full detail text |
| DescriptionShort | string | Short text shown on timeline hover |

### Skills entries
Each entry has:
| Field | Type | Notes |
|---|---|---|
| Skill | string | Technical / Social / Methodical / Language |
| Description | string | Name of the individual skill |
| Order | integer | Display order within its category |
| Level | string | Experience or Interest |
| Value | number 1–5 | Competency or interest level |

---

## Assets & Static Files

- **Portrait image** — displayed on the Home page profile card.
- **PDF CV** — downloadable from Home and CV pages (same file, German language).

---

## Visual Design

| Token | Value |
|---|---|
| Primary colour | Dark blue `#003eb0` |
| Secondary colour | Light blue `#8fa7d3` (navbar, footer backgrounds) |
| Font | Sans-serif (Helvetica / Arial fallback) |
| Base font size | 1 rem |
| H1 | 1 rem bold, primary colour |
| H2 | 1.5 rem bold, primary colour |
| Nav link hover | Text turns white |
| Border radius | 4 px |
| Content max-width | ~750 px |

---

## Behavioural Notes

- All external links open in a new tab.
- Skills chart updates reactively on radio-button change — no page reload.
- CV table filtering is per-column and always visible.
- Chat input debounces briefly before triggering the callback; max 500 chars enforced.
- Loading spinner is shown during the AI response wait to signal async activity.
- The AI assistant produces independent (stateless) responses — no conversation memory across submissions.
