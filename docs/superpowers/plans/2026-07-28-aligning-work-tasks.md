# Aligning Work Tasks Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build, validate, document, and publish a general-purpose Codex Skill that converts ambiguous workplace handoffs into executable, evidence-backed, safely reportable work.

**Architecture:** Keep the installable Skill small: `SKILL.md` owns the universal decision workflow, while `references/domain-lenses.md` holds optional industry fields. Keep human-facing guidance, research, and evaluation cases at repository level so they do not inflate Skill context or violate Skill packaging rules.

**Tech Stack:** Markdown, YAML, Codex Skill format, Python-based `init_skill.py` and `quick_validate.py`, Git, GitHub CLI.

---

## File Map

- `README.md`: Chinese user guide, installation, prompts, progress tracking, image evidence, privacy, and limitations.
- `docs/research.md`: Cross-industry research synthesis and cited sources.
- `docs/superpowers/specs/2026-07-28-aligning-work-tasks-design.md`: Approved design.
- `docs/superpowers/plans/2026-07-28-aligning-work-tasks.md`: This execution plan.
- `evals/README.md`: Evaluation method and scoring rules.
- `evals/cases.md`: Baseline and forward-test scenarios with expected behaviors.
- `skills/aligning-work-tasks/SKILL.md`: Universal task-alignment workflow and safety gates.
- `skills/aligning-work-tasks/agents/openai.yaml`: Skill list metadata and default invocation.
- `skills/aligning-work-tasks/references/domain-lenses.md`: ERP, new media, development, sales, finance, service, construction, healthcare, HR, procurement, and legal fields.

### Task 1: Preserve RED Baselines and Define Evals

**Files:**
- Create: `evals/README.md`
- Create: `evals/cases.md`

- [ ] **Step 1: Record the no-Skill baseline observations**

Write the three observed behaviors without inventing transcripts:

```markdown
- ERP: handled secrets and environment caution well, but did not define four mutually exclusive statuses or a complete evidence rubric.
- New media: produced a useful plan, but silently invented a rule that no response meant approval to publish.
- Clear task: correctly executed without unnecessary clarification, establishing the anti-overengineering baseline.
```

- [ ] **Step 2: Define seven evaluation cases**

Include ERP verification, urgent social publishing, clear file merge, mid-task scope change, insufficient screenshot evidence, exposed credentials, and no writable task table. Each case must state input, expected behavior, prohibited behavior, and pass conditions.

- [ ] **Step 3: Verify every design risk has a case**

Run:

```powershell
Select-String -LiteralPath evals/cases.md -Pattern 'ERP','新媒体','清晰任务','需求变更','证据不足','密码','不可写'
```

Expected: all seven patterns are present.

- [ ] **Step 4: Commit the RED artifacts**

```powershell
git add evals/README.md evals/cases.md
git commit -m "test: define task alignment evals"
```

### Task 2: Initialize the Skill and Implement the Universal Workflow

**Files:**
- Create: `skills/aligning-work-tasks/SKILL.md`
- Create: `skills/aligning-work-tasks/agents/openai.yaml`
- Create: `skills/aligning-work-tasks/references/domain-lenses.md`

- [ ] **Step 1: Initialize with the official generator**

Run:

```powershell
& 'C:\Users\陈木关\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' `
  'C:\Users\陈木关\.codex\skills\.system\skill-creator\scripts\init_skill.py' `
  aligning-work-tasks `
  --path 'skills' `
  --resources references `
  --interface 'display_name=任务对齐' `
  --interface 'short_description=把模糊职场任务转成可执行、可验证、可安全汇报的工作交接' `
  --interface 'default_prompt=使用 $aligning-work-tasks 对齐这项工作，指出关键信息缺口并生成可执行、可验证的交接结果。'
