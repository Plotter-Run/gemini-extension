---
name: apply-feedback
description: Apply client feedback from Plotter to the site or design. Use when the user asks to apply, fix or work through Plotter feedback, review comments or tasks, or "what did the client say".
---

# Apply Plotter feedback

Clients mark up pages in Plotter (pins, ink, comments, copy changes, voice).
Each mark is a **task** tied to the exact page element. This skill reads the
open tasks, applies the ones the user picks, and closes the loop.

## Steps

1. **Find the project.** Use `projectId` from `.plotter.json` if the repo has
   one. If you know the site's published address, skip this: `list_tasks`
   takes `site_url` instead. Otherwise call `list_projects` (it includes your
   Quick reviews, marked `quick`) and ask which project.
2. **List the open tasks** with `list_tasks` (`project_id` or `site_url`; add
   `page` to focus on one page). Show the user a short numbered list: task number, page, what
   the client asked. Ask which to apply (default: all of them). Tasks marked
   `waiting_on_reviewer` have a question nobody has answered yet: list them
   apart and leave them, unless the user says otherwise. A task whose
   `last_reply.by` is `reviewer` may hold the answer to an earlier question:
   read its thread.
3. **Read each chosen task** with `get_task`. It returns the note or the copy
   change (`original_text` → `proposed_text`), the thread, the voice transcript,
   the element (`selector`, `tag`, `text`, `near_text`) and a picture of the
   page as the client marked it, with their ink. Look at the picture: a circle
   or an arrow often says more than the note.
4. **Find the element in the source.** Search for the element's text and
   `near_text` first, then use the selector and tag to confirm. For a copy
   change, replace `original_text` with `proposed_text` exactly.
5. **Apply the change.** Mark the task `in_progress` with `update_task` as
   you start, so the team sees it's being worked on. Keep it to what was
   asked. If a task is ambiguous, or you can't find the element with
   confidence, don't guess:
   - If the user can answer (which file, which component, what they meant),
     ask them now.
   - If only the reviewer can (what they want changed), ask on the task's
     thread: `update_task` with a short, specific `reply` ("Which headline:
     the hero, or the one above the menu?") and `status: "open"`. The reviewer
     gets it by email; `list_tasks` shows the task as `waiting_on_reviewer`
     until they answer. Tell the user you asked.
6. **Summarise** per task: what changed (file and line), why it was skipped,
   or what you asked.
7. **Close the loop:**
   - **Hosted review** (the project was made with `create_review`): build, then
     `push_version` with the files and `applied_task_ids` (the tasks you
     applied). They're marked resolved, and the client sees them in the new
     version as "Fixed — check it", so they confirm each one. Give the user the
     new review link.
   - **Live site** (the user deploys it): after the change is deployed, call
     `update_task` for each applied task with `status: "resolved"` and a short
     `reply` saying what changed ("Applied: the headline is 48px now."). The
     reviewer gets the reply by email. Don't resolve tasks for changes that
     aren't live yet; reply with what changed and leave the status.

## Never

- Don't resolve a task you skipped or asked about.
- Don't leave a task `in_progress` you didn't finish: set it back to `open`
  with a reply saying why.
- Don't change anything the tasks didn't ask for.
- Don't paste the client's personal details into commits or code.
