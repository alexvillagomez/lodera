# Lodera

Adaptive math test-prep that's live in production with paying users. It's a question bank for standardized math where students practice against a huge pool of auto-generated, instantly graded questions, and the app keeps modeling what each student knows so it can serve the next best problem.

**Live site:** [lodera.ai](https://lodera.ai)

This repo is a write-up, not the source. Lodera is a commercial product so the code stays private, but I wanted a place to explain how it actually works.

## What it does

Students pick topics and practice. Behind that simple loop, the app is doing a few things that make it more than a quiz app:

- **A real mastery model.** Every answer updates a latent-ability estimate through an IRT/Elo model. I report "mastery" as the difficulty a student clears about 80% of the time, not raw percent correct, which is a much more honest signal. New topics run a quiet cold-start calibration so strong students don't have to grind through the easy stuff first.
- **Adaptive serving.** A serving engine aims each question at the student's current ability, with diversity and priority logic so practice doesn't repeat or pile up on a single idea.
- **Content is a pipeline, not a static bank.** Parametric templates expand into tens of thousands of unique, machine-graded questions with misconception-based wrong answers. Each one gets auto-tagged to a learning taxonomy using embeddings plus an LLM judge, so the content scales without me hand-writing every question.
- **A Challenge arena.** A two-sided Elo system rates hand-curated puzzles across a bunch of subjects (statistics, linear algebra, discrete math, geometry, and more) and recalibrates their difficulty from real play.

## Stack

- Next.js 15, React 19, and TypeScript, deployed on Vercel with push-to-main continuous deploy
- Supabase and Postgres, around 25 tables locked down with row-level security, with all data access going through typed server-side API routes
- A standalone Python and SymPy pipeline for authoring, generating, tagging, and importing content
- Supabase Auth for accounts and Stripe for subscriptions, with a free daily tier and a trial

## Why I built it

I wanted to build a learning tool that adapts to the student instead of handing everyone the same worksheet, and I wanted the recommendation and difficulty modeling to be real rather than cosmetic. It's also where a lot of my recommendation-systems work started, since serving the right question at the right difficulty is the same core problem as any ranking or recommendation system.