```

Expected: the three Skill paths exist and no extra resource directories are created.

- [ ] **Step 2: Replace the generated SKILL.md with the minimal behavior needed to pass RED**

Use this trigger-only frontmatter:

```yaml
---
name: aligning-work-tasks
description: Use when a workplace request, delegation, handoff, status update, verification task, or approval is vague, incomplete, conflicting, difficult to execute, or at risk of information loss across managers, employees, teams, or shifts.
---
```

The body must require: sensitive-data redaction, facts/gaps/conflicts/assumptions separation, minimal task contract, three-way execution gate, per-item checkboxes, evidence criteria, four-state classification, real file updates only when tools allow, concise upward reporting, and no over-questioning for clear low-risk work.

- [ ] **Step 3: Add one compact end-to-end example**

Use a generic issue-verification example showing an ambiguous message becoming: three blocking questions, a project checklist, required condition/result evidence, status, table delta, and upward report. Do not use real company names or credentials.

- [ ] **Step 4: Check generated UI metadata**

Run:

```powershell
Get-Content -Raw skills/aligning-work-tasks/agents/openai.yaml
```

Expected: quoted `display_name`, `short_description`, and a `default_prompt` explicitly containing `$aligning-work-tasks`.

- [ ] **Step 5: Commit the minimal GREEN Skill**

```powershell
git add skills/aligning-work-tasks
git commit -m "feat: add task alignment skill"
```

### Task 3: Add Progressive Industry Lenses

**Files:**
- Modify: `skills/aligning-work-tasks/references/domain-lenses.md`

- [ ] **Step 1: Add the shared reference format**

For every domain use exactly four headings: `常见缺口`, `补充字段`, `证据`, `高风险门禁`.

- [ ] **Step 2: Add ten lenses**

Cover ERP implementation, new media, software development and testing, sales, finance, customer service, construction, healthcare, HR, and procurement/legal. Keep each lens concise and avoid explaining the universal workflow again.

- [ ] **Step 3: Link conditional loading from SKILL.md**

Require reading `references/domain-lenses.md` only when domain-specific fields materially affect clarification or acceptance.

- [ ] **Step 4: Verify every lens contains the shared headings**

Run:

```powershell
$text = Get-Content -Raw skills/aligning-work-tasks/references/domain-lenses.md
@('ERP','新媒体','软件','销售','财务','客服','工程','医疗','人力','采购') | ForEach-Object { if ($text -notmatch $_) { throw "Missing lens: $_" } }
```

Expected: exit code 0 with no missing-lens error.

- [ ] **Step 5: Commit the industry lenses**

```powershell
git add skills/aligning-work-tasks
git commit -m "docs: add industry task lenses"
```

### Task 4: Write the GitHub Guide and Research Note

**Files:**
- Create: `README.md`
- Create: `docs/research.md`

- [ ] **Step 1: Write README installation and quick start**

Document installation from `skills/aligning-work-tasks`, explicit invocation with `$aligning-work-tasks`, a first-use prompt, and expected output types.

- [ ] **Step 2: Write evidence-image guidance**

State: upload enough evidence, not every screenshot; pair query-condition and result images; number files; crop unrelated UI; redact credentials, personal data, bank details, customer information, and production-only identifiers.

- [ ] **Step 3: Write progress-follow-up guidance**

Tell users to keep one task in one conversation, preserve item IDs, report what changed, attach new evidence, and ask for a table delta after each round. Explain that a new conversation loses unprovided state.

- [ ] **Step 4: Add three complete usage examples**

Include receiving an ambiguous task, assigning a task clearly, and validating returned evidence. Ensure external publishing and production modification still require explicit approval.

- [ ] **Step 5: Write the cross-industry research note**

Summarize the shared task-contract pattern and cite HubSpot creative briefs, GitHub issue forms, Procore RFIs, the 2026 SBAR/ISBAR systematic review, BlackLine financial close guidance, and the user's anonymized ERP pattern without naming people or companies.

- [ ] **Step 6: Commit repository documentation**

```powershell
git add README.md docs/research.md
git commit -m "docs: add usage guide and research"
```

### Task 5: VERIFY GREEN and REFACTOR

**Files:**
- Modify if needed: `skills/aligning-work-tasks/SKILL.md`
- Modify if needed: `skills/aligning-work-tasks/references/domain-lenses.md`
- Modify: `evals/README.md`
- Modify: `evals/cases.md`

- [ ] **Step 1: Run the seven cases with the Skill**

Use independent agents with only the raw case and Skill path. Record whether each required and prohibited behavior occurred; do not give expected answers to the agents.

- [ ] **Step 2: Compare with RED failures**

The ERP case must use all four statuses and evidence rules. The media case must not treat silence as publication approval. The clear task case must stay concise and proceed without unnecessary questions.

- [ ] **Step 3: Refactor only observed gaps**

Add explicit counters for any new unsafe assumption, status ambiguity, evidence shortcut, false persistence claim, or over-questioning observed during forward tests.

- [ ] **Step 4: Re-run failed cases**

Expected: all seven cases pass without introducing new failures in previously passing cases.

- [ ] **Step 5: Record the final results and commit**

```powershell
git add evals skills/aligning-work-tasks
git commit -m "test: validate task alignment behavior"
```

### Task 6: Validate, Audit, and Publish

**Files:**
- Modify only if validation finds an issue.

- [ ] **Step 1: Run official Skill validation**

```powershell
& 'C:\Users\陈木关\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' `
  'C:\Users\陈木关\.codex\skills\.system\skill-creator\scripts\quick_validate.py' `
  'skills/aligning-work-tasks'
```

Expected: `Skill is valid!`

- [ ] **Step 2: Scan repository text**

```powershell
Get-ChildItem -Recurse -File | Where-Object { $_.FullName -notmatch '\\.git\\' } | Select-String -Pattern 'TODO','TBD','冬姐','科林','password=','token=','secret='
```

Expected: no placeholders, real names, or credential assignments. Generic security guidance mentioning words such as password is acceptable after manual review.

- [ ] **Step 3: Check repository status and history**

```powershell
git status --short
git log --oneline --decorate -8
```

Expected: clean worktree and commits for design, evals, Skill, industry lenses, docs, and validation.

- [ ] **Step 4: Verify GitHub authentication**

```powershell
& 'C:\Program Files\GitHub CLI\gh.exe' auth status
```

Expected: authenticated to `github.com` as the user's account. If not authenticated, stop and request browser authorization; do not request or store a personal access token in project files.

- [ ] **Step 5: Create and push the private repository**

```powershell
& 'C:\Program Files\GitHub CLI\gh.exe' repo create aligning-work-tasks --private --source . --remote origin --push
```

Expected: a private GitHub repository URL and `main` tracking `origin/main`.

- [ ] **Step 6: Final verification**

```powershell
git status --short
& 'C:\Program Files\GitHub CLI\gh.exe' repo view --json nameWithOwner,url,visibility,defaultBranchRef
```

Expected: clean status, repository name `aligning-work-tasks`, visibility `PRIVATE`, and default branch `main`.
