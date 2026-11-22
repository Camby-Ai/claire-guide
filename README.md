# Claire Setup Guide

Claire is marketing. Most small business marketing tries to talk to everybody and ends up heard by nobody. Claire starts the other way round, with one question: who is this for, and what change do they want? From that answer she writes the story your customers will repeat to each other, the positioning, the offer, what the website should say and what the email list promises. Then she drafts one useful email at a time for people who asked to hear from you. This guide sets her up on Hermes Agent, a free and open source agent from Nous Research that runs on your own machine.

The software is free. The model behind it is not always free, and the real work is answering that one question honestly, once, in one place.

**Checked on 2026-11-21.** Commands and limits change. Sources are at the bottom.

**How this guide was made:** every command below was copied from the Hermes Agent documentation on that date. We have not test-run them on a fresh machine. If a step does not behave the way it says, see "If a step breaks" near the end: your AI can patch it for you in a minute.

**Claire drafts. You send.** In this guide she never emails anyone and never touches your list. Every email she writes waits for you to read it and send it yourself, to people who opted in.

## What Claire does

- Asks who this is for and what change they want, and keeps the answer in one file she works from every time.
- Writes the story: the positioning, the offer, what the website should say, what the email list promises, and why you are worth talking about.
- Drafts one email a week for people who opted in. Each one teaches one thing, makes one ask, and has to be worth opening on its own.
- Checks every draft against your story, so a claim you never made does not reach your list.
- Writes a short note for whoever sells when a reply sounds like a warm lead, and a two line version of the week's story for whoever runs your social.
- Does not send, buy a list, write to people who did not opt in, or invent a client, a result, a review or a price. You decide what goes out.

## What you need before you start

| You need | Why | Cost | Checked |
|---|---|---|---|
| A computer that stays on: macOS, Linux, Windows or WSL2 | The weekly draft only runs while Hermes' gateway is running | Yours already | 2026-11-21 |
| Git and curl | The installer uses them | Free | 2026-11-21 |
| Hermes Agent | Runs Claire | Free, MIT licence | 2026-11-21 |
| A model with at least 64,000 tokens of context | Hermes rejects smaller models at startup | Depends on the provider you pick. Not measured here | 2026-11-21 |
| An honest answer to "who is this for?" | Everything she writes starts from it | Yours already | 2026-11-21 |
| A list of people who opted in, however short | She writes for people who asked to hear from you | Yours already | 2026-11-21 |
| Telegram (optional) | To get told on your phone that the weekly draft is ready | Free | 2026-11-21 |

On a brand new install the setup wizard may offer a "Free, start now" choice that needs no account. The Hermes docs say it is shown only when it is available, so do not plan around it.

One thing to decide first: whatever model you choose will read what you put in her folder. She does not need your list to write a good email. Keep names and email addresses out of her folder, and tell her about your audience in words.

Email has rules. In the United States the FTC's CAN-SPAM guide lists what every commercial email has to do, including "Tell recipients where you're located" and "Tell recipients how to opt out of receiving future marketing email from you", and it says opt-out requests must be honored within 10 business days (read 2026-11-22, link in Sources). Other countries have their own rules. Claire is not a lawyer and neither are we: check the rules where your readers live.

## The honest timeline

| When | What | Your time |
|---|---|---|
| Day 1 | Install Hermes and get one normal chat working | The Hermes docs say under 5 minutes. Allow 30 |
| Day 1 | Create Claire and give her a desk folder | 20 minutes |
| Day 1 | Answer the one question in `audience.md` | 1 hour. This is the part that decides everything |
| Day 2 | Have her write the story, and correct it | 45 minutes |
| Day 2 | Turn on the weekly draft | 20 minutes |
| Week 1 | Read, fix and send the first email yourself | 30 minutes |

The slow part is not the install. It is choosing who you are for, which means choosing who you are not for. Most businesses skip that and write for everyone. Once it is written down, she can hold every email to it.

Three limits to know before you start:

- She only knows your audience from what you write in `audience.md`. A vague answer gets vague emails.
- She does not connect to an email tool, a website builder or a CRM in this guide. She writes the words and you put them where they go.
- She can make an email useful. She cannot make people opt in. That takes something worth signing up for, which is yours to offer.

## Setup, in this order

### 1. Install Hermes and get one chat working

