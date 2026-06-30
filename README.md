# buildAcademy Student Manual

**What we're building:** An AI-powered sales outreach generator
**Format:** 3 live online evenings (Google Meet) -- Tue 6:00-8:00pm AEST
**Dates:** Tue 23 Jun, Tue 30 Jun, Tue 7 Jul

Same app, same skills as a single-day build -- spread over three weeks so each layer
has room to land. Week 1 we plan and scaffold, Week 2 we connect the database and the
AI, Week 3 we debug and ship it live.

---

## Before the Cohort -- Setup

Follow the setup video and the steps below before Week 1 so the three Tuesdays are
pure building -- not a minute spent on installs. The goal is a build-ready laptop.

Complete these before Week 1:

1. **Install Cursor** -- go to cursor.com, download and install it, and sign up for a Pro subscription (or start a free trial)
2. **Create these accounts** (all free):
  - **GitHub** -- github.com
  - **Vercel** -- vercel.com (sign up with your GitHub account)
  - **Supabase** -- supabase.com
  - **OpenRouter** -- openrouter.ai (add $5 credit -- this will last you weeks beyond the cohort)

Then connect your tools (next section) so the Cursor agent can drive everything from
night one.

---

## Connect Your Tools (before Week 1)

Throughout buildAcademy we lean on one core skill: **using the Cursor agent to do the
actual work** -- installing tools, running commands, and making the real changes to
your app. You describe what you want in plain English; the agent executes it. We start
practising right now by connecting the two services you'll use all cohort -- **GitHub**
(where your code lives) and **Supabase** (your database). There's more on both in *The
Tech Stack* below; for now the point is just to get them talking to Cursor, because
these are the same skills you'll use to build and ship later.

### 1. Pull this manual from GitHub (your first agent prompt)

Open Cursor, open a new agent chat (Cmd+L on Mac, Ctrl+L on Windows), and paste this:

```
Check whether Node.js and Git are installed on this machine, and install whichever
is missing (use Homebrew on Mac -- installing Homebrew first if needed; on Windows
download Node.js from nodejs.org and Git from git-scm.com).
Then clone the public repo https://github.com/callumholt/buildAcademy.git onto my
Desktop and open it in Cursor.
Tell me the installed version of Node.js and Git when you're done.
```

If the agent reports a version for Node.js and Git and the project opens in Cursor,
your GitHub connection works -- you've just pulled real code down from the internet by
asking in plain English. Keep this manual open in Cursor as your reference for all
three weeks.

### 2. Connect Supabase to Cursor (MCP)

MCP is what lets the Cursor agent talk directly to Supabase -- so later it can create
your database tables and read your data for you, without you leaving Cursor. We'll set
it up the buildAcademy way: by asking the agent to do it.

**First, log in to Supabase in your browser** (open supabase.com and sign in). With
that already done, the approval step later is a single click instead of a full login
in a small popup window.

**Step 1 -- let the agent add the MCP server.** In an agent chat, paste this:

```
Add the Supabase MCP server to my Cursor configuration. Edit (or create) the file at
~/.cursor/mcp.json and add an MCP server named "supabase" with the URL
https://mcp.supabase.com/mcp. Keep any servers already in the file exactly as they
are, and make sure the final file is valid JSON. Show me the file when you're done.
```

The agent writes the small config file for you -- this is the same file Cursor opens if
you click **Settings → Cursor Settings → Tools & MCP → New MCP Server**, just without
you having to hand-edit any JSON.

