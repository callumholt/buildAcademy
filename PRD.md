# Product Requirements Document -- Outreach Generator

**Product:** Outreach Generator -- an AI-powered cold outreach copy generator
**Document type:** MVP Product Requirements Document (PRD)
**Status:** Approved for build (MVP)
**Version:** 1.0
**Last updated:** 23 June 2026
**Owner:** buildAcademy

---

## How to use this document

This PRD is the detailed, expanded version of the scoping you do in Week 1 of
buildAcademy. The manual's `README.md` scopes the app in five questions (the **SCOPE**
framework); this document takes that same thinking and writes it out in full -- the
problem, the users, every feature, the data, the AI, the architecture, and -- just as
importantly -- everything we deliberately leave out.

It has two jobs:

1. **Spec the app we build together** so anyone (you or the Cursor agent) knows exactly
   what "done" looks like.
2. **Model what a good MVP PRD looks like** so you can write one for your own app later.
   The structure here is reusable: swap the problem, users, features, data, and prompt,
   and you have a plan for any AI SaaS MVP.

A PRD is not a contract you follow blindly -- it is the thinking made visible so you can
prompt with intent and notice when you are drifting off-scope. Keep it lean, keep it
honest about what is out of scope, and update it when reality teaches you something.

---

## Table of contents

