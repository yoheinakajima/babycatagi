# BabyCatAGI 👶🐱🤖

BabyCatAGI is **OG BabyAGI mod #3** in the **Baby<animal>AGI** series.

It builds on what we learned from **BabyBeeAGI**, with a focus on:

* **Higher objective completion**
* **Lower errors**
* **Faster runs** (GPT-4 task creation happens once)
* A **hand-crafted mini-agent** as a tool

✨ ~300 lines of code

Repo: [https://github.com/yoheinakajima/babycatagi/](https://github.com/yoheinakajima/babycatagi/)

---

## Quick lineage

If you’re coming in fresh, the mental model is:

* **OG BabyAGI (babyagi_og)**: the original minimal commit — three agents in a loop (task execution / creation / prioritization), GPT-3 only, designed to run forever.
* **BabyBeeAGI**: introduces a combined task manager + dependencies + tools + (usually) stops.
* **BabyCatAGI**: restructures the loop for speed + adds **multiple dependencies** + adds a **mini-agent tool**.

OG codebase: [https://github.com/yoheinakajima/babyagi_og](https://github.com/yoheinakajima/babyagi_og)

---

## What’s different vs BabyBeeAGI

### 1) Task manager runs *before* the loop

BabyBeeAGI ran task management *inside* the loop. With GPT-4 in the mix, that gets slow fast.

BabyCatAGI moves **task creation** up front:

* GPT-4 is used **once** to create the initial task list
* Then the loop executes tasks without constantly re-planning

Net effect:

* Faster
* Cheaper
* Fewer rate-limit headaches

### 2) Multiple dependencies

BabyBeeAGI supported a single dependency.

BabyCatAGI lets a task depend on **multiple prior tasks** (pulling outputs from more than one task).

This is super useful for workflows like:

* Run two web searches
* Scrape / extract from both
* Compare or synthesize in a third step

“Memory” here is still simple and local: **a task array** holding outputs (not a vector DB).

### 3) Mini-agent as a tool

BabyCatAGI has two main tools:

* **`text-completion`** (default)
* **`web-search`** (optional, enabled if you set SERPAPI)

…but the **web-search tool is actually a mini-agent**:

* Google searches the query
* Loops through results
* Scrapes each URL
* Chunks long pages
* Runs an extraction pass to keep only info relevant to the objective + task

It’s a simple but surprisingly effective way to get more signal out of web results without stuffing everything into one prompt.

---

## Tools

By default:

* **text-completion** is enabled

If you set a `SERPAPI_API_KEY`:

* **web-search** turns on automatically (via SerpAPI)

In this version, web-search is where the “cat brain” lives — it’s not just returning links, it’s doing a lightweight crawl + extraction pass.

---

## Quick start

At the top of the script, set:

* `OPENAI_API_KEY`
* `SERPAPI_API_KEY` *(optional — enables web-search)*
* `OBJECTIVE`
* `YOUR_FIRST_TASK`

Run it.

You’ll see:

* Objective
* Generated task list
* Each task’s output
* A running session summary of everything collected

---

## Notes on results

In practice (so far):

* Completion rate is **much higher**
* Error rate is **lower**
* Still not consistent enough to trust without review
* Rate limits are the biggest constraint

A practical balance that’s worked well:

* **5 search results** per query

---

## The vibe

BabyCatAGI is basically:

> “Stop replanning every step, let tasks run, and when you do web search… do it like a tiny agent.”

Meow!!! 🐱