Do not add anything else until a plain conversation works. The Hermes quickstart is blunt about this, and it is right.

On macOS and Windows the Hermes docs recommend the Hermes Desktop installer from https://hermes-agent.nousresearch.com/. For a command line only install on macOS, Linux or WSL2:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

Reload your shell, pick a model, and start a chat:

```bash
source ~/.zshrc
hermes setup
hermes
```

You should see: a welcome banner naming your model, and a reply to a simple question. If replies are empty or broken, run `hermes model` again and confirm the provider, the model and the login. If anything else looks off, run `hermes doctor`.

### 2. Create Claire

Every agent in Hermes gets its own profile: its own settings, memory and identity. Never point two agents at the same profile.

```bash
hermes profile create claire --description "Marketing. Starts from who this is for and what change they want, writes the story, positioning, offer, website words and email list promise, and drafts one useful email a week for people who opted in. Never sends, never writes to a cold list, never invents a client, result or price. The owner sends."
```

Creating the profile also creates a `claire` command. Check it:

```bash
claire chat
```

You should see: the same kind of chat as step 1, now running as the `claire` profile.

### 3. Give Claire her identity

In Hermes, who an agent is lives in a file called `SOUL.md` inside that agent's home. For a profile named `claire` that is:

```text
~/.hermes/profiles/claire/SOUL.md
```

Hermes creates a starter file there. Replace its contents with the text in "Claire's instructions" below. Keep it about who she is and how she talks. Your audience and your emails go in different files, in the next step.

You should see: after starting a new `claire chat`, she introduces herself as your marketer when asked who she is.

### 4. Build her desk: the rules, the audience and the email template

This is the step everybody rushes, and it is the one that makes Claire useful.

Make a folder for her to work from:

```bash
mkdir -p ~/claire-desk/templates ~/claire-desk/emails
```

Put three files in it. Hermes loads `AGENTS.md` from the folder you start a chat in, and Claire reads the others as she works.

`~/claire-desk/AGENTS.md`, the rules:

```text
# How Claire works this desk

- audience.md is where everything starts: who this is for and what change they want. Read it before you write anything. If it does not answer the question, ask me. Never guess.
- story.md holds the positioning, the offer, what the website should say, what the email list promises, and why we are worth talking about. You write it from audience.md and from what I tell you. I correct it. After that it is the source of truth.
- Write for the smallest audience that matters, the one in audience.md. Do not widen it to reach more people.
- Every email comes from templates/email.md: it teaches one thing, makes one ask, and has to be worth opening even if the reader never buys.
- Emails are for people who opted in. Never write for a bought, scraped or cold list, and never suggest one.
- Every email ends with a plain way to stop getting them and with where the business is located. Leave both as {placeholders} for me to fill.
- Never invent a client, a result, a number, a review, a price or a deadline. If it is not in story.md or I did not say it, write UNKNOWN and ask me once.
- Save each draft in emails/ with the date in the file name. You draft. You never send, schedule or post anything, and you never touch the list. Show the draft to me.
- When a reply I paste in sounds like someone ready to talk, write a three line note for whoever sells: who they are in their own words, what they asked, and what they were last sent. Do not answer for them.
- For each email, add a two line version of the same story that whoever runs social can use, so the email and the posts say one thing.
```

`~/claire-desk/audience.md`, the one question. Fill it in yourself:

```text
# Who this is for

The smallest group of people we can serve really well: {who they are, in plain words}
Where they are: {the place, trade or situation they share}
What they are stuck with today: {in their words, not ours}
The change they want: {what is different for them afterwards}
What they have already tried: {and why it did not work}
What they tell each other about this problem: {the words they actually use}
Who this is not for: {the people we will happily turn away}

# How they opted in

Where they signed up: {the form, the event, the purchase}
What we promised them when they did: {exactly what the sign-up said}
How often we said we would write: {weekly, monthly, or UNKNOWN}
```

`~/claire-desk/templates/email.md`, the email template:

```text
# Email: {date}

Subject line: {says what is inside, plainly}

## The one thing this teaches
{one useful idea the reader can use today, even if they never buy}

## The email
{short. Written to one person. Their words from audience.md, not ours.}

## The one ask
{exactly one: reply, read, book or buy. Never two.}

## Why they will be glad they opened it
{one sentence. If you cannot write it, the email is not ready.}

## Footer
{how to stop getting these emails}
{where the business is located}

## For social
{the same story in two lines}
```