1. [TL;DR](#1-tldr)
2. [Problem and opportunity](#2-problem-and-opportunity)
3. [SCOPE summary](#3-scope-summary)
4. [Goals, non-goals, and success metrics](#4-goals-non-goals-and-success-metrics)
5. [Target users and personas](#5-target-users-and-personas)
6. [User stories](#6-user-stories)
7. [Product scope -- MVP feature set](#7-product-scope----mvp-feature-set)
8. [Functional requirements](#8-functional-requirements)
9. [AI and prompt specification](#9-ai-and-prompt-specification)
10. [Data model](#10-data-model)
11. [System architecture and tech stack](#11-system-architecture-and-tech-stack)
12. [Data flow](#12-data-flow)
13. [Non-functional requirements](#13-non-functional-requirements)
14. [Configuration and environment variables](#14-configuration-and-environment-variables)
15. [Out of scope and future roadmap](#15-out-of-scope-and-future-roadmap)
16. [Risks and mitigations](#16-risks-and-mitigations)
17. [Acceptance criteria -- definition of done](#17-acceptance-criteria----definition-of-done)
18. [Assumptions and open questions](#18-assumptions-and-open-questions)
19. [Glossary](#19-glossary)
20. [Appendix -- mapping to the cohort build order](#20-appendix----mapping-to-the-cohort-build-order)

---

## 1. TL;DR

Outreach Generator is a single-purpose web app: a user enters who they are reaching out
to and why, and the app uses an AI model to produce a personalised cold email (subject
+ body) and a short LinkedIn connection message. Generated copy can be saved and revisited
on a history page.

The MVP is intentionally small. It covers every fundamental layer of a modern web app --
a frontend UI, an AI integration, a database, version control, and deployment -- without
the production concerns (authentication, payments, rate limiting) that differ from one
product to the next. The point is a real, deployed, useful app that demonstrates the full
shape of an AI SaaS build.

**One-line value proposition:** turn five facts about a prospect into ready-to-send,
personalised outreach in seconds.

---

## 2. Problem and opportunity

### 2.1 The problem

Founders, salespeople, and recruiters know that personalised outreach dramatically
outperforms generic templates -- but personalising every message is slow. Writing a
genuinely tailored cold email and LinkedIn note for each prospect takes 5--15 minutes of
thinking about their role, company, and likely pain point. At any real volume, people
either:

- send generic copy that gets ignored, or
- spend hours per week writing one-off messages, or
- stop doing outreach altogether.

### 2.2 The opportunity

A modern AI model is very good at exactly this task: given structured context about a
prospect, it can produce conversational, specific, on-brand copy in seconds. The work for
the product is not the writing -- the model does that -- it is **capturing the right
context, giving the model a disciplined prompt, returning structured output the UI can
use, and storing the results** so the user builds a usable history.

This is the same pattern behind most AI products: collect input, send it to a model with
a well-designed prompt, receive structured output, do something useful with it, and
persist it.

---

## 3. SCOPE summary

The app, scoped against the buildAcademy **SCOPE** framework:

| Letter | Question | Answer for Outreach Generator |
| --- | --- | --- |
| **S -- Solve** | Who are we solving for? | People who do cold outreach: founders, salespeople, recruiters. |
| **C -- Challenge** | What problem does it solve? | Writing personalised cold emails and LinkedIn messages is slow. |
| **O -- One thing** | What must work or it is pointless? | The generated copy must be genuinely good and personalised. If the output is generic or low quality, nothing else matters. |
| **P -- Path** | Where does information come from and go? | User types five prospect details -> the app sends them to the AI -> the AI returns structured copy -> the user saves it to a database -> the user reads it back in history. |
| **E -- Exclude** | What are we deliberately leaving for later? | Authentication, multi-user accounts, payments, rate limiting, editing/searching history, sending email, CRM integrations. |

Everything below is an expansion of this table.

---

## 4. Goals, non-goals, and success metrics

### 4.1 Product goals (MVP)

1. Let a user generate high-quality, personalised outreach copy from five inputs.
2. Let a user copy that copy to the clipboard in one click.
3. Let a user save generated copy and view a history of everything they have generated.
4. Be deployed and usable on the public internet from any device, including mobile.

### 4.2 Non-goals (MVP)

The following are explicitly **not** goals for this version. They are not failures of
scope -- they are the scope. See [section 15](#15-out-of-scope-and-future-roadmap).

- Sign-up, login, or per-user data isolation.
- Charging money or any billing.
- Protecting the app from abuse (rate limiting, spend caps).
- Editing or deleting saved entries.
- Searching, filtering, or paginating history.
- Sending the email or message from within the app.

### 4.3 Success metrics

**Product metrics (if this were a real launch):**

| Metric | Definition | MVP target |
| --- | --- | --- |
| Activation | A new user generates at least one piece of copy | 80% of visitors who start the form |
| Generation success rate | Generations that return valid, displayable copy | >= 98% |
| Save rate | Generations that the user saves to history | >= 30% |
| Time to first result | Form submit -> copy on screen | < 10 seconds typical |

**Learning metrics (the real point of the cohort):**

- Each participant has a working app deployed on their own public URL.
- Each participant can explain how data flows through every layer.
- Each participant can fix a broken build without spiralling.

---

## 5. Target users and personas

### 5.1 Primary persona -- "Sam the founder"

- Runs a small B2B startup, does their own sales outreach.
- Comfortable describing a prospect in plain language; not a copywriter.
- Wants messages that sound human and specific, fast.
- Sends outreach in bursts (e.g. 10--20 prospects in a sitting).

### 5.2 Secondary persona -- "Riya the recruiter"

- Reaches out to candidates daily on email and LinkedIn.
- Needs warm, personalised first-touch messages at volume.
- Values a saved history so she can recall who she contacted and what she said.

### 5.3 Anti-persona (not designed for in MVP)

- Large sales teams needing shared workspaces, permissions, and CRM sync.
- Users who need guaranteed compliance/PII controls on stored prospect data.

---

## 6. User stories

Written as jobs-to-be-done. IDs are referenced by the functional requirements.

- **US-1:** As a user, I want to enter a prospect's details so the app has the context it needs to personalise copy.
- **US-2:** As a user, I want to generate a cold email and a LinkedIn message from those details so I do not have to write them myself.
- **US-3:** As a user, I want to see a clear loading state while the AI works so I know the app is doing something.
- **US-4:** As a user, I want to copy each piece of copy in one click so I can paste it into my email client or LinkedIn.
- **US-5:** As a user, I want to regenerate if I do not like the result so I can get a better option without re-typing.
- **US-6:** As a user, I want to save a result so I can find it again later.
- **US-7:** As a user, I want to view a history of everything I have generated, newest first, so I can recall past outreach.
- **US-8:** As a user, I want to open a history entry and read the full email and message so I can reuse or adapt it.

---

## 7. Product scope -- MVP feature set

The MVP is three screens and one AI call.

| # | Feature | Screen | Priority |
| --- | --- | --- | --- |
| F1 | Generate form (capture five inputs) | Generate | Must |
| F2 | AI generation via API route + OpenRouter | (server) | Must |
| F3 | Results display with copy + regenerate | Results | Must |
| F4 | Save to history (write to database) | Results | Must |
| F5 | History page (read from database) | History | Must |
| F6 | Light polish (nav, responsive layout) | All | Should |

Priority uses MoSCoW: **Must** = MVP fails without it; **Should** = include if time allows;
anything not listed is out of scope.

---

## 8. Functional requirements

Each requirement has an ID, the user story it serves, the behaviour, and acceptance
criteria.

### 8.1 F1 -- Generate form

**FR-1.1 (US-1):** The Generate screen presents a single form with exactly five fields:

| Field | Input type | Required | Notes |
| --- | --- | --- | --- |
| Prospect Name | text | Yes | The person being contacted |
| Company | text | Yes | Their company |
| Their Role | text | Yes | Their job title |
| Your Offer / What You Do | textarea | Yes | What the user is selling or offering |
| Pain Point / Reason for Reaching Out | textarea | Yes | The prospect's likely problem |

**FR-1.2:** A primary **Generate** button submits the form.

**FR-1.3:** All five fields are required. If any is empty on submit, the app prevents
submission and indicates which field is missing (browser-native validation is acceptable
for MVP).

**FR-1.4:** Field values are trimmed of leading/trailing whitespace before being sent.

**FR-1.5:** While a generation is in progress, the Generate button shows a loading state
("Generating...") and is disabled to prevent duplicate submissions.

**Acceptance criteria:**
- Submitting with all fields filled triggers a generation.
- Submitting with any field empty does not trigger a network call.
- The button cannot be double-clicked into two concurrent generations.

### 8.2 F2 -- AI generation (API route)

**FR-2.1 (US-2):** A server-side API route at `POST /api/generate` accepts a JSON body
containing: `prospect_name`, `company`, `role`, `offer`, `pain_point`.

**FR-2.2:** The route validates that all five fields are present and non-empty; otherwise
it returns HTTP 400 with an error message.

**FR-2.3:** The route calls the OpenRouter chat completions API
(`https://openrouter.ai/api/v1/chat/completions`) using the `OPENROUTER_API_KEY`
environment variable. The key is used **only** server-side and is never exposed to the
browser.

**FR-2.4:** The model is a current Claude model served via OpenRouter. The cohort build
uses `anthropic/claude-sonnet-4-20250514`; the model is a single configurable string and
can be swapped without other changes.

**FR-2.5:** The request uses the system and user prompts defined in
[section 9](#9-ai-and-prompt-specification) and requests JSON-only output.

**FR-2.6:** The route parses the model's response into an object with exactly three string
fields -- `email_subject`, `email_body`, `linkedin_message` -- and returns it as JSON.

**FR-2.7:** Error handling:
- Upstream (OpenRouter) error or timeout -> HTTP 502/500 with a friendly message.
- Response is not valid JSON / missing fields -> HTTP 500 with a parse-error message; the
  client may offer to regenerate.
- The route uses `fetch` (no extra HTTP client dependency).

**Acceptance criteria:**
- A valid request returns `{ email_subject, email_body, linkedin_message }`.
- A request missing a field returns 400 and does not call the model.
- A malformed model response does not crash the app; the user sees an error and can retry.

### 8.3 F3 -- Results display

**FR-3.1 (US-2):** When generation succeeds, the Results view appears below (or in place
of) the form and shows two cards:

1. **Cold Email** card -- the subject line in bold, the body beneath it.
2. **LinkedIn Message** card -- the message text.

**FR-3.2 (US-4):** Each card has a **Copy** button that copies that card's full text to
the clipboard and gives visible confirmation (e.g. the button briefly reads "Copied").

**FR-3.3 (US-5):** A **Regenerate** button re-runs F2 with the same inputs and replaces
the displayed results.

**FR-3.4:** A **Save to History** button (see F4) is present beneath the cards.

**FR-3.5:** Results are only shown once a successful result exists; before first
generation, no empty result cards are rendered.

**Acceptance criteria:**
- Both pieces of copy are visible and readable.
- Copy buttons place the correct text on the clipboard.
- Regenerate produces a new result without the user re-typing inputs.

### 8.4 F4 -- Save to history

**FR-4.1 (US-6):** The **Save to History** button inserts one row into the `outreach`
table containing all five inputs and all three AI outputs.

**FR-4.2:** On success, the UI shows a transient confirmation ("Saved!") that disappears
after ~2 seconds.

**FR-4.3:** After a successful save, the button is disabled to prevent duplicate saves of
the same result.

**FR-4.4:** The write uses the shared Supabase client (`lib/supabase.ts`) configured from
environment variables.

**Acceptance criteria:**
- A saved generation appears as a new row in the `outreach` table.
- The same result cannot be saved twice in a row.
- A failed save surfaces an error rather than silently doing nothing.

### 8.5 F5 -- History page

**FR-5.1 (US-7):** A `/history` route fetches all rows from the `outreach` table ordered
by `created_at` descending (newest first).

**FR-5.2:** Rows are displayed in a clean list/table showing at minimum: Prospect Name,
Company, and a nicely formatted Date.

**FR-5.3 (US-8):** Each row is expandable; expanding it reveals the full email (subject +
body) and LinkedIn message, each with a Copy button.

**FR-5.4:** The page includes a link back to the Generate screen, and the Generate screen
includes a link to History.

**FR-5.5:** If there are no saved entries, the page shows a friendly empty state.

**Acceptance criteria:**
- All saved entries appear, newest first.
- Expanding a row shows the complete saved content.
- Navigation between Generate and History works in both directions.

### 8.6 F6 -- Polish (Should-have)

**FR-6.1:** A simple navigation affordance links Generate and History.

**FR-6.2:** Layout is responsive and usable on a phone screen.

**FR-6.3:** The page has a clear title ("Outreach Generator") and consistent, clean
styling.

---

## 9. AI and prompt specification

### 9.1 Model

- **Provider:** OpenRouter (single API key, model-agnostic gateway).
- **Model:** `anthropic/claude-sonnet-4-20250514` (configurable string; any current Claude
  model works).
- **Why OpenRouter:** one key and one endpoint that can reach many models, so the model
  choice is a one-line change rather than a re-integration.

### 9.2 System prompt

```
You are an expert B2B copywriter. Always respond with valid JSON only, no markdown.
```

### 9.3 User prompt (template)

The five inputs are interpolated into a prompt that asks for both pieces of copy and
specifies the output contract:

```
You are an expert B2B copywriter.

Write two pieces of outreach copy for the following context:

Prospect: [prospect_name] -- [role] at [company]
What we offer: [offer]
Their likely pain point: [pain_point]

Output:
1. A cold email with a subject line. Keep it under 150 words.
   Conversational, not salesy. End with one clear CTA.
2. A LinkedIn connection message. Under 300 characters.
   Warm, human, specific.

Format your response as JSON:
{
  "email_subject": "",
  "email_body": "",
  "linkedin_message": ""
}
```

### 9.4 Output contract

The model must return a JSON object with exactly these keys, all strings:

| Key | Meaning | Constraint |
| --- | --- | --- |
| `email_subject` | Cold email subject line | Short, specific |
| `email_body` | Cold email body | Under ~150 words, conversational, one CTA |
| `linkedin_message` | LinkedIn connection note | Under 300 characters |

### 9.5 Prompt design rationale (teaching note)

- **Role framing** ("expert B2B copywriter") sets tone and quality.
- **Structured context** (the five fields) is what makes the output personalised --
  garbage in, garbage out.
- **Explicit constraints** (word/character limits, one CTA, "conversational not salesy")
  steer the output toward usable copy.
- **JSON-only output** is essential: it lets the code reliably split subject, body, and
  message into separate fields instead of trying to parse free text.

---

## 10. Data model

A single table backs the MVP.

### 10.1 Table: `outreach`

| Column | Type | Source | Notes |
| --- | --- | --- | --- |
| `id` | uuid | auto | Primary key, default `gen_random_uuid()` |
| `prospect_name` | text | input | |
| `company` | text | input | |
| `role` | text | input | |
| `offer` | text | input | |
| `pain_point` | text | input | |
| `email_subject` | text | AI output | |
| `email_body` | text | AI output | |
| `linkedin_message` | text | AI output | |
| `created_at` | timestamptz | auto | Default `now()` |

- The **first five** columns are user inputs.
- The **next three** are AI outputs.
- The **last two** are handled automatically by the database.

### 10.2 Why store inputs alongside outputs

Saving the inputs next to the generated copy means the history shows *who* the user wrote
to and *why*, not just the final text -- which is what makes history genuinely useful for
recall and reuse.

### 10.3 Row-Level Security (RLS)

For the MVP, RLS is **disabled** on this table. This is acceptable only because there is
no authentication and the app is a personal/learning project. It is a known limitation,
not a recommended production posture -- see [risks](#16-risks-and-mitigations) and
[roadmap](#15-out-of-scope-and-future-roadmap).

---

## 11. System architecture and tech stack

### 11.1 Components

| Layer | Technology | Responsibility |
| --- | --- | --- |
| Frontend | Next.js (App Router, TypeScript, Tailwind CSS) | The three screens and all UI |
| Server | Next.js API route (`/api/generate`) | Calls OpenRouter; keeps the API key server-side |
| AI | OpenRouter -> Claude | Generates the copy |
| Database | Supabase (Postgres) | Stores saved outreach |
| Source control | GitHub | Stores code history; triggers deploys |
| Hosting | Vercel | Serves the app; auto-deploys on push |
| Build environment | Cursor (agent) + Supabase MCP | How the app is built and the database is managed |

### 11.2 Key architectural decisions

- **The AI call lives in a server-side API route, not the browser.** This keeps
  `OPENROUTER_API_KEY` secret. Putting model calls in client code would leak the key.
- **The database client uses the public anon key.** Because there is no auth in the MVP,
  the app reads/writes with the Supabase anon key and RLS disabled. This is the explicit
  MVP trade-off.
- **Deployment is Git-connected.** Vercel is connected to the GitHub repo so every push to
  `main` rebuilds and redeploys -- no manual deploy step after setup.
- **Model access is brokered through OpenRouter** so the model is a configuration choice,
  not an integration.

---

## 12. Data flow

### 12.1 Generation and save (runtime)

1. The user fills in the five-field form on the Generate screen and submits.
2. The browser sends those five values to `POST /api/generate`.
3. The API route wraps them in the system + user prompt and calls OpenRouter.
4. The model returns JSON; the route parses it and returns
   `{ email_subject, email_body, linkedin_message }`.
5. The Results screen displays the email and LinkedIn message with copy buttons.
6. On **Save to History**, the browser writes a row (inputs + outputs) to Supabase.
7. The History page reads all rows from Supabase, newest first, and displays them.

### 12.2 Code-to-internet flow (delivery)

1. Code is committed and pushed from the laptop to GitHub.
2. Vercel detects the push and automatically rebuilds and redeploys.
3. The live site updates at the Vercel URL.

---

## 13. Non-functional requirements

### 13.1 Performance

- A typical generation returns within ~10 seconds; the UI must show a loading state for
  the full duration.
- History should load all rows for a single user comfortably (data volume is small in the
  MVP; pagination is out of scope).

### 13.2 Security and privacy

- `OPENROUTER_API_KEY` is server-side only and never sent to the browser.
- Secrets live in `.env.local` (never committed to GitHub) locally, and in Vercel's
  environment variables in production.
- **Known MVP limitation:** with no auth and RLS disabled, anyone with the URL can use the
  app and all saved data is shared. Acceptable for a personal/demo build only.

### 13.3 Cost

- Each generation consumes OpenRouter tokens. The cohort budget assumes a small top-up
  (e.g. $5) is sufficient for development and demos.
- **Known MVP limitation:** there is no rate limiting or spend cap, so a malicious user
  could run up cost. Out of scope for MVP; see roadmap.

### 13.4 Reliability and error handling

- Upstream failures, timeouts, and malformed responses must surface a clear, non-technical
  error and leave the app usable (the user can retry/regenerate).
- A failed save must not appear to succeed.

### 13.5 Accessibility and UX

- Form fields have labels; the form is keyboard-usable.
- Sufficient colour contrast; readable typography.
- Clear primary actions and visible loading/disabled states.

### 13.6 Maintainability

- The Supabase client is defined once (`lib/supabase.ts`) and reused.
- The model name and prompt are easy to locate and change.
- Code is committed in small, described steps (the Checkpoint habit).

---

## 14. Configuration and environment variables

| Variable | Where used | Secret? | Source |
| --- | --- | --- | --- |
| `NEXT_PUBLIC_SUPABASE_URL` | Client (Supabase client) | No (public) | Supabase project settings |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Client (Supabase client) | No (public) | Supabase project settings |
| `OPENROUTER_API_KEY` | Server (API route only) | **Yes** | openrouter.ai -> Keys |

- Locally: stored in `.env.local`, which is git-ignored.
- In production: stored in Vercel's environment variables for the project.
- The `NEXT_PUBLIC_` prefix intentionally exposes those two values to the browser; the
  OpenRouter key has no such prefix and must never be exposed.

---

## 15. Out of scope and future roadmap

These are deliberately excluded from the MVP. Naming them is how the MVP ships. Each is a
natural next build once the foundations are solid.

| Area | Why it is out of scope for MVP | What it becomes later |
| --- | --- | --- |
| Authentication | Adds significant surface area; not needed to prove the core | Supabase Auth so each user sees only their own data |
| Authorisation / RLS | No auth means no per-user rules yet | Re-enable RLS and write read/write policies |
| Payments | Not monetising during the build | Stripe for subscriptions or credits |
| Rate limiting / abuse protection | No public exposure risk during the build | Per-user/IP limits and spend caps on OpenRouter |
| Edit / delete history | Read + create is enough to prove the loop | Full CRUD on saved entries |
| Search / filter / paginate history | Data volume is tiny in MVP | Server-side search and pagination |
| Model picker / prompt tuning UI | One good model and prompt is enough | Let users choose models and adjust tone |
| Send from app / CRM sync | Copy-to-clipboard covers the job | Email/LinkedIn/CRM integrations |
| Analytics | Not needed to learn the stack | Product analytics (e.g. PostHog) |
| Automated tests / CI | Out of scope for a first MVP | Unit/integration tests and a CI pipeline |
| Custom domain | `vercel.app` URL is fine for MVP | Connect a custom domain in Vercel |

---

## 16. Risks and mitigations

| # | Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- | --- |
| R1 | Model returns invalid or non-JSON output | Medium | High (breaks results) | Strict "JSON only, no markdown" prompt; parse guard; user can regenerate |
| R2 | OpenRouter outage, latency, or rate limit | Low | High | Clear error state and retry; keep generation idempotent |
| R3 | API key leakage | Low | High | Key stays server-side only; never `NEXT_PUBLIC_`; not committed |
| R4 | RLS disabled exposes data | High (by design) | Medium | Accepted MVP trade-off; documented; roadmap to enable RLS + auth |
| R5 | No rate limiting -> runaway cost | Medium | Medium | Small credit cap during build; roadmap to add limits |
| R6 | Setup friction (Supabase MCP / deploy auth) | Medium | Low--Medium | Agent-driven setup with documented troubleshooting; dashboard fallbacks |
| R7 | Generated copy quality is mediocre | Medium | High (it is the "one thing") | Disciplined prompt with constraints; quality depends on specific inputs -- coach users to write a real pain point |

---

## 17. Acceptance criteria -- definition of done

The MVP is "done" when **all** of the following are true:

1. A user can fill in the five-field form and generate copy. *(F1, F2)*
2. The cold email (subject + body) and LinkedIn message display with working copy buttons.
   *(F3)*
3. Regenerate produces a new result from the same inputs. *(F3)*
4. Save to History writes a complete row to the `outreach` table and confirms success.
   *(F4)*
5. The History page lists all saved entries newest-first and expands to show full content.
   *(F5)*
6. The app is deployed to a public Vercel URL and works end-to-end on a phone. *(deploy)*
7. Pushing to GitHub automatically redeploys the live site. *(deploy)*
8. No secret key is exposed to the browser; `OPENROUTER_API_KEY` is server-side only.
   *(NFR security)*

A demo script that proves "done": open the live URL on a phone -> generate copy for a real
prospect -> copy the email -> save to history -> open History -> confirm the entry ->
confirm the row exists in Supabase.

---

## 18. Assumptions and open questions

**Assumptions:**
- A single shared dataset is acceptable because this is a personal/learning MVP.
- One model and one prompt are sufficient to prove the core value.
- Users are comfortable copy-pasting the output into their own email/LinkedIn.

**Open questions (to revisit post-MVP):**
- Should regeneration keep a short history of variants so the user can compare?
- What is the right default tone, and should it be user-adjustable?
- At what usage level does the lack of auth/rate limiting become unacceptable?

---

## 19. Glossary

| Term | Plain-English meaning |
| --- | --- |
| **MVP** | Minimum viable product -- the smallest version that delivers the core value |
| **API route** | Server-side code that the frontend calls; here it talks to the AI and hides the key |
| **OpenRouter** | A gateway that gives one API key access to many AI models |
| **MCP** | Model Context Protocol -- lets the Cursor agent talk directly to Supabase |
| **RLS** | Row-Level Security -- database rules for who can read/write which rows |
| **Anon key** | Supabase's public client key; safe to expose, governed by RLS (disabled here) |
| **Environment variable** | A configuration value (often a secret) kept outside the code |
| **CTA** | Call to action -- the one clear next step in the email |

---

## 20. Appendix -- mapping to the cohort build order

How this PRD maps onto the three-week build in `README.md`:

| Week | Build step | PRD section |
| --- | --- | --- |
| 1 | Scope the app | 3, 4, 5, 6 |
| 1 | Create project + Generate form | F1 (8.1), 11 |
| 2 | Supabase + table + env vars | 10, 14 |
| 2 | API route + AI integration | F2 (8.2), 9 |
| 2 | Results screen | F3 (8.3) |
| 2 | Save to history | F4 (8.4), 10 |
| 3 | History page | F5 (8.5) |
| 3 | Deploy to Vercel | 11, 12.2, 17 |
| Post-cohort | "What's next" | 15 |

---

*This PRD describes an intentionally minimal MVP. Its discipline is in what it leaves out
as much as what it includes -- which is exactly the scoping skill buildAcademy teaches.*
