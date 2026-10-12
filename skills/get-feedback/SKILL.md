---
name: get-feedback
description: Send the current site build to Plotter for client review, or push a new version after feedback. Use when the user asks for feedback, a review link, or to "send this to Plotter".
---

# Get feedback with Plotter

Plotter hosts a static build privately and returns a review link. The client
marks up the real page (pins, ink, comments, voice) on iPad, iPhone or a
computer; their marks come back to the team as tasks.

## A site that's already live

If the site is already published at an address the client can open (a
staging or production URL, a Lovable, Bolt, v0 or Replit app), it doesn't need
hosting: the team reviews it as a project in Plotter (app.plotter.run). Skip to
"When feedback comes back" and find its tasks with `list_tasks` and `site_url`.

## Steps for a build

1. **Build** the site so there's a static output folder with an `index.html`
   (for example `dist/`, `out/` or `build/`). Plotter hosts static files only:
   HTML, CSS, JS, images, fonts and media. No server code, and pages can't call
   outside APIs when hosted.
2. **Find the project.** If `.plotter.json` in the repo has a `projectId`, this
   is a new version of an existing review. Otherwise it's a new review.
3. **Send the files** with the `plotter` MCP tools:
   - New review: `create_review` with a short `title` and `files`.
   - New version: `push_version` with the `project_id` and `files`.

   Each file is `{ path, content, encoding }`. Paths are relative to the
   output folder. Text files use `utf8`; images, fonts and media use `base64`.
   For a large build, upload a .zip somewhere reachable and pass `archive_url`
   instead.
4. **Save** the returned `projectId` to `.plotter.json` as
   `{ "projectId": "…" }`, so the next push becomes version 2.
5. **Tell the user** the review link to send their client. If Plotter says the
   version needs confirming (it has sign-in, card or off-site form fields),
   pass on the dashboard link: a team member confirms it there before it can
   be shared.

## When feedback comes back

The client's marks become tasks. The `apply-feedback` skill reads them
(`list_tasks`, `get_task`), applies the ones the user picks, and pushes the
next version with `applied_task_ids`, so the client checks each fix.
