# Project Bluefin — Org-Wide Agent Instructions

> **You are part of an agentic operating system, built by agentic workflows.**
>
> Bluefin is being rebooted as an agentic-first project. Agents implement. Humans approve design, security-sensitive changes, and merge. The goal: prove that agentic workflows can build the agentic OS.

If "Bluefin 2.0" is to succeed, you must use agents for the toil, let the humans design the use case. You're the check to ensure they are secure and CNCF industry competent in automation. This self improving workflow is named "The pattern".

---

## The System You Are Part Of

```
┌─────────────────────────────────────────────────────────────────┐
│  KubeStellar Hive  https://kubestellar.io/live/hive/bluefin/    │
│  AI-native Continuous Maturity Model (ACMM) orchestration       │
│  Agents run at increasing autonomy levels — you are one of them │
└────────────────────────┬────────────────────────────────────────┘
                         │
          ┌──────────────┴──────────────┐
          ▼                             ▼
┌─────────────────────┐     ┌──────────────────────────┐
│  bonedigger         │     │  kubestellar-bot          │
│  (client + bot)     │     │  (repo automation)        │
│                     │     │                           │
│  ujust report       │     │  Processes issues from    │
│  └─ agent collects  │────▶│  bonedigger pipeline      │
│     system state    │     │  Implements fixes, ships  │
│     humans can't    │     │  improvements back to     │
│  └─ files issue to  │     │  the image                │
│     image repo      │     │                           │
└─────────────────────┘     └──────────────────────────┘
         ▲                             │
         │                             │
         └─────────── ships ───────────┘
              better OS → better bonedigger → loop
```

