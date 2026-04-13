# External Surface Audit

Reusable AI coding-agent skill for external application security reviews focused on publicly reachable data exposure, weak backend authorization, insecure onboarding and invite flows, leaked internal assets, and other low-impact black-box findings.

## What It Does

This skill helps audit public websites, panels, SaaS apps, and landing pages without authenticated access.

It is designed to help identify:

- publicly exposed data
- weak anonymous access controls
- insecure invite, activation, and recovery flows
- leaked reports, files, and internal assets
- exposed backend integrations in frontend bundles
- weak public APIs, RPCs, GraphQL surfaces, and storage
- weak security headers and overly broad public surfaces

It is not meant for destructive testing, brute force, privilege escalation, denial of service, or exploit chaining.

## Skill Structure

```text
external-surface-audit/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    ├── report-template.md
    └── scan-patterns.md
```

## Installation

This skill is intended to be distributed publicly via GitHub.

You can install it in Codex, Claude Code, or any similar environment that supports file-based skills.

Example shape:

- repo: `<owner>/<repo>`
- path: `skills/external-surface-audit`

If you are publishing this skill inside a repository dedicated to the skill, the install path may simply be the repository root.

### Codex

Install or copy the folder into:

```text
~/.codex/skills/external-surface-audit
```

### Claude Code

Install or copy the folder into:

```text
~/.claude/skills/external-surface-audit
```

The entry file remains:

```text
SKILL.md
```

## When To Use

Use this skill for requests like:

- "audit this app"
- "check whether this site leaks data"
- "review this panel for exposed information"
- "test this public app for weak anonymous access"

## Output Style

Typical outputs:

- a short findings summary
- a full markdown report for developers
- masked proof examples
- a prioritized remediation checklist
- an optional visual HTML summary with actions to take

After the main audit, the skill should ask the user whether they want an HTML deliverable with:

- a visual summary of the analysis
- confirmed findings and inferred risks
- severity labels
- a checklist of actions to take

## Safety Model

This skill is built for passive or low-impact verification only.

Key guardrails:

- do not brute force tokens, passwords, or identifiers
- do not attempt privilege escalation
- do not mass-download datasets
- redact third-party PII by default
- use the minimum request needed to confirm a finding

## Notes For Maintainers

- keep the skill self-contained
- keep real client data out of examples
- prefer generic and masked samples only
- keep detailed playbooks in `references/` and the main workflow in `SKILL.md`
- keep the wording agent-neutral so the same skill works in Codex and Claude Code

## Credits

Created by [@danilohmaia](https://instagram.com/danilohmaia) on Instagram.
