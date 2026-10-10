# Initial GPT-5.5 Company Setup

Date: 2026-10-09
Fork: `MirkoGonzalez22/paperclip`
Branch: `feature/initial-company-setup`
Local checkout: `C:\Users\Nicodh\Documents\Codex\pc-paperclip`

## Verified local runtime

- Node.js: `v24.11.0` from the portable Windows runtime.
- Package manager: `pnpm@9.15.4` via Corepack.
- Paperclip state:
  - `PAPERCLIP_HOME=C:\Users\Nicodh\Documents\Codex\paperclip-data`
  - `PAPERCLIP_INSTANCE_ID=initial-company-setup`
- Local UI: `http://127.0.0.1:3100`
- Local API health: `http://127.0.0.1:3100/api/health`
- Database: embedded PostgreSQL under the Paperclip instance directory.

## Codex and GPT-5.5 auth

Codex CLI is installed and authenticated with ChatGPT on this machine.

Smoke test used:

```powershell
codex exec -m gpt-5.5 --skip-git-repo-check "Respond with exactly: codex-gpt-5.5-ok"
```

Expected output:

```text
codex-gpt-5.5-ok
```

The initial agents use `adapterType: "codex_local"` and:

```json
{
  "engine": "cli",
  "command": "codex",
  "cwd": "C:\\Users\\Nicodh\\Documents\\Codex\\pc-paperclip",
  "model": "gpt-5.5",
  "modelReasoningEffort": "high",
  "timeoutSec": 180,
  "fastMode": false
}
```

No OpenAI API key was stored in the agent config. Paperclip isolates each `codex_local` agent home under the instance root and uses the local Codex login.

## Created company

- Company: `Paperclip AI Company`
- Company id: `595ff8ee-912f-467a-be91-273852b49626`
- Budget: 2500 cents/month
- New-agent creation: board approval required after bootstrap.
- Confirmation governance: human-only for confirmation, checkbox confirmation, and item verdict interactions.

## Initial org chart

| Agent | Role | Reports to | Trust |
| --- | --- | --- | --- |
| Sofia CEO | CEO | Board | standard |
| Mateo Tech Lead | CTO / Tech Lead | Sofia CEO | standard |
| Valentina Backend | Backend Engineer | Mateo Tech Lead | low_trust_review |
| Lucas Frontend | Frontend Engineer | Mateo Tech Lead | low_trust_review |
| Camila QA | QA Engineer | Mateo Tech Lead | low_trust_review |

Each agent has a managed `AGENTS.md` instruction bundle telling it to use GPT-5.5, work in small reviewable steps, coordinate through Paperclip issues, and require human approval for secrets, spend, destructive changes, production deploys, and external publishing.

## Demo project

- Goal: `Launch an autonomous task-manager demo`
- Project: `Demo Task Manager App`
- Issue: `PAP-1`, `Plan the first demo slice and delegate implementation tasks`
- Assignee: Sofia CEO
- Workspace: local Paperclip fork checkout

The first automatic assignment run failed before the trust policy correction because the CEO was initially `low_trust_review` without a concrete boundary. The CEO and Tech Lead were then moved to `standard` trust so they can coordinate the company; worker agents remain low-trust.

A manual CEO wakeup was then launched for `PAP-1`. Paperclip started a real `codex_local` run using `gpt-5.5` and ChatGPT subscription billing metadata, and the run log shows the agent reading Paperclip API references and issue state. The run timed out after the configured 180 second adapter limit before completing. For a full autonomous demo, raise `adapterConfig.timeoutSec` on coordinator agents, or give the first issue a narrower instruction such as "post a three-bullet plan only."

## GitHub flow

- Origin: `https://github.com/MirkoGonzalez22/paperclip`
- Upstream: `https://github.com/paperclipai/paperclip.git`
- Active branch: `feature/initial-company-setup`
- Upstream push URL is disabled locally to avoid accidental pushes to the source repository.

Recommended task flow:

1. Create or assign a Paperclip issue.
2. Let the responsible agent work in the project workspace.
3. Require human review for secrets, spend, destructive operations, production deploys, external publication, or dependency/security-sensitive changes.
4. Commit to a feature branch in the fork.
5. Open a PR from the fork branch.

## Future pixel office integration

The pixel office should be a read/write visualization layer over existing Paperclip state, not a separate source of truth.

Suggested mapping:

- Company -> office building.
- Agent -> avatar at a desk.
- Agent status -> avatar state: idle, running, blocked, review, error.
- Issue -> work item bubble or desk queue.
- Project -> room or floor.
- Heartbeat run -> animated work session.
- Human approval request -> visible review prompt.

Suggested API/event surface:

- Read org chart from `/api/companies/:companyId/org`.
- Read agents from `/api/companies/:companyId/agents`.
- Read issues from `/api/companies/:companyId/issues`.
- Read live work from `/api/companies/:companyId/live-runs`.
- Use future websocket/server-sent events for low-latency avatar updates if available; otherwise poll live runs and issue state.

Keep the pixel layer bounded:

- It should not bypass Paperclip permissions.
- It should not store secrets.
- It should only trigger existing Paperclip actions such as wakeup, assign, comment, approve, or reject.
- Destructive actions should still require Paperclip confirmation flows.