**Step 2 -- connect (log in).** Open **Settings → Cursor Settings → Tools & MCP**. You'll
see **supabase** in the list with an amber dot and **"Needs authentication"**, plus a
**Connect** button. Click **Connect** -- a browser window opens. Log in to Supabase if
asked, then on the **Authorize Cursor** screen **select your organization** from the
dropdown (if you just made your account, there's one default org) and click the green
**Authorize Cursor**. Leave the permissions as they are -- they include Database
read + write, which is exactly what we need. It uses a secure login, so there are no
tokens to copy or paste. (If **supabase** doesn't appear yet, fully quit and reopen
Cursor, since MCP servers load when Cursor starts.)

**Step 3 -- confirm it's connected.** The dot next to **supabase** turns green once
you've approved. Then test it in an agent chat:

```
Using the Supabase MCP, list my Supabase projects and any tables in them.
If I don't have a project yet, just confirm that you can see my Supabase account.
```

If the agent can see your Supabase account, you're connected -- and you've just given
Cursor a direct line to your database. (Official step-by-step, in case the screens look
different: [https://supabase.com/docs/guides/getting-started/mcp](https://supabase.com/docs/guides/getting-started/mcp))

> Seeing a red dot or an error on the Supabase MCP? That's the most common setup snag
> -- jump to the **Troubleshooting** section at the bottom of this manual.

Stuck on any of this? Reply to the setup email and we'll sort it out before Week 1.

---

## Each Night (Google Meet)

We'll send you the Meet link beforehand, so you're all set -- just click it to join.
The link opens at **5:30pm** each night -- 30 minutes early -- so you can test your
audio, video, and screen-share before we start. Cameras on: this is a cohort, not a
lecture. We start at 6:00pm sharp.

---

# WEEK 1 -- Scope & Scaffold

**End state tonight:** a scoped plan for the app, the project running on your laptop, and the Generate form on screen in your browser.

First, the big picture -- what we're building, the tools we'll use, the three habits that keep you unstuck, and the plan for all three weeks. Then we start scoping.

## What We're Building

Our app does one thing: you type in who you're reaching out to and why, and it gives you a personalised cold email and LinkedIn message.

For the complete spec -- every feature, the data model, the AI prompt, the architecture, and what's deliberately out of scope -- see the **[Product Requirements Document (PRD)](PRD.md)**.

### Why this app?

This project is deliberately simple, but it covers every fundamental skill you need to build real software:

- **A frontend UI** -- the form and results screens are what the user actually sees and interacts with. Every app starts here.
- **An AI integration** -- the app sends a request to an AI provider (OpenRouter), receives structured data back, and does something useful with it. This is the same pattern behind every AI-powered product.
- **A database** -- the app writes data to Supabase and reads it back. Storing and retrieving information from a permanent data store is the backbone of any application that remembers anything. This is effectively google sheets or microsoft excel, and infact those software packages can even be used as a data store.
- **Version control** -- pushing code to GitHub means your work is backed up in the cloud and you can always go back to a previous version. This is how every professional development team works. It also means if your laptop blows up, you can just get a new laptop and connect to github and continue on from where you left it. Github is like a library or a book, it stores your code, but it doesn't run your code.
- **Deployment** -- connecting to Vercel takes your app from "running on my laptop" to "live on the internet with a real URL." This is the step most people skip when learning, and it's the step that makes everything real. Vercel runs ontop of AWS and is effectively an 'always on computer' that runs your code just like localhost:3000 does on your local machine.

By the end of three weeks you'll have touched every layer of a modern web application. The app itself is useful, but the real value is understanding how these pieces fit together -- because every app you build after this uses the same architecture.

### How it all fits together

```mermaid
flowchart LR
    User(("User")) -- fills in form --> Form[Generate Form]
    Form -- sends data --> API["The messenger<br/><i>connects form to AI</i>"]
    API -- prompt + context --> OR[OpenRouter]
    OR -- JSON response --> API
    API -- email + LinkedIn msg --> Results[Results Screen]
    Results -- save --> DB[(Supabase)]
    DB -- load history --> History[History Page]
    Laptop[Your Laptop] -- git push --> GH[GitHub]
    GH -- auto-deploy --> V[Vercel URL]
```



### Following the data, step by step

Same journey as above, but this time watch the actual information move. Think of it like
filling in a form, handing it to an assistant, and getting a finished letter back.

```mermaid
flowchart TD
    A["You type in the form<br/><i>Sarah, VP Sales at Acme,<br/>I sell CRM software,<br/>her team wastes hours on admin</i>"]
    A -- "those 5 details get sent" --> B["The messenger in the middle<br/><i>connects your form to the AI</i>"]
    B -- "wraps your details in<br/>a request and asks the AI" --> C["OpenRouter (the AI)<br/><i>writes the copy</i>"]
    C -- "sends back finished writing<br/>as neat, labelled data" --> B
    B -- "passes the writing<br/>to the screen" --> D["Results screen<br/><i>Subject + email body<br/>+ LinkedIn message,<br/>each with a Copy button</i>"]
    D -- "you click Save" --> E["Supabase (the database)<br/><i>stores a permanent copy,<br/>like a row in a spreadsheet</i>"]
    E -- "the History page<br/>asks for all saved rows" --> F["History page<br/><i>every email you've<br/>ever generated, newest first</i>"]
```



**What's actually moving at each arrow:**

1. **You → messenger:** the 5 things you typed (name, company, role, your offer, their pain point)
2. **Messenger → AI:** those details wrapped in instructions ("write a cold email and a LinkedIn message")
3. **AI → messenger:** the finished writing, sent back as labelled data (`email_subject`, `email_body`, `linkedin_message`)
4. **Messenger → screen:** the writing, shown to you with Copy buttons
5. **Screen → database:** when you hit Save, everything (your inputs + the AI's outputs) is stored permanently
6. **Database → History page:** the History page reads everything back out so you can see all your past work

```mermaid
flowchart LR
    L["Your laptop<br/><i>where you build</i>"] -- "git push<br/><i>upload your code</i>" --> G["GitHub<br/><i>the cloud backup</i>"]
    G -- "auto-deploy<br/><i>builds & publishes</i>" --> V["Vercel<br/><i>your live website,<br/>open to anyone</i>"]
```



Your code travels separately: you push it to GitHub (the backup), and Vercel automatically
turns that into a live website. Every time you push, the live site updates itself.

**Three screens:**

- **Generate** -- a form where you enter the prospect's details
- **Results** -- displays the AI-generated email and LinkedIn message
- **History** -- shows all your saved outreach entries

We'll design the database table in Week 1 and write the AI prompt in Week 2 -- where
each one is actually used.

---

## The Tech Stack

Here's what each tool does, in plain English:


| Tool           | What it does                                                                                                                             |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Cursor**     | Your AI building partner. You describe what you want in English, it writes the code.                                                     |
| **Supabase**   | Your database -- where your app stores information. Like a spreadsheet your app can read and write to.                                   |
| **MCP**        | The bridge -- lets Cursor talk directly to Supabase, so the agent can create tables and manage your database without you leaving Cursor. |
| **GitHub**     | Your save file -- keeps a history of every version of your code. Like Google Docs version history.                                       |
| **Vercel**     | Your hosting -- puts your app on the internet so anyone can visit it.                                                                    |
| **OpenRouter** | Your AI access -- one key that connects to any AI model (Claude, GPT, etc.)                                                              |


---

## Three Habits That Will Save You

Keep these in mind every night. These three are what YouTube won't teach you -- they
map to the three ways people get stuck with AI tools (the fix spiral, the planning
gap, the deployment cliff).

1. **The Checkpoint Rule** -- Before we start anything new, we commit and push to GitHub. It's a save point. If the next thing breaks, we go back to here.
2. **The Reset Rule** -- If Cursor starts spiralling (fixing one thing and breaking another), we stop, revert to the last checkpoint, and try a different prompt. We never chase a fix spiral.
3. **One thing at a time** -- We never ask Cursor to do five things at once. One feature, one prompt, one test.

---

## Build Order

This is the checklist for the whole cohort. After every step, we commit and push to GitHub. The arrows show where each week lands.

```
WEEK 1 -- Scope & Scaffold
0. Scope the app (on paper -- no code yet)
1. Create the project + connect to GitHub                → CHECKPOINT
2. Build the Generate form (5 fields + button)           → CHECKPOINT

WEEK 2 -- Database & AI
3. Set up Supabase (MCP + table + secret keys)           → CHECKPOINT
4. Connect the form to the AI (OpenRouter)               → CHECKPOINT
5. Build the Results screen (email + LinkedIn display)   → CHECKPOINT
6. Add "Save to History" (write to Supabase)             → CHECKPOINT

WEEK 3 -- Debug & Ship
7. Build the History page (read from Supabase)           → CHECKPOINT
8. Deploy to Vercel                                      → CHECKPOINT (final)
```

---

## Step 0: Scope the App (before any code)

This is the most important step of the whole cohort -- and the one most people skip.
Anyone can ask an AI to "build me an app". The people who actually ship are the ones
who scope first: they decide exactly what they're building before they prompt. AI
removed the hard part of *writing* the code. It did not remove the hard part of
*deciding what to build* -- that's now your job, and it's the skill that lets you walk
away from this cohort and build your own thing.

### The SCOPE framework

Five questions, one word: **SCOPE**. Write them down -- they work for **any** app you
ever build, not just this one:

- **S -- Solve:** who are you solving the problem for? (the user)
- **C -- Challenge:** what specific problem does the app solve? (the job)
- **O -- One thing:** what's the one thing that, if it didn't work, makes the whole app pointless? (the core -- protect this above all)
- **P -- Path:** where does the information come from, and where does it need to end up? (inputs → outputs → storage)
- **E -- Exclude:** what will you deliberately leave for later? (the cuts -- this is how you actually ship)

### Our app, scoped

Here's the framework applied to the app we're building together:

- **For:** someone doing cold outreach -- a founder, salesperson, or recruiter
- **Problem:** writing personalised cold emails and LinkedIn messages is slow
- **Core:** the generated copy must be good and personalised. If that fails, nothing else matters.
- **Info:** user types prospect details → AI generates copy → we save it to a database → they read it back in history
- **Later (out of scope):** login, payments, multiple users. We name these so we stop trying to build everything at once.

> **Scope is not a limitation -- it's the skill. Naming what you're NOT building is how anything ever ships.**

### Go deeper: the full PRD

Those five answers are the *short* version of scoping. The fully written-out version --
every feature, the data, the AI prompt, the architecture, and everything we're
deliberately leaving out -- lives in the **[Product Requirements Document (PRD)](PRD.md)**.

A PRD is just scoping expanded into a plan you (or the agent) can build from. Skim it to
see where tonight's SCOPE answers lead, and use it as a template when you scope your own
app after the cohort -- swap the problem, users, features, data, and prompt, and you've
got a plan for any AI SaaS MVP.

### From scope to structure

**Path** (inputs → outputs → storage) turns straight into the table our app needs.
This is what "thinking in data" looks like:

```
Table: outreach
-----------------------------------------
id              | uuid (auto-generated)
prospect_name   | text
company         | text
role            | text
offer           | text
pain_point      | text
email_subject   | text
email_body      | text
linkedin_message| text
created_at      | timestamp (auto-generated)
-----------------------------------------
```

- The first five columns are what you type in (**inputs**)
- The next three are what the AI generates (**outputs**)
- The last two are automatic (the database handles them)

We save the inputs next to the outputs so your history shows *who* you wrote to and
*why*, not just the text. We'll build this table for real in Week 2.

Keep the SCOPE framework -- it's the move you'll repeat for every app you build after
this cohort. Tonight we put it to work on the app we're all building together.

### The payoff: scope well, and the build almost writes itself

Here's why all this planning matters. Once you've put the time, energy, and effort into
scoping properly -- once you and the agent both understand exactly what you're building --
the build itself becomes remarkably straightforward. With a clear PRD in hand, you can
often **one-shot the whole app from a single prompt** and get something accurate and
close to done, because there's no ambiguity left for the agent to guess at. The plan *is*
the hard part; the code is what flows out of it.

For teaching purposes, we deliberately break the build into small steps across the next
three weeks -- so you see every layer, understand what each piece does, and learn to fix
it when it breaks. But once you've done it once and you trust your scope, **you don't have
to go step by step.** You can hand the agent your PRD and let it build the lot in one go.

Here's a single prompt that takes a PRD like [PRD.md](PRD.md) and builds *and* deploys the
whole app -- database, code, and live URL -- using the Supabase MCP, the GitHub CLI, and
the Vercel CLI you set up earlier:

```
Read PRD.md in this project and build the entire app it describes, then put it live on
the internet. Tell me what you're doing at each stage.

Here are my keys -- these are just examples, so use my real values instead:

SUPABASE_URL = https://abcd1234efgh.supabase.co
SUPABASE_ANON_KEY = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example-anon-key-here
OPENROUTER_API_KEY = sk-or-v1-1234567890abcdef1234567890abcdef

Then:

1. Build the app with all the screens and features exactly as the PRD describes.

2. Using the Supabase MCP, create the database table(s) in my Supabase project exactly
   as the PRD's data model describes (turn off Row Level Security for now, since there's
   no login yet).

3. Connect the app to my database and, if the PRD needs AI, to the AI -- using the keys
   above. Keep these keys private; never expose them in the part of the app that runs in
   people's browsers.

4. Make the app save information to my database and read it back exactly the way the PRD
   describes.

5. Put my code on GitHub using the GitHub CLI: if it isn't installed or I'm not logged in,
   set that up first, then create a new private repository and upload my code.

6. Publish it with the Vercel CLI so it's live on the internet: install and log in if
   needed, create the project, add my three keys above so the live app can use them,
   connect my GitHub repository so future changes go live automatically, and deploy. Give
   me the live web address when it's done.

Save my progress to GitHub after each major step. If anything in the PRD is unclear, ask
me instead of guessing.
```

You don't need to use that tonight -- it's here so you can see where we're heading. The
rest of this manual walks the same journey one careful step at a time.

## YOUR TURN!!!

## Step 1: Create the Project + Connect to GitHub

### Create the project

1. Open Cursor
2. Open a new agent chat (Cmd+L on Mac, Ctrl+L on Windows)
3. Type this prompt:

```
Create a new web app project called "outreach-generator" on my Desktop.
Use whatever stack is standard for building a modern web app in Cursor.
After creating it, open the project folder and start the app so I can see it in my browser.
```

1. Wait for the agent to finish
2. Start the app by prompting it with `start the app`. Then open your browser to the address the agent gives you (usually [http://localhost:3000](http://localhost:3000)) — that's your app running

### Key files to know

The agent will create a bunch of files — you don't need to understand them all. These three
are worth knowing:

- **The main page** — what you see in the browser. We'll replace it with our form.
- **The layout** — the frame around every screen. Rarely needs changing.
- **`.env.local`** — where secret keys live. This file never gets uploaded to GitHub.

### Connect to GitHub

No clicking around github.com -- the agent can create the repository *and* push your
code for you using the GitHub CLI. In the agent chat:

```
Put this project on GitHub using the GitHub CLI (gh):
1. If the GitHub CLI (gh) isn't installed, install it (Homebrew on Mac, or the
   official installer on Windows).
2. If I'm not logged in to GitHub yet, run "gh auth login" and walk me through it --
   choose GitHub.com over HTTPS and authenticate in the browser when it opens.
3. Commit all current changes with the message "initial project setup".
4. Create a new PRIVATE GitHub repo called "outreach-generator" from this project,
   add it as the "origin" remote, and push to the main branch.
```

The first time you run this, a browser window opens to log in to GitHub -- approve it
and the agent does the rest. When it finishes, your code is on GitHub (open the repo
link it gives you to check). Logging in here also means every future `git push` just
works -- no logging in again.

---

## Step 2: Build the Generate Form

In Cursor's agent chat:

```
Replace the main page with a clean outreach email generator form.

The form should have these fields:
- Prospect Name (text input)
- Company (text input)
- Their Role (text input)
- Your Offer / What You Do (textarea)
- Pain Point / Reason for Reaching Out (textarea)

And a large "Generate" button at the bottom.

Make it look professional — centered on the page, clean spacing, subtle shadows.
The page title should be "Outreach Generator".

The form should not submit yet — just build the UI.
```

Review and accept the changes. Check the browser -- the form should be there.

**CHECKPOINT:** In the agent chat: `Commit all changes with the message 'add generate form' and push to GitHub`

### Homework (Week 1 → Week 2)

1. Make sure the project still runs after you reopen Cursor.
2. Make the form yours -- change the title, colours, and add placeholder example text in the fields, using one Cursor prompt at a time and committing after each.
3. Confirm the Supabase MCP shows a green dot in Cursor (**Settings → Cursor Settings → Tools & MCP**); if not, click **Connect** to reconnect.

---

# WEEK 2 -- Database & AI

**End state tonight:** Supabase connected, the AI generating real personalised copy, results displayed on screen, and Save-to-History working.

## Step 3: Set Up Supabase + MCP + Secret Keys

### Part A: Confirm the Supabase MCP is connected

You connected the Supabase MCP back in setup, so the agent can already talk to your
Supabase account. Quick check: open **Settings → Cursor Settings → Tools & MCP** and
make sure the dot next to **supabase** is green. If it says **"Needs authentication,"**
click **Connect** and approve access. (If you skipped setup, follow *Connect Your Tools →
Connect Supabase to Cursor (MCP)* near the top of this manual.)

### Part B: Create the Supabase project

Because the agent can talk to Supabase, let it create the project for you. In the agent chat:

```
Using the Supabase MCP, create a new Supabase project called "outreach-generator"
in my organization, in the region closest to me. Let me know when it's ready.
```

Wait ~2 minutes for it to provision. (Prefer to do it by hand? Go to supabase.com →
**New project**, name it `outreach-generator`, pick the closest region, and save the
database password somewhere safe.)

### Part C: Create the database table

In Cursor's agent chat:

```
Using Supabase MCP, create a table called "outreach" in my Supabase project
with the following columns:

- id: uuid, primary key, default gen_random_uuid()
- prospect_name: text
- company: text
- role: text
- offer: text
- pain_point: text
- email_subject: text
- email_body: text
- linkedin_message: text
- created_at: timestamptz, default now()

Disable Row Level Security on this table for now.
```

After it completes, check your Supabase dashboard (Table Editor) to verify the table is there.

> **Why another Supabase step?** Part C built the storage in the cloud (the `outreach`
> table). Part D teaches *your app* how to use it — like giving the website the keys to
> the filing cabinet. Cursor's MCP connection (from setup) lets the agent manage your
> database while you build. The Project URL + anon key let the app itself read and write
> when someone clicks "Save to History". You need both.

### Part D: Connect the app to Supabase

1. In Supabase, go to **Settings** then **API** — copy the **Project URL** and **anon key**
2. Go to openrouter.ai, then **Keys** — copy your API key
3. In Cursor's agent chat:

```
Connect this app to my Supabase project so it can save and load data later.

Create a .env.local file with these values (paste yours in below):

NEXT_PUBLIC_SUPABASE_URL=<paste-your-supabase-url>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<paste-your-anon-key>
OPENROUTER_API_KEY=<paste-your-openrouter-key>

Do whatever setup is needed so this app can talk to Supabase when it's running —
install any packages and add any connection code you need.

Don't wire up the Save button yet — just get the app ready to talk to Supabase.

When you're done, explain what you set up in plain English.
```

After this, restart the app so it picks up the new environment variables. In the agent chat: `Stop the app and start it again`

**CHECKPOINT:** In the agent chat: `Commit all changes with the message 'set up supabase and env vars' and push to GitHub`

---

## Step 4: Build the API Route + AI Integration

This is the heart of the app: the messenger in the middle takes the form details, sends them to the
AI with a carefully written prompt, and gets back structured copy. Here's the prompt
we're building around -- notice it asks for **JSON** so the code can reliably split the
subject, body, and LinkedIn message apart:

```
You are an expert B2B copywriter.

Write two pieces of outreach copy for the following context:

Prospect: [name] -- [role] at [company]
What we offer: [your_offer]
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

Now build it. In the agent chat:

```
I need to add AI-powered email generation to this app. Do the following:

1. Create the backend connection that receives the form data when someone clicks
   Generate, sends it to OpenRouter (https://openrouter.ai/api/v1/chat/completions)
   using the OPENROUTER_API_KEY environment variable, and returns the AI's response.
   - The form sends: prospect_name, company, role, offer, pain_point
   - Use the model "anthropic/claude-sonnet-4-20250514"
   - System prompt: "You are an expert B2B copywriter. Always respond with valid
     JSON only, no markdown."
   - User prompt: include all five fields and ask for a cold email (subject + body,
     under 150 words, conversational, one clear CTA) and a LinkedIn message
     (under 300 characters, warm and specific)
   - Request the response in JSON format:
     { "email_subject": "", "email_body": "", "linkedin_message": "" }
   - Parse the JSON from the AI response and return it
   - Include error handling

2. Wire up the Generate button on the main page so that when clicked:
   - It sends the form data to that backend connection
   - Shows a loading state on the button while waiting ("Generating...")
   - When the response comes back, store the result
   - For now, just console.log the result — we'll display it properly next
```

### Test it

1. Fill in the form with a real prospect
2. Hit Generate
3. Open browser DevTools (Cmd+Option+J on Mac, Ctrl+Shift+J on Windows)
4. Check the console -- you should see a JSON object with `email_subject`, `email_body`, and `linkedin_message`

If it works — you just built an AI feature. That pattern — take input, send it to a model, get structured output back — is behind every AI product.

**CHECKPOINT:** In the agent chat: `Commit all changes with the message 'add AI generation via openrouter' and push to GitHub`

---

## Step 5: Build the Results Screen

In the agent chat:

```
Update the main page so that when the AI response comes back, instead of
console.log, display the results below the form:

Show two cards side by side:
1. "Cold Email" card — show the subject line in bold, then the body below it.
   Include a "Copy" button that copies the full email to clipboard.
2. "LinkedIn Message" card — show the message. Include a "Copy" button.

Below both cards, show two buttons:
- "Regenerate" (calls the AI again with the same inputs)
- "Save to History" (we'll wire this up next)

Make the cards look clean with borders and padding.
```

**CHECKPOINT:** In the agent chat: `Commit all changes with the message 'display results with copy buttons' and push to GitHub`

---

## Step 6: Save to Supabase

In the agent chat:

```
Update the "Save to History" button on the main page so that when clicked it saves
everything to the "outreach" table in Supabase:

- All form fields: prospect_name, company, role, offer, pain_point
- All AI outputs: email_subject, email_body, linkedin_message

Show "Saved!" for 2 seconds, then disable the button so they can't save twice.
Use the Supabase connection you set up in Part D.
```

### Test it

1. Generate an email
2. Hit "Save to History"
3. Go to your Supabase dashboard, then Table Editor, then the outreach table
4. Your data should be there -- and it persists. Close the browser, it's still there.

**CHECKPOINT:** In the agent chat: `Commit all changes with the message 'save to supabase' and push to GitHub`

### Homework (Week 2 → Week 3)

1. **Build the History page yourself** with Cursor (Step 7 below has the prompt) -- a `/history` page that reads all rows newest-first and shows prospect, company, and date, expandable to the full email and message.
2. **Bring a real bug.** Intentionally tinker and break something, or note anything that broke. Week 3 opens with a live debugging workshop on YOUR bugs.
3. Commit after every change.

---

# WEEK 3 -- Debug & Ship

**End state tonight:** the app is live on the internet on your own Vercel URL, you can open it on your phone, and you have a repeatable method for fixing your own bugs.

## Debugging Workshop

This is the skill that makes you independent. Before we ship, we work through real bugs -- the ones you brought from homework -- as a group. Learn the method, not just the fix:

1. **Read the error** -- out loud. Most people skip this. The error usually says what's wrong.
2. **One change at a time** -- never fix five things at once.
3. **The Reset Rule** -- if a fix breaks something else, revert to the last checkpoint (`Revert all uncommitted changes back to the last commit`) and re-prompt more specifically.
4. **Give the agent context** -- paste the actual error text and the file into Cursor, don't just say "it's broken".

You'll use the Reset Rule a hundred times after this cohort. It's the difference between being stuck and being unstoppable.

## Step 7: Build the History Page

You built this for homework -- here's the prompt, so everyone's is working before we deploy. In the agent chat:

```
Create a new History page at /history that:

1. On load, fetches all rows from the Supabase "outreach" table, ordered by
   created_at descending (newest first)
2. Displays them in a clean table with columns: Prospect Name, Company,
   Date (formatted nicely)
3. Each row is clickable — when clicked, it expands to show the full email
   and LinkedIn message below the row, with copy buttons
4. Include a link back to the main page ("Generate New")

Also update the main page to include a link to /history ("View History")
in the top right corner.

Keep it simple and clean.
```

### Test it

1. Go to /history in the browser
2. The entry you saved earlier should appear
3. Click it -- the full email and LinkedIn message should expand
4. Go back to the main page, generate a new one, save it, check history -- both should be there

**CHECKPOINT:** In the agent chat: `Commit all changes with the message 'add history page' and push to GitHub`

## Polish (Optional)

Pick one or two things to improve and ask Cursor to do them:

- Add a navigation bar with links between Generate and History
- Improve the colour scheme or add a logo placeholder
- Add placeholder text in the form fields (examples of what to type)
- Add a character count to the LinkedIn message field
- Make it mobile-responsive

**CHECKPOINT:** In the agent chat: `Commit all changes with the message 'polish and styling' and push to GitHub`

## Step 8: Deploy to Vercel

This is where your app goes live on the internet -- and, true to buildAcademy, the
agent can do it for you with the Vercel CLI. In the agent chat:

```
Deploy this project to Vercel using the Vercel CLI:
1. If the Vercel CLI isn't installed, install it.
2. If I'm not logged in, run "vercel login" and walk me through it — I'll approve
   the login in my browser.
3. Link this project to a new Vercel project called "outreach-generator", accepting
   whatever settings the CLI suggests.
4. Add my three environment variables from .env.local to the Production environment:
   NEXT_PUBLIC_SUPABASE_URL, NEXT_PUBLIC_SUPABASE_ANON_KEY, and OPENROUTER_API_KEY.
5. Connect my GitHub repo so future pushes auto-deploy (vercel git connect).
6. Deploy to production (vercel --prod) and give me the live URL.
```

The first time, a browser window opens to log in to Vercel -- approve it and the agent
handles the rest. After ~2 minutes you'll get a live URL -- open it; that's your app, on
the internet.

**Why the environment variables (step 4)?** `.env.local` only ever lived on your laptop,
and it never gets pushed to GitHub. Vercel needs its own copy of the secrets to run your
app. This is exactly how every production app works.

**Why connect the repo (step 5)?** From now on, every `git push` rebuilds and redeploys
your live site automatically -- no manual deploys after this.

(Prefer clicking? You can also go to vercel.com → **Add New → Project**, import
`outreach-generator` from GitHub, add the three environment variables, and click
**Deploy** -- same result.)

### Test the live app

- Open the URL on your phone
- Generate an email
- Save it to history
- Check the history page
- Verify the data appears in your Supabase dashboard

If something doesn't work:

- **App loads but AI doesn't work** -- the `OPENROUTER_API_KEY` env var probably didn't make it to Vercel. Ask the agent: `Check my Vercel production environment variables and add OPENROUTER_API_KEY from .env.local if it's missing, then redeploy.`
- **App won't build** — read the build error (the Vercel CLI prints it, or check the deployment log at vercel.com). Paste the error into Cursor and ask the agent to fix it, then commit, push, and Vercel redeploys automatically

---

## What Comes Next

You've built a fully working app. It's live, it generates real output, and it saves to a real database. But there are things we deliberately left out because they're beyond the foundations. Here's what you'd add next:

1. **Authentication** -- right now, anyone with the URL can use it. Adding login (Supabase Auth) means each person only sees their own data.
2. **Security** -- we turned off Row Level Security on the database. Before sharing this with others, you'd turn that back on and set rules for who can read and write data.
3. **Rate limiting** -- right now, someone could hit your Generate button 10,000 times and burn through your OpenRouter credits. You'd add limits.
4. **Error handling** -- what happens if OpenRouter is down? If the internet drops? Production apps handle these gracefully.
5. **Custom domain** -- right now it's on a `.vercel.app` URL. You can connect your own domain in Vercel's settings.

These aren't scary -- they're the next steps, and they're exactly what the ongoing community covers, module by module. You now understand the architecture well enough to ask Cursor to help you add them. The architecture is identical for any app -- swap the fields, change the prompt, redesign the table, and you can build for your own idea.

### Scope your own app (the SCOPE skill)

When you're ready to build your own idea, start the same way we started this one: scope it
first. This repo includes a reusable skill that runs the **SCOPE** interview for you and
writes a full PRD. In a Cursor agent chat, paste:

```
Follow skills/scope-to-prd/SKILL.md: interview me about the app I want to build,
one question at a time, then write my PRD to PRD.md.
```

The agent asks you the five SCOPE questions, then turns your answers into a complete plan
(see [skills/scope-to-prd/SKILL.md](skills/scope-to-prd/SKILL.md) and the example
[PRD.md](PRD.md)) -- the same move you'll repeat for every app you build after this.

---

## Quick Reference

### Useful Cursor shortcuts


| Shortcut                                   | What it does         |
| ------------------------------------------ | -------------------- |
| Cmd+L (Mac) / Ctrl+L (Windows)             | Open agent chat      |
| Cmd+Shift+J (Mac) / Ctrl+Shift+J (Windows) | Open Cursor settings |
| Ctrl+`                                     | Open the terminal    |


### Checkpoint prompt

Copy and paste this every time, changing the message:

```
Commit and push all changes
```

### Reset Rule prompt

If something breaks and you want to go back to the last checkpoint:

```
Revert all uncommitted changes back to the last commit
```

Then start a new agent chat and try again with a clearer prompt.

### Your secret keys

Keep these somewhere safe -- you'll need them for Vercel deployment:


| Key                             | Where to find it                            |
| ------------------------------- | ------------------------------------------- |
| `NEXT_PUBLIC_SUPABASE_URL`      | Supabase dashboard, then Settings, then API |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase dashboard, then Settings, then API |
| `OPENROUTER_API_KEY`            | openrouter.ai, then Keys                    |


---

## Schedule

Each night follows the same shape: a short recap and demo, 90 minutes of building, then homework and questions.


| Time          | What's happening                                                         |
| ------------- | ------------------------------------------------------------------------ |
| 5:30pm        | Meet link opens -- test audio, video, and screen-share                   |
| 6:00 - 6:15pm | What we're covering tonight -- recap last week, demo tonight's end state |
| 6:15 - 7:45pm | Content and build -- the teaching and the hands-on build                 |
| 7:45 - 8:00pm | Homework, next week, and Q&A                                             |


**The three weeks:**


| Week          | Date       | Focus                                                                       |
| ------------- | ---------- | --------------------------------------------------------------------------- |
| Setup (video) | Before W1  | Installs and accounts -- get your laptop build-ready                        |
| Week 1        | Tue 23 Jun | Scope & Scaffold -- scope the app, project setup, GitHub, the Generate form |
| Week 2        | Tue 30 Jun | Database & AI -- Supabase, MCP, the AI feature, results, save               |
| Week 3        | Tue 7 Jul  | Debug & Ship -- debugging workshop, history page, deploy live               |


---

## Troubleshooting

### Supabase MCP won't connect

If the **supabase** server shows an amber or red dot in **Settings → Cursor Settings →
Tools & MCP**, work through these in order:

1. **Click Connect / log in again.** An amber dot with **"Needs authentication"** just means you haven't approved access yet -- click **Connect**, log in to Supabase, select your organization, and click **Authorize Cursor**.
2. **Fully quit and reopen Cursor.** MCP servers only load when Cursor starts, so a brand-new entry sometimes doesn't appear (or stays grey) until a full restart -- not just reloading the window.
3. **Check the config file.** In an agent chat: `Open ~/.cursor/mcp.json and check that the "supabase" server has the url https://mcp.supabase.com/mcp and that the whole file is valid JSON.` A stray comma or a wrong URL is the usual culprit.
4. **Make sure you're logged into Supabase in your browser**, then click **Connect** again -- the approval is much smoother when you're already signed in.

If it still won't connect after a restart and re-login, delete the **supabase** entry and re-add it with the setup prompt (*Connect Your Tools → Connect Supabase to Cursor (MCP)*).