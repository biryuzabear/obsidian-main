# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is an Obsidian vault containing personal notes. All content is in Markdown format following Obsidian conventions.

## Folder Structure

### Active Content
- **Daily/** - Daily notes organized by month (April, June, July, March)
- **Notes/** - Quick notes and references (e.g., Cloudflare.md)
- **Ideas/** - Future plans and project ideas (AWS Certification, LLM projects, personal goals)
- **Projects/** - Active projects with tracking (Job Search, German Learning, Self Employment, etc.)
- **Events/** - Upcoming or past events (interviews, appointments)
- **Pages/** - Standalone pages with arbitrary format (test tasks, medical descriptions, any freeform content)

### Reference & Knowledge
- **Content/** - Book notes and summaries (A Liberated Mind, Jedi Techniques, The Happiness Trap)
- **Documents/** - Official documents organized by country (Armenia, Denmark, Germany, Israel, Russia, Ukraine, USSR)
- **People/** - Person profiles (family members, contacts)
- **Sites/** - Website/service information (Job Center Riesa)
- **Bases/** - Database files for Ideas and Projects tracking

### Records & Media
- **Records/** - Collections and data (e.g., Tarot readings)
- **Attachments/** - Images, screenshots, scanned documents (PNG, JPG files with timestamps)

### Archive & Templates
- **Archive/** - Old/completed content:
  - Archive/Therapy Sessions/ - Therapy session notes
  - Archive/Biryuzabeard/ - Old journal entries
  - Archive/Notes/ - Archived notes
  - Archive/Projects/ - Completed projects
  - Archive/Medical/ - Medical history
- **Templates/** - Note templates for each content type (content.md, daily.md, project.md, etc.)

## File Placement Guidelines

| Content Type          | Location                         |
| --------------------- | -------------------------------- |
| Daily journal entry   | Daily/{Month}/                   |
| New project           | Projects/{ProjectName}.md        |
| Content notes         | Content/{TitleName}.md           |
| Person profile        | People/{FullName}.md             |
| Quick note/reference  | Notes/{Topic}.md                 |
| Standalone page       | Pages/{PageName}.md              |
| Future idea/goal      | Ideas/{IdeaName}.md              |
| Event/appointment     | Events/{EventName}.md            |
| Official document     | Documents/{Country}/             |
| Image/attachment      | Attachments/                     |
| Completed/old content | Archive/{appropriate subfolder}/ |

## Conventions

- Use templates from Templates/ when creating new notes
- Person names in Cyrillic for Russian speakers
- Treat all content as personal and confidential
- Maintain Obsidian-compatible syntax (wikilinks, tags, YAML frontmatter)
- Prioritize Russian over English for file content

## Working with Projects

- **Iterative approach** — work through steps, can add new ones in parallel
- **Artifacts** — results of steps (plans, information, code) are added to project notes
- **Headings** — use correct hierarchy (H1 for title, H2 for sections, H3+ for subsections)
- **Callouts** — use for:
  - Large chunks of information (features, use cases)
  - Code blocks — **always collapsed** (`> [!code]-`)
  - Important notes and warnings
- **Project structure**:
  - Goal — project objective
  - Steps — steps with checkboxes
  - Info — artifacts, features, use cases, code, notes
