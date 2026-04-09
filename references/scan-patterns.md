# Scan Patterns

Use these patterns as discovery aids. They are not findings by themselves.

## Initial Targets

- `/`
- `/robots.txt`
- `/sitemap.xml`
- known app routes like `/login`, `/signup`, `/invite`, `/accept-invite`, `/reset-password`
- main JS bundles under `/assets/`
- source maps if exposed

## Bundle Search Patterns

Search bundles for:

- `supabase.co`
- `firebase`
- `graphql`
- `api/`
- `rest/v1`
- `functions/v1`
- `storage`
- `bucket`
- `invite`
- `token`
- `activation`
- `reset`
- `admin`
- `role`
- `permission`
- `audit`
- `report`
- `export`
- `webhook`
- `mailgun`
- `kiwify`
- `hotmart`
- `stripe`

## Supabase Discovery

Look for:

- project URL
- anon key
- `from("table")`
- `rpc("function")`
- `storage.from("bucket")`
- `functions/v1/...`

Useful verification paths:

- `/rest/v1/<table>?select=*&limit=3`
- `/rest/v1/rpc/<function>`
- `/functions/v1/<function>`

## Firebase Discovery

Look for:

- `apiKey`
- `projectId`
- `authDomain`
- `storageBucket`
- Firestore collection names in code

## GraphQL Discovery

Look for:

- `/graphql`
- persisted queries
- operation names
- fragments for admin or internal types

## Storage and File Discovery

Look for:

- `reports/`
- `exports/`
- `uploads/`
- `backup/`
- `tmp/`
- `public/`
- `.csv`
- `.pdf`
- `.json`
- `.xlsx`

## Header Review

Check at minimum:

- `Strict-Transport-Security`
- `Content-Security-Policy`
- `X-Frame-Options` or `frame-ancestors`
- `Referrer-Policy`
- `Permissions-Policy`
- `X-Content-Type-Options`

Missing headers are usually hardening findings, not direct data exposure.

## Sensitive Data Categories

Escalate severity when exposed data includes:

- children or minors
- health data
- school or ministry attendance
- volunteer rosters
- emails and phones
- payment identifiers
- internal planning docs
- invite or recovery tokens
- admin actions or audit logs

## Validation Discipline

Prefer:

- `limit=1` or `limit=3`
- targeted field selection where possible
- masked examples in the final report

Avoid:

- bulk export
- repeated token trials
- write tests unless explicitly justified
