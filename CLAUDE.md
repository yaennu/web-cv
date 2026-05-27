# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **greenfield CV portfolio website** for a single person. The full feature specification lives in `FEATURES.md` — read it before making any implementation decisions. No framework has been chosen yet; tech stack selection is part of the work.

## Pages & Routes

| Route | Purpose |
|---|---|
| `/` | Home — profile card, bio, projects, tech background |
| `/cv` | Interactive timeline chart + filterable data table + PDF download |
| `/skills` | Radar/polar chart with category radio selector (Technical/Social/Methodical/Language) |
| `/assistant` | Stateless AI chatbot that answers CV-related questions |

## Data Model

**CV entries:** Category (Work/Education/Coding/Diverse), Start, End, Order, Place, Description, DescriptionShort

**Skills entries:** Skill (Technical/Social/Methodical/Language), Description, Order, Level (Experience/Interest), Value (1–5)

The Skills Technical category renders **two overlaid radar polygons** (Experience filled + Interest outline). All other categories render a single Experience polygon.

## Design Tokens

| Token | Value |
|---|---|
| Primary colour | `#003eb0` (dark blue) |
| Secondary colour | `#8fa7d3` (nav/footer backgrounds) |
| Font | Sans-serif (Helvetica/Arial fallback) |
| H1 | 1 rem bold, primary colour |
| H2 | 1.5 rem bold, primary colour |
| Border radius | 4 px |
| Content max-width | ~750 px |

All external links open in a new tab. Nav link hover turns text white.

## AI Assistant Behaviour

- Input: single-line, max 500 chars, placeholder "Ask something about my CV."
- Empty input shows "Please enter a question." without making an API call.
- Each submission is independent — no conversation history.
- Strip citation markers from responses before display.
- Show a loading spinner during the async fetch.

## Commands

No build tooling is configured yet. Once a framework is chosen, document its commands here (e.g. `npm run dev`, `npm run build`, `npm test`, `npm run lint`).
