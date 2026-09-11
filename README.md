# Team Access Manager

A single-page web app for managing team members, roles, and permissions —
built using **Cursor**, an AI-assisted code editor, to prototype the UI
quickly from a written spec rather than hand-coding every element.

## Why I built this

The job I'm applying for specifically calls out comfort with AI-assisted
development tools like Cursor, Replit, and Copilot. This project is a
direct demonstration of that: I wrote a detailed feature spec, had
Cursor's AI agent generate the implementation, then reviewed, tested, and
verified the result myself before shipping it — the same workflow this
role would actually use day to day.

## What it does

- Add a team member (name, email, role)
- Three roles — Admin, Editor, Viewer — each mapped to a fixed set of
  permissions
- Changing a person's role updates their shown permissions instantly
- Remove a team member
- Data persists in the browser (localStorage) between visits

## How it was built

1. Wrote a specific prompt describing the exact features, role/permission
   mapping, and styling constraints
2. Cursor's AI agent generated `index.html` (HTML, CSS, and JS in one
   file, no framework or build step)
3. I reviewed the generated code to confirm the permission logic and
   remove functionality matched the spec
4. Manually tested every feature (add, role change, remove, refresh
   persistence) before pushing to GitHub

## Tech

Plain HTML/CSS/JavaScript, generated with Cursor and reviewed/tested by
hand. Runs by just opening `index.html` in a browser — no install needed.

## A note on process

I used an AI coding assistant to generate this UI, which is exactly the
kind of AI-assisted development this role is looking for. I reviewed and
manually tested every feature listed above before considering it done —
the goal was fast, correct prototyping, not blindly shipping
AI-generated code without checking it.
