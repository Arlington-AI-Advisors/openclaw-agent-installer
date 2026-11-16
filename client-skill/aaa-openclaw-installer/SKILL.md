---
name: aaa-openclaw-installer
description: Guide an approved Arlington AI Advisors client through installing OpenClaw from the AAA client bundle, preview first, with the client running each command in their own Terminal.
---

# AAA OpenClaw Installer

Use this skill when an approved Arlington AI Advisors client wants to install OpenClaw from the bundle they downloaded from the AAA Client page.

## Goal

A clean, previewed, client-approved OpenClaw studio setup, with no unsupervised changes to the client's machine.

## What is in the bundle

This folder is the bundle. It contains:
- `install.sh`: the AAA OpenClaw installer
- `skills.git`: the studio guide skills the installer sets up, so nothing has to come from GitHub
- `VERSION.txt`: which installer and skills versions this bundle was built from

The installer reads the skills from this folder when `STUDIO_SKILLS_URL` points at `skills.git`.

## Important: the client runs the installer, not you

`install.sh` requires an interactive terminal. It stops with "An interactive terminal is required" if you run it from your own tool. So:
- Give the client one command at a time to paste into their own Terminal app.
- Ask them to paste back the output, then explain it in plain English.
- Never try to work around the interactive-terminal check.

## Procedure

1. **Confirm the boundary.**
   - The machine belongs to the client or is approved for this install.
   - No passwords, API keys, Discord tokens, or other secrets get pasted into this chat.
   - Stop if anyone asks you to bypass approvals.

2. **Find the bundle and check the machine.** You may run these read-only checks yourself:
   - The absolute path of this folder (call it BUNDLE). Prefer `~/aaa-openclaw-installer`.
   - `BUNDLE/install.sh` and `BUNDLE/skills.git` both exist. Read `BUNDLE/VERSION.txt` and tell the client which version they have.
   - OS: macOS, Linux, or Windows with Ubuntu on WSL2. On Windows, stop until Ubuntu on WSL2 is ready.
   - `git --version` and `curl --version`. The client runs as a normal user, never `sudo`.

3. **Explain the plan in plain English.**
   - What the installer checks and what it may install: Git, GitHub CLI, OpenClaw and Node, and the chosen coding assistant
   - Which steps need the human: sign-ins, paid services, Discord bot creation
   - How to stop at any time: Ctrl+C

4. **Preview first.** Have the client open Terminal and paste exactly this, with BUNDLE replaced by the real absolute path:

   ```bash
   STUDIO_SKILLS_URL="file://BUNDLE/skills.git" bash "BUNDLE/install.sh" --preview --plain
   ```

   The preview installs nothing and changes no files. Ask the client to paste the output back. Summarize what is already installed, what would change, which accounts are needed, where credentials would be stored, and every approval the installer will ask for.

5. **Install only after explicit approval.** Have the client paste:

   ```bash
   STUDIO_SKILLS_URL="file://BUNDLE/skills.git" bash "BUNDLE/install.sh"
   ```

   - Stay with them through each prompt; answer questions, do not answer prompts for them.
   - Keep the studio folder choice explicit.
   - Reuse working tools; never replace a broken existing OpenClaw install without asking.
   - Have them save the exact return/resume command the installer prints.

6. **Verify and hand back.**
   - Note the versions from `VERSION.txt`.
   - Confirm the return command.
   - Tell the client the next human steps only.
   - Do not start outbound messaging, Discord bot creation, or paid account changes unless the client explicitly asks and understands the action.

## Hard rules

- Never invent credentials, and never ask for secrets in chat.
- Never force `sudo`.
- Never skip the preview.
- A website password is not authorization for destructive changes.
- Do not download the installer or skills from anywhere else; this bundle is the approved copy.
- If the bundle is missing `install.sh` or `skills.git`, stop and tell the client to contact Arlington AI Advisors.
