---
name: external-surface-audit
description: Reusable playbook for external application security reviews focused on publicly reachable data exposure, weak backend authorization, insecure onboarding and invite flows, leaked internal assets, and other passive or low-impact findings across Supabase and non-Supabase stacks. Use when auditing a public website, panel, SaaS app, or landing page for exposed data or weak public surfaces without authenticated access.
---

# External Surface Audit

Use this skill for black-box reviews of public applications where the goal is to identify what an anonymous visitor can learn or access without login.

Typical trigger phrases:

- "audit this app"
- "check if this site leaks data"
- "see whether anything is exposed publicly"
- "review this panel for data exposure"
- "test this website for public vulnerabilities"

This skill is optimized for:

- public data exposure
- weak authorization on public APIs
- insecure invite and onboarding flows
- leaked internal assets and reports
- exposed backend integrations in frontend bundles
- weak security headers and overly broad public surfaces

This skill is not for destructive testing, brute force, exploit chaining, denial of service, credential stuffing, or bypass attempts.

## Guardrails

Only perform passive or low-impact verification.

- Do not brute force tokens, passwords, or identifiers.
- Do not attempt privilege escalation.
- Do not mass-download datasets.
- Do not mutate data unless the user explicitly asks for a safe write test and the risk is low.
- Do not expose real third-party PII in the response. Redact examples by default.
- Prefer the minimum request needed to confirm a finding.
- Treat onboarding, invite, recovery, magic-link, and payment flows as sensitive even if they seem public by design.

If the user has not clearly asked for an audit or does not appear authorized to request one, pause and ask for confirmation before proceeding.

## What To Look For

Prioritize these classes of issues:

1. Public reads of sensitive data
2. Public access to invite tokens, recovery tokens, or onboarding state
3. Public access to internal reports, exports, dashboards, docs, or files
4. Public RPCs, edge functions, GraphQL resolvers, REST endpoints, or admin routes that leak data
5. Public storage buckets or object URLs with sensitive files
6. Source maps or bundles that reveal internal routes, schemas, keys, flags, or backend references
7. Weak security headers, overly broad CORS, or framing exposure
8. Search or indexing endpoints leaking operational or user data
9. Misconfigured realtime, websocket, or broadcast channels
10. Verbose error messages that reveal internals

## Default Workflow

### 1. Fingerprint the app

Start with the public landing route and identify:

- framework and hosting
- JS and CSS bundles
- API base URLs
- auth provider
- backend platform
- analytics and third-party integrations
- visible routes and feature areas

Useful first checks:

- home HTML
- headers
- `robots.txt`
- `sitemap.xml`
- main bundle names

### 2. Inspect frontend artifacts

Search the bundle for:

- backend URLs
- API keys meant for public clients
- table and collection names
- route names
- edge function names
- GraphQL operations
- storage bucket names
- admin feature flags
- onboarding and invite flow strings

If source maps are public, inspect them carefully, but do not assume source maps alone are a vulnerability. Treat them as a discovery aid.

For search patterns and platform-specific indicators, read [scan-patterns.md](references/scan-patterns.md).

### 3. Enumerate public surfaces

Map the externally reachable surfaces before testing them:

- REST endpoints
- GraphQL endpoints
- RPC or function endpoints
- WebSocket or realtime endpoints
- storage buckets and object paths
- public files and report directories
- exports and generated reports
- invite, reset, signup, and activation routes

Start with read-only probes that confirm whether the surface exists and whether anonymous access is allowed.

### 4. Verify backend authorization

For each interesting surface, verify:

- anonymous read behavior
- response shape
- whether errors indicate proper authorization checks
- whether results are empty because of policy or simply because the table has no rows
- whether public data can be joined to more sensitive data

If a function is intended to be admin-only, verify that anonymous access is denied.

### 5. Test sensitive workflow edges

Pay special attention to:

- invite flows
- password reset
- account activation
- organization switching
- exports
- payment and subscription status
- team administration
- role and permission models

The most important question is:

"What can an anonymous visitor infer or access about real users, organizations, or internal operations?"

### 6. Classify impact

When you confirm a finding, classify what is exposed:

- PII: names, emails, phones, addresses, birthdays
- child data or school/ministry data
- team or volunteer data
- payment or subscription data
- private content or internal IP
- internal planning documents
- audit or operations logs
- tokens or identifiers that can alter a sensitive flow

### 7. Report minimally but clearly

Use redacted examples.

- Show enough structure to prove the issue.
- Mask emails, phones, tokens, and names by default.
- Avoid copying full documents or large tables into the answer.

Use the report structure in [report-template.md](references/report-template.md).

## Platform-Specific Notes

### Supabase

Common external checks:

- exposed project URL and anon key in frontend bundle
- REST tables via `/rest/v1/...`
- RPCs via `/rest/v1/rpc/...`
- edge functions via `/functions/v1/...`
- storage buckets and public object URLs
- realtime or broadcast references

Important interpretation rule:

An exposed `anon key` is normal. The vulnerability is weak policy, weak grants, or an overly permissive public function.

### Firebase and Firestore

Look for:

- `apiKey`, `projectId`, `authDomain`, `storageBucket`
- Firestore collection names and query paths in frontend code
- open security rules assumptions
- public storage references

### Hasura or GraphQL backends

Look for:

- introspection enabled publicly
- verbose schema exposure
- public queries returning sensitive types
- weak role handling via headers or unauthenticated defaults

### Appwrite, Directus, Strapi, PocketBase, Xano, Parse, custom REST

Look for:

- public collection reads
- public file storage
- admin route exposure
- verbose metadata or schema endpoints
- public webhooks or logs

### Search and document systems

Look for:

- Algolia or Meilisearch indices
- Elasticsearch endpoints
- public PDFs, CSVs, reports, backups, and exports
- `reports/`, `exports/`, `backup/`, `uploads/`, `files/`, `public/` paths

## Heuristics That Often Pay Off

- Public onboarding often leaks invite or activation state.
- Team management features often reference role tables that reveal account structure.
- Exports and reports are frequently forgotten in public web roots.
- Empty table responses do not prove safety.
- Client-side "hidden" routes often reveal backend names worth checking.
- Public "profile" or "ranking" endpoints often over-return fields.
- Internal reporting pages often live under predictable directories.

## Evidence Standard

Call a finding confirmed only when you have direct evidence, such as:

- a successful anonymous response with sensitive fields
- a direct public file retrieval
- a function response showing weak authorization

Call something a risk or suspicion when it is only inferred from the bundle or route structure.

Make that distinction explicit in the write-up.

## Response Style

When answering the user:

- lead with confirmed findings
- keep examples masked
- separate confirmed exposure from inferred risk
- note what was tested and what was not
- recommend specific next actions for the developer

## Deliverables

Typical deliverables are:

- short findings summary in chat
- full audit markdown for developers
- masked proof examples
- prioritized remediation checklist

If the user asks for a reusable artifact, generate a structured markdown report using the template in [report-template.md](references/report-template.md).

## Distribution Notes

Keep this skill self-contained for public reuse:

- keep `SKILL.md` as the entry point
- keep detailed patterns in `references/`
- avoid embedding real client data, tokens, or copied findings
- prefer generic, masked examples only