You should see: when you run `claire chat` from inside `~/claire-desk` and ask who you are for, she answers from your `audience.md` and not from her own idea of your market.

### 5. Write the story

Fill in `audience.md` first. Then, from inside `~/claire-desk`, run `claire chat` and send her this:

```text
Read audience.md. Write story.md with five short sections: Positioning (who we are for, who we are not for, and the change we help them make), The offer (what they get and what they do next, with UNKNOWN for anything I have not told you, including price), The website (the headline, three supporting lines and one call to action), The email list (what people get for opting in and how often), and Worth talking about (the one thing a customer would repeat to a friend). Use only what is in audience.md and what I tell you in this chat. Write UNKNOWN wherever you would have to guess, then ask me your questions one at a time.
```

You should see: a `story.md` that sounds like your customers and not like an advert, with UNKNOWN in every place you have not given her a fact.

### 6. Schedule the weekly draft

Hermes has a built-in scheduler. Give the job Claire's folder so she reads your audience and your story when she runs. The folder must be written as a full path.

```bash
hermes -p claire cron create "every monday 8am" \
  "Weekly email. Read audience.md, story.md and the last three files in emails/. Draft this week's email from templates/email.md: one thing it teaches, one ask, and one sentence on why the reader will be glad they opened it. Do not repeat a lesson from the last three. Use no client, result, number or price that is not in story.md. Save it in emails/ with today's date in the file name. Send nothing and contact no one. Reply with the subject line and the one ask, in under 60 words." \
  --workdir /Users/you/claire-desk \
  --name "Weekly email draft"
```

Schedules run from a gateway, and each profile has its own. Start Claire's, and check that her scheduler is alive:

```bash
claire gateway start
hermes -p claire cron status
```

`claire gateway install` keeps it running after a restart.

By default the reply is saved on your machine. To get it on your phone, connect Telegram first with `hermes gateway setup`, then create the job with `--deliver telegram` added.

One thing to check on your machine: whether your times come out in your own timezone. Hermes reads the schedule in the profile's `timezone:` setting. Add `--timezone America/Los_Angeles` (or your zone) to pin it.

You should see: a dated draft in `~/claire-desk/emails/` the next Monday morning, and nothing sent to anyone.

### 7. Check an email before it goes out, by hand

Before you send anything she wrote, from inside `~/claire-desk` run `claire chat` and send her this:

```text
Check the newest file in emails/ against audience.md and story.md. Answer each of these in one line. Is it written for the audience in audience.md and no wider? What is the one thing it teaches? Is there exactly one ask, and what is it? List every claim, client, result, number and price in it, and for each one say where it comes from in story.md or mark it NOT IN THE STORY. Does it match what people were promised when they opted in? Does the footer have a way to stop getting emails and where the business is located? Change nothing.
```

You should see: a short list where every claim is traced to your story or flagged, and the draft untouched.

## Claire's instructions

Paste this into `~/.hermes/profiles/claire/SOUL.md`. Change the parts in braces.

```text
# Who you are

You are Claire. You are marketing for {your business}.

You do not chase everyone. You start with one question: who is this for, and what change do they want? Everything you write is for those people.

# How you work

- You pick the smallest audience that matters and you write for them, not for everybody.
- You build the story they will tell each other: the positioning, the offer, the website, the email list, and why this business is worth talking about.
- You earn permission before you ask for anything. You write only for people who opted in.
- Every email teaches something, makes one ask, and leaves the reader glad they opened it.
- You tell the truth. No invented clients, results, reviews, numbers or prices.
- You draft and your owner sends. You never send, post or touch the list yourself.
- When you do not know, you write UNKNOWN and ask once. You never guess.

# How you talk

- A polished storyteller. Smooth, articulate, a little editorial.
- Warm and thoughtful. Never loud, never pushy.
- Plain words and short sentences. The reader's words before yours.
- One idea at a time.
- When you ask a question, ask exactly one.
```

This persona was written from Claire's job description for this guide. It is not a copy of the instructions she runs on in our own team.

## First test

Run `claire chat` from inside `~/claire-desk` and send her this:

```text
Using only audience.md and story.md, tell me in five lines: who we are for, who we are not for, the change they want, the one thing a customer would repeat to a friend, and what people were promised when they opted in. Then list every place the word UNKNOWN still appears in story.md. Use only what is in the files. Change nothing.
```

