# Plotter for Gemini CLI

[Plotter](https://plotter.run) lets people mark up websites, web apps, images and
designs, share feedback and track changes together, in their browser or on iPhone
and iPad. This extension connects Gemini CLI to your Plotter account so it can work
through that feedback.

- **apply-feedback**: reads the open tasks for a site (each tied to the exact page
  element, with a picture of the mark), marks each one in progress, makes the
  change, and resolves it with a reply saying what changed. When a task is unclear
  it asks you, or asks the reviewer on the task's thread and leaves it open until
  they answer.
- **get-feedback**: builds a static site, hosts it privately on Plotter and returns
  a review link for your client.

## Install

```sh
gemini extensions install https://github.com/Plotter-Run/gemini-extension
```

Then, in Gemini CLI, run `/mcp auth plotter`. Plotter opens in your browser: sign
in and pick the organization. There's no token to copy. Revoke access any time in
Plotter under **Settings › Integrations**.

## Use it

In your site's repository, ask:

- "Work through the Plotter feedback for https://example.com"
- "What did the client say about the pricing page?"
- "Build this and send it to Plotter for review"

## Help

- Setup guide: https://plotter.run/help/connect-claude-code-or-codex
- Privacy: https://plotter.run/privacy
- Support: https://plotter.run/support
