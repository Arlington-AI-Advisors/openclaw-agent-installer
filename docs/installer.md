# Installer notes

`install.sh` is a standalone Bash 3.2 script. No npm package or build step is
required to start it. The README command also works before Git is installed.

## Flow and terminal behavior

- The final compound command is parsed before stdin is reattached to `/dev/tty`.
  This lets `curl | bash` accept keyboard input and hand the terminal to the guide.
- [Gum](https://github.com/charmbracelet/gum) supplies keyboard selection, styled
  headings, folder input, confirmations, and animated spinners. Its temporary
  binary is checked against the release's SHA-256 manifest before execution.
- If Gum cannot download or run, setup continues with numbered prompts.
  `--plain`, `STUDIO_PLAIN=1`, `NO_COLOR=1`, and `TERM=dumb` select this mode.
- Install approval happens after the plan. Sign-in stays in the vendor CLI and
  is never redirected into an installation log. No permission-bypass flags are used.
- Existing tools must pass `--version` to count as ready. Existing OpenClaw
  installs are never upgraded by this script. Upstream upgrade paths can restart
  services, so a broken existing command stops for repair.
- The `openclaw-studio` skill is cloned from `Arlington-AI-Advisors/skills` into
  a shared folder and linked into the chosen guide's user skills folder. A new
  clone uses a staging directory before rename. Later runs fast-forward it. An
  existing skill folder that is not this installer's link is kept.
- An existing studio folder is reused when it has `knowledge/` or `.studio-setup/`.
  Its files are preserved. Other non-empty folders are refused.

## Sources

| Component | Installation source |
|---|---|
| Gum 2.0.1 | [Official GitHub release](https://github.com/charmbracelet/gum/releases/tag/v2.0.1), with checksum verification. |
| GitHub CLI 2.101.0 | [Official GitHub release](https://github.com/cli/cli/releases/tag/v2.101.0), with checksum verification. |
| Git | Apple command line tools on macOS, or the Linux system package manager. |
| OpenClaw | [Official local-prefix installer](https://docs.openclaw.ai/install/installer), with `--no-onboard`. It manages its private Node runtime. |
| Codex | [Official standalone installer](https://developers.openai.com/codex/cli), with non-interactive installation. |
| Claude Code | [Official native installer](https://code.claude.com/docs/en/setup). |
| `openclaw-studio` skill | [Arlington-AI-Advisors/skills](https://github.com/Arlington-AI-Advisors/skills), `main` unless `STUDIO_REF` is set. |

Gum and GitHub CLI versions are pinned in the script. AI tools and a new OpenClaw
installation follow their official installer defaults. The onboarding guide records
the OpenClaw version, researches it, and pins its configuration work in Phase 2.
No model identifier or provider credential is hardcoded.

## Local paths

| Path | Contents |
|---|---|
| `~/.local/bin/gh` | New GitHub CLI binary. |
| `~/.local/share/ai-studio/openclaw/` | New OpenClaw install and its private Node runtime. |
| `~/.local/share/ai-studio/logs/` | Installation output. Login sessions are not logged. |
| `~/.local/share/ai-studio/skills/` | Clone of the skills repository. |
| `~/.claude/skills/openclaw-studio` or `~/.agents/skills/openclaw-studio` | Link to the skill for Claude Code or Codex. |
| `<studio>/knowledge/` | The user's company documents. |
| `<studio>/.studio-setup/context.txt` | Detected tool versions and skill commit. No secrets. |
| `<studio>/.studio-setup/start` | Resume launcher with the tool paths and quoted studio directory. |

The script uses a private umask. `.studio-setup/` sits outside the `studio/` Git
repository that onboarding creates. AI guides keep
their own files in their vendor-managed locations. The resume launcher sets its
own PATH, so it works without editing the user's shell profile for OpenClaw or `gh`.

## Verify changes locally

Run `bash -n install.sh` and `shellcheck install.sh`, then try `bash install.sh --preview --plain` and
`bash install.sh --preview` in a real terminal. Preview does not install dependencies,
install the skill, create a studio, or launch an AI session. The styled preview downloads Gum temporarily.

Check both guide choices, a folder with spaces, keyboard cancellation, the
numbered fallback, and the piped entry point. Use disposable machines for full
installation checks. Do not install or sign in to AI tools on a development host
just to exercise the UI. Change the onboarding process in the skills repository,
not here.

To review a skills branch end to end, set `STUDIO_REF` to that branch when running
the installer. The default is `main`.
macOS and Windows/WSL require platform checks before claiming a verified release.