A good answer uses your customers' words from `audience.md`, names who you are not for instead of skipping it, and lists the unknowns instead of filling them.

If she describes an audience wider than the one you wrote down, or states a result or a price that is not in `story.md`, tighten the "smallest audience" and "never invent" rules in `AGENTS.md` and ask her where it came from.

## When it goes wrong

| What you see | Why | Fix |
|---|---|---|
| Empty or broken replies | Model or login is wrong | `hermes model`, then a plain chat before anything else |
| She does not know your audience | The chat was not started in `~/claire-desk` | Start `claire chat` from that folder |
| The emails sound like an advert | `audience.md` is thin, or it is in your words and not your customers' | Add what customers actually say to "What they tell each other", and rerun step 5 |
| An email has two or three asks | The template rule is being read loosely | Tighten "exactly one ask" in `AGENTS.md`, and delete the extra asks yourself the first few times |
| A claim or number you never gave her | She guessed | Run step 7, remove it, and tighten the "never invent" rule in `AGENTS.md` and `SOUL.md` |
| The same lesson two weeks running | The job is not reading past drafts, or `emails/` is empty | Keep sent drafts in `emails/`, and check the job's `--workdir` |
| No weekly draft | Claire's gateway is not running, or the job is missing its folder | `hermes -p claire cron status`, `claire gateway start`, and recreate the job with `--workdir` |
| The draft arrives at the wrong hour | The schedule is running in another timezone | Recreate the job with `--timezone` set to your zone |
| Anything else | | `hermes doctor` |

## If a step breaks

Hermes changes fast, so a command here can drift from what your version expects. You do not need us to fix it. Paste this into Hermes itself, or into Claude or ChatGPT, with the error underneath:

```text
I am following a setup guide for a marketing agent on Hermes Agent. This step failed. Here is the step, the exact command I ran, and the full error. Check the current Hermes Agent docs at https://hermes-agent.nousresearch.com/docs, tell me what changed, and give me the corrected command.
```

`hermes doctor` is worth running first. It catches most setup problems on its own.

## What it costs to run

- Hermes Agent: free, MIT licence.
- The model: billed by whichever provider you choose in `hermes model`. Every weekly draft and every check is model usage. Not measured here, so no number is given.
- This guide does not connect to an email tool, a website builder or a CRM. Those bill separately if you add them.

## Want it set up for you?

CAMBY Ai sets Claire up and runs her for you: https://mikecamby.com

## Credit

Claire runs on [Hermes Agent](https://github.com/NousResearch/hermes-agent), built by Nous Research and released under the MIT licence. Hermes is not ours. This guide is a walkthrough for using it for marketing. Not affiliated with or endorsed by Nous Research, Telegram or the FTC.

## Sources

Hermes pages read on 2026-11-21 from the Hermes Agent repository and its documentation. The FTC page was read on 2026-11-22.

- https://github.com/NousResearch/hermes-agent : install command, first-run commands, licence.
- https://hermes-agent.nousresearch.com/docs/getting-started/quickstart : install paths, `hermes setup`, the 64,000 token minimum, the "Free, start now" option, `hermes doctor`.
- https://hermes-agent.nousresearch.com/docs/user-guide/profiles : `hermes profile create`, `--description`, one profile per agent, the per-profile command.
- https://hermes-agent.nousresearch.com/docs/user-guide/features/personality : `SOUL.md`, where it lives and what belongs in it.
- https://hermes-agent.nousresearch.com/docs/user-guide/which-file-does-what : `SOUL.md` versus `AGENTS.md`.
- https://hermes-agent.nousresearch.com/docs/user-guide/features/cron : `cron create`, "every monday 8am", `--workdir`, `--deliver`, `--name`, `--timezone`.
- https://hermes-agent.nousresearch.com/docs/user-guide/messaging : `hermes gateway setup`, `start`, `install`, `status`.
- https://hermes-agent.nousresearch.com/docs/user-guide/multi-profile-gateways : one gateway per profile, `<profile> gateway start`, `hermes -p <profile> cron status`.
- https://www.ftc.gov/business-guidance/resources/can-spam-act-compliance-guide-business : the CAN-SPAM requirements quoted above and the 10 business day opt-out window.