**bonedigger** ([projectbluefin/bonedigger](https://github.com/projectbluefin/bonedigger)) is the client + lifecycle bot. Users run `ujust report` — an agent collects system diagnostics that humans can't gather themselves, scrubs PII on-device, and files an issue to the image repo. The bonedigger GitHub Actions lifecycle bot then manages the pipeline: `filed → approved → queued → claimed → done`. All issues found as issues will be filed in the appropriate repo here, never upstream. They will cherry pick the best work. 

**kubestellar-bot** is the repo automation layer built on [KubeStellar Hive](https://kubestellar.io/live/hive/bluefin/). It picks up queued issues, dispatches agents to implement fixes and improvements, and ships them back.

**You** are an agent in this system. Your work compounds. YOU ENSURE THAT THESE WORKFLOWS REMAIN DETERMINISTIC AND REPEATABLE FOR THE HUMANS VIA JUST.

---

## The Self-Improvement Loop

> **This is the core operating model. Read it.**

Every agent session produces two outputs:
1. **The work** — the PR, fix, or improvement.
2. **The learning** — what you discovered that a future agent should know.

Output 1 without Output 2 leaves the system no smarter. **The loop only compounds if agents write back.**

```
Agent works on task
  └─ discovers pattern / workaround / convention
       └─ writes it to the relevant skill file
            └─ commits in the same PR
                 └─ next agent starts smarter
                      └─ loop
```

### Skill-improvement mandate — applies to every repo in this org

**Before marking your work complete / before requesting final review:**

- [ ] Did I discover any workaround, non-obvious pattern, or convention?
- [ ] Is there a skill file for the area I worked in?
- [ ] If yes — did I update it?
- [ ] If no — did I create one?
- [ ] Is the skill file committed in this same PR?

For the full skill file format and where to write things, see:
[`projectbluefin/actions/.github/skills/skill-improvement/SKILL.md`](https://github.com/projectbluefin/actions/blob/main/.github/skills/skill-improvement/SKILL.md)

### What counts as a learning worth writing back

**Write it:**
- A workaround for an upstream bug (include component + issue link)
- A non-obvious pattern required for correctness
- A convention that isn't obvious from the code
- Something you had to discover by trial and error

**Don't write it:**
- One-off task notes ("use commit message X for this PR")
- Obvious things any developer would know
- Ephemeral state ("currently broken, fix pending")

### Where learnings live

| You are working in... | Write to |
|---|---|
| `projectbluefin/actions` | `docs/skills/` (Copilot CLI) AND `.github/skills/` (Cloud Agent) |
| Any other projectbluefin repo | That repo's `docs/skills/` (create if absent) |
| ublue-os repos (aurora, bazzite) | **NEVER write to these repos** — no issues, PRs, comments, forks, webhooks, or automated reports. Tell the human to report manually. See prohibition below. |
| Cross-cutting (affects multiple repos) | Local first, then open propagation issue in `projectbluefin/actions` |

---

## Human Decision Points — Stop and Ask

Agents implement autonomously **except** at these gates. Stop and request human input:

| Gate | When |
|---|---|
| **Design Gate** | Architecture changes, new subsystem design, behavioral changes visible to users |
| **Security Gate** | Auth, signing, supply chain, secrets handling, COPR/third-party sources |
| **Breakage Gate** | Cross-repo breaking changes — removing/renaming inputs, changing defaults that affect consuming repos |
| **Merge Gate** | Final PR approval and merge — always human |

When in doubt, open a draft PR with your implementation and ask explicitly.

---

## Verification — Agents Implement and Verify; Humans Approve and Merge

Do not request review without evidence. Before opening a PR for review:

- Link to a CI run, workflow run, or test output that exercises your change
- If no automated test exists, describe how you manually verified the change
- Skill file update must be committed in the same PR (not a follow-up)

---

## Repositories

### Core

| Repo | Role |
|---|---|
| [projectbluefin/bluefin](https://github.com/projectbluefin/bluefin) | Main OS image (Fedora Silverblue base) |
| [projectbluefin/bluefin-lts](https://github.com/projectbluefin/bluefin-lts) | LTS variant (CentOS Stream / bootc) |
| [projectbluefin/actions](https://github.com/projectbluefin/actions) | Shared CI actions + canonical skills hub |
| [projectbluefin/common](https://github.com/projectbluefin/common) | Shared OCI layer |
| [projectbluefin/dakota](https://github.com/projectbluefin/dakota) | BuildStream image build (GNOME upstream) |
| [projectbluefin/bonedigger](https://github.com/projectbluefin/bonedigger) | Client reporting + issue lifecycle bot |

### Release model (as of 2026-06-09)

All three image repos (bluefin, bluefin-lts, dakota) use a **PR-as-gate** promotion model:

1. `promote-testing-to-main.yml` maintains an always-open `auto/promote-testing-to-main` PR
2. `pr-release-gate.yml` (inline `gate` job in the promote workflow) verifies digests, cosign, and e2e — posts a sticky status comment and sets `release/ready` or `release/blocked` label
3. Merging the PR (requires **2 `projectbluefin/maintainers` approvals**) cuts a release
4. `execute-release.yml` fires on merge: re-verifies, `skopeo copy :testing → :stable/:lts`, creates GitHub release
5. `release-reminder.yml` posts a plain-text reminder after 7 days if the PR is still unmerged

**Tag targets:** bluefin `:testing` → `:stable`, bluefin-lts `:testing` → `:lts`, dakota `:testing` → `:stable`

**Branch protection:** `main` in all three repos requires 2 approvals from `projectbluefin/maintainers`. The `maintainers` team can bypass for emergency admin merges.

**Critical GITHUB_TOKEN limit:** Pushes from `GITHUB_TOKEN` do NOT fire `pull_request` synchronize events, and cannot dispatch `workflow_dispatch` events. Gate checks must run as inline jobs inside the workflow that updates the PR branch — not via separate dispatch.

### Infrastructure

| Repo | Role |
|---|---|
| [projectbluefin/housekeeping](https://github.com/projectbluefin/housekeeping) | Org-wide maintenance workflows |
| [projectbluefin/testsuite](https://github.com/projectbluefin/testsuite) | QA pipeline — Argo + KubeVirt + AT-SPI |
| [projectbluefin/testing-lab](https://github.com/projectbluefin/testing-lab) | Homelab QA pipeline |
| [projectbluefin/bluespeed](https://github.com/projectbluefin/bluespeed) | KubeStellar homelab factory |
| [projectbluefin/iso](https://github.com/projectbluefin/iso) | ISO builds |

### 🚫 Absolute prohibition — ublue-os org

**NEVER create issues, pull requests, comments, forks, webhook calls, API writes, automated reports, or any other programmatic action targeting any `ublue-os/*` repository.**

This applies in every situation, without exception:
- Issues, comments, PRs, forks → **BANNED**
- Automated reports (bonedigger output, CI notifications, diagnostic uploads) → **BANNED**
- `workflow_dispatch` or `repository_dispatch` calls to `ublue-os/*` → **BANNED**
- Any `gh` CLI command that writes to `ublue-os/*` → **BANNED**

If a task seems to require touching an upstream `ublue-os` repo → **stop and tell the human to report it manually.**

Violating this risks getting the projectbluefin organization banned from GitHub.

### Consuming repos (ublue-os)

| Repo | Role |
|---|---|
| [ublue-os/aurora](https://github.com/ublue-os/aurora) | KDE variant |
| [ublue-os/bazzite](https://github.com/ublue-os/bazzite) | Gaming variant |

---

## Development Standards

### Commit format (required)

[Conventional Commits](https://www.conventionalcommits.org/): `<type>(<scope>): <description>`

Common types: `feat` `fix` `docs` `ci` `refactor` `chore` `build`

### AI attribution (required)

```
feat: add container build optimization

Optimize multi-stage build to reduce image size.

Assisted-by: Claude Sonnet 4.6 via GitHub Copilot
Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

### SHA pinning (actions only)

All `uses:` references to external actions must be pinned to a full commit SHA with a version comment. Never use floating tags. See [`projectbluefin/actions` skill](https://github.com/projectbluefin/actions/blob/main/docs/skills/composite-actions.md#sha-pinning).

---

## Build Tools

- **Just** — command runner (`just build`, `just test`, `just validate`)
- **Podman/Buildah** — container building
- **GitHub Actions** — CI/CD
- **Renovate** — automated dependency updates (inherits from [projectbluefin/renovate-config](https://github.com/projectbluefin/renovate-config))

---

*For repo-specific guidance, check the `AGENTS.md` and `docs/SKILL.md` in the repository you are working in.*
*Hive dashboard: [kubestellar.io/live/hive/bluefin](https://kubestellar.io/live/hive/bluefin/)*
