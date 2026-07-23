# dukkho.com — Creative & Technical Blueprint

*A melancholic museum. Five exhibits. One room tone.*

**Constraints this blueprint is built around:** solo builder, non-technical, AI-agent-assisted, near-zero infrastructure budget. Every recommendation below is chosen to survive those three facts. Novelty lives in the art. The infrastructure is deliberately boring.

---

## 0. The Thesis

Before anything else, the site needs one sentence it is arguing.

> **You cannot hold anything still. Trying to is what breaks it.**

Every exhibit is a proof of that sentence. The flame you shield goes out. The pet you love dies. The moment you refuse to blink past is the moment you lose the rest of the life. The stranger arrives and leaves on a timer. The sorrow you burn is gone even though you wrote it.

Hold this line at the center. When a design decision is ambiguous, ask which option argues the thesis harder. That is your art direction, not a mood board.

---

## 1. The Aesthetic & Vibe

### 1.1 The black is not black

`#000000` is a developer's black. It reads as a broken page or an OLED void, and it kills every shadow you might want to cast. Real cinematic darkness has a temperature.

The whole palette is built on one contrast: **the world is cold, the light is warm.** Grief is blue-black. Love is amber. There is no third color. That restraint *is* the brand.

| Token | Bengali | Hex | Use |
|---|---|---|---|
| `--kalo` | কালো | `#0A090C` | The void. The base of everything. Never pure black. |
| `--chhai` | ছাই | `#2A2724` | Ash. Dead warm grey. Spent things, borders, the graveyard. |
| `--dhoa` | ধোঁয়া | `#6B6B73` | Smoke. **All body text.** Deliberately low contrast. |
| `--chand` | চাঁদ | `#E8E4DC` | Bone-white. Headlines only, and rarely. Never `#FFF`. |
| `--diya` | দিয়া | `#FF9E3D` → `#E8622C` | The only saturated color on the site. Flame, life, the pet. |

**Rules, absolute:**

- No pure white anywhere. Ever. Bone or nothing.
- Body text sits at roughly 4.2:1 contrast, not 7:1. Grief is not high contrast. (Ship a "raise contrast" toggle in settings — this is a deliberate aesthetic choice, so give people a way out of it rather than shipping something genuinely unreadable.)
- Amber appears on screen only when something is alive. When it dies, the amber leaves the page and never returns to that page.
- Zero gradients as decoration. Gradients only as light falloff from an actual light source.

### 1.2 Typography

The Bengali is the hard part, and it's where most bilingual sites betray themselves. Bengali cannot be an afterthought that inherits the Latin type scale.

- **Bengali:** `Anek Bangla` (variable, Ek Type — genuinely well-drawn, free) with `Noto Serif Bengali` as the serif voice for the emotional/display moments.
- **English display:** `Instrument Serif` — free, high-contrast, the closest thing to an A24 title card on Google Fonts.
- **English body:** `Newsreader` at low optical size, or `Fraunces` if you want more character.
- **UI/data:** one mono (`JetBrains Mono`) for timestamps, counters, leaderboards. Data should look recorded, not designed.

**The detail that separates real bilingual design from translated design:** Bengali needs a size multiplier of roughly **1.10–1.15×** against Latin at the same visual weight, because of its x-height and the density above the matra line. Build this into your type scale as a per-script variable on day one. If you retrofit it later you will rewrite every component.

**Use the matra as a design system.** The horizontal line Bengali letters hang from is your rule, your divider, your underline, your progress bar. Every horizontal line on the site should feel like it belongs to the same family as the matra. That is how the site becomes *Bengali* rather than *a dark site with Bengali on it*.

### 1.3 Motion

Everything is roughly **4× slower than feels correct.** The standard web easing is 200–300ms; here you're at 800–2000ms. Use a long-tailed expo-out (`cubic-bezier(0.16, 1, 0.3, 1)`), stretched.

- **Nothing bounces.** Bounce reads as playful, which is the enemy. Things fall and settle, with weight.
- **No hover state that swaps a color.** Hover is a slow bloom of light that takes 600ms to arrive and 1200ms to leave.
- **No loading spinners, anywhere, ever.** Loading is a held breath: a fade, or the diya guttering. A spinner is the single fastest way to make this feel like a web app.
- **Text is written, not faded in.** Character-by-character with *irregular* timing — pauses, hesitations. Uniform typewriter effects read as robotic; the irregularity is what reads as a person.
- **The cursor is a candle.** A soft radial light that follows the pointer. On the darkest pages, content is only fully legible near it — you read by candlelight. (Pointer-fine only, disabled under `prefers-reduced-motion`, and never on touch.)
- Use CSS `animation-timeline: view()` for scroll-driven work. Zero JS, and it won't fight the OS the way scroll-hijacking libraries do. **Caveat: this is not Baseline yet** — Chrome, Edge, Opera and Safari 26+ ship it, but as of Firefox 152 (June 2026) it's still behind a flag in stable, putting global support around 82%. It's an Interop 2026 priority so that should resolve, but until it does, treat scroll-driven motion as *enhancement* and make sure the page reads correctly with the animation simply absent. Given the audience is largely mobile Chrome and Safari, this is an acceptable trade — just don't hang anything load-bearing on it.

### 1.4 Sound — the highest-leverage thing on this list

Most "emotional" websites have no sound, and so they feel like documents. Sound is what makes this a room.

**The site has a room tone.** A bed at roughly −40dB that you never consciously hear: room hum, a ceiling fan, distant rain, tape hiss. Its entire job is to be missed. When it cuts, the body notices before the mind does.

**Autoplay is blocked, so make the unlock a ritual.** Not a mute button in a corner — an entry gate: *"এটি শব্দসহ দেখা উচিত।"* / *"This is meant to be heard."* One click, which both unlocks the audio context and consecrates entry. The browser's restriction becomes your front door.

**Instrumentation — avoid orchestral sadness, it's a cliché and it's expensive.** Use instead:

- Harmonium drone (the sound of Bengali domestic music, and it does grief without asking)
- Bowed metal, singing bowl, prepared piano
- Field recordings of Dhaka: rain on a tin roof, crows, a distant azaan, a rickshaw bell, a ceiling fan, a pressure cooker
- Everything through slight tape wobble. Nothing pristine.

**Interaction sounds are physical, never digital.** No clicks, no beeps, no UI blips. A match strike. Cloth. Paper. Breath. A latch.

**And silence is your loudest instrument.** When the pet dies, the room tone cuts to *absolute digital silence* for four seconds before anything appears on screen. That is the most devastating audio you can write and it costs zero bytes.

### 1.5 Micro-interactions that make it feel precious rather than shipped

- **No navigation bar.** A museum has rooms, not a menu. Moving between exhibits takes a moment of walking down a dark hallway. The friction is the point.
- **No modals, no toasts, no cookie banner shaped like a cookie banner, no footer with social icons in a row.** Any one of these instantly converts the museum back into a product.
- **The site remembers you without an account.** A device-local identity, given a name rather than chosen. You return and it says *"তুমি তিন দিন আগে এখানে ছিলে।"* — intimacy with zero auth, zero PII, zero cost.
- **Scarcity as a mechanic.** Some things you can do exactly once, ever, per person. This is free to implement and it is the entire emotional engine — a thing you can repeat is a feature, a thing you get once is an experience.
- **The tab title is a mechanic.** When you leave the pet's tab, `document.title` becomes *"তুমি কোথায়?"* Tab-away is a behavior the museum can respond to.
- **The favicon is state.** The diya in the favicon goes out.
- **Empty space is the lead character.** 60%+ void on every screen. One thing at a time. Two competing elements is a bug.

---

## 2. The Tech Stack

Two principles, and they're both about you being one non-technical person.

**Principle one: choose the most boring, highest-training-density tools available.** An AI agent writes Next.js + TypeScript + Tailwind + Postgres almost flawlessly, because it has seen millions of examples. It writes obscure frameworks confidently and wrongly, and you will not be equipped to tell the difference. Every point of stack novelty is a bug you cannot debug. **Put all your novelty in the art.**

**Principle two: nothing you have to babysit.** No servers, no containers, no Redis, no queues. Everything scales to zero.

### 2.1 The stack

| Layer | Choice | Why |
|---|---|---|
| Framework | **Next.js (App Router) + TypeScript + Tailwind** | Highest training density of any web stack. Agents are near-flawless here. |
| Animation | **Motion** (ex-Framer Motion) | The default; enormous corpus; handles the long-easing work cleanly. |
| **Hosting** | **Cloudflare Workers/Pages** via `@opennextjs/cloudflare` | **Not Vercel — see 2.2. This is the single most important line in this document.** |
| Persistence | **Supabase** (Postgres + Auth + Storage) | 500MB DB, 50k MAU, 200 concurrent realtime connections, 2M realtime msgs/mo on free. |
| Realtime/presence | **Cloudflare Durable Objects** (or PartyKit, which wraps them) | Now available on the **free** Workers plan with SQLite storage. Perfect fit for pairing two strangers. |
| Assets | **Cloudflare R2** | 10GB free and — critically — **zero egress fees.** S3 egress would eventually bankrupt an audio-and-video-heavy art site. |
| Face tracking | **MediaPipe Face Landmarker** (`@mediapipe/tasks-vision`) | Runs fully on-device. See 2.4. |
| Audio | **Howler.js** for the bed, raw Web Audio API for anything reactive | Don't reach for Tone.js unless you end up doing generative music. |

**The architectural rule that keeps two platforms from becoming complexity:**

> **Supabase is memory. Durable Objects are presence.**

Anything that must persist and be queried later — graves, leaderboards, saved words, diaries — is Supabase. Anything ephemeral and live — two strangers in a room, flames burning right now — is a Durable Object that deletes itself. Each tool does the one thing it is uniquely good at, and you never have to think about which to use.

### 2.2 ⚠️ Do not deploy this to Vercel

This is load-bearing and worth being blunt about. Vercel's Hobby plan is **restricted to non-commercial personal use**, and their definition of commercial explicitly includes *"any method of requesting or processing payment from visitors of the site."* They also reserve the right to disable a Hobby deployment **with or without notice, for any reason or no reason.**

The moment the first 10 BDT bKash payment goes through, dukkho.com is a commercial deployment on a plan that forbids it. Vercel actively enforces this. Pro is $20/mo/seat, which is real money against a bootstrapped budget and a 10 BDT price point.

Cloudflare's free plan permits commercial use, gives 100,000 requests/day, and now includes Durable Objects. Deploy Next.js there via `@opennextjs/cloudflare` from day one. Migrating later is a genuinely annoying week; starting there is an afternoon.

### 2.3 The pet's death clock — do not use cron

The instinct is a nightly job that scans every pet and kills the ones whose time is up. Don't. Free-tier schedulers are unreliable, and the query gets worse every day you succeed.

**Do this instead: roll the death timestamp at birth and never run anything.**

When a pet is created, deterministically derive `death_at` — somewhere between day 21 and day 56 — seeded from the pet's ID, and store it. The pet is alive if and only if `now() < death_at`. There is no background process. Nothing executes. Death isn't performed, it's *discovered* — you find out when you show up.

This is simultaneously the cheapest possible implementation, tamper-proof (server derives it, client never sees it), infinitely scalable, and **the most emotionally honest version of the mechanic.** The universe didn't decide to kill your pet today. It was always going to be that day. You just weren't there.

The one job you do want is a single daily cron for out-of-app notification, and it stays tiny because it's an indexed lookup: `WHERE death_at BETWEEN (now - 24h) AND now AND notified = false`.

Same lazy-evaluation trick applies broadly. Generate the diary at death from the stored event log, not nightly. Compute leaderboard ranks on read, not on write.

### 2.4 The webcam piece is free and private, and you should say so loudly

MediaPipe's Face Landmarker returns 52 blendshape coefficients including `eyeBlinkLeft` (index 9) and `eyeBlinkRight` (index 10). Threshold both above ~0.5, require both eyes, debounce to avoid double-counting a single blink. It runs entirely in the browser via WASM/WebGPU.

**No video frame ever leaves the device. There is nothing to upload, store, or breach.** That is both a genuine privacy guarantee and a $0 line item, and it should be stated in plain language *before* you trigger the permission prompt, not buried in a policy afterward.

Known failure modes to design for: glasses with glare, low light, low-quality front cameras, and reduced accuracy on darker skin tones in poor lighting. Build a short calibration beat ("look at the flame — blink twice") that doubles as a permission-warming ritual, and make the spacebar fallback fully supported and unapologetic.

### 2.5 Payments — a hard reality check

Direct bKash PGW / Tokenized Checkout requires a merchant account and real paperwork. Aggregators (SSLCOMMERZ, ShurjoPay) are faster to integrate but carry meaningful setup costs — SSLCOMMERZ's setup fee is on the order of BDT 15,000 — against mobile-wallet transaction fees of roughly 1.5–2%.

The percentage isn't the problem: 2% of 10 BDT is 0.2 BDT. The problems are **the fixed setup cost and the multi-week onboarding.**

Two consequences:

1. **Start the merchant paperwork on day one, in parallel with everything.** It is the longest-lead-time item in the entire project and it is not code, so it can run while you build.
2. **Reconsider what the 10 BDT actually buys.** See §3.1 — the current design charges people to grieve, which is both the wrong emotional shape and the worst possible conversion funnel.

### 2.6 Working with AI agents — build hygiene

Since agents are writing most of this:

- **One `CLAUDE.md` at the repo root** containing the design tokens, the motion rules, the "no pure white" law, the bilingual type rule. Every agent session reads it. Without this, exhibit four will not look like exhibit one.
- **One exhibit per branch. Never two at once.** Agents lose coherence across parallel surfaces faster than they lose it across depth.
- **Playwright screenshot tests.** You are non-technical, so you cannot read a diff and catch a visual regression. Let the machine catch it.
- TypeScript strict mode, no exceptions. It converts a class of bugs you can't debug into errors the agent fixes itself.

---

## 3. Elevating the Mechanics

One enhancement each. In every case I'm trying to do the same thing: turn a *feature* into a *bargain the user has to make.*

### 3.1 [1] Burn Your Dukkho → **Save One Word**

The current design has an emotional problem and a business problem, and they're the same problem: **a paywall in front of grief makes grief a transaction.** It also puts your hardest conversion ask in front of your most shareable moment.

**Invert it. The burning is free. What costs 10 BDT is keeping something.**

Type your sorrow. Before you strike the match, you may choose **exactly one word** from what you wrote to survive the fire. Everything else burns and is genuinely, permanently gone — never stored, never logged. The word you chose doesn't burn. It hangs in the ash, still glowing, and then it joins **the Wall** — a slow-drifting field of thousands of single Bengali words that strangers paid to save.

Why this is better on every axis:

- **The bargain is real and hard.** Which one word? That choice is the art. People will sit with it.
- **The emotional shape is correct.** You're not paying to grieve. You're paying to keep one thing. That's what people actually want and actually pay for.
- **It solves your privacy exposure entirely.** You never store a confession. You store one word a person consciously chose to make public.
- **It's inherently viral, and the viral object is beautiful.** "I paid to save the word মা." The Wall is a genuinely extraordinary artifact after ten thousand people.
- **Free burn = top of funnel.** Everyone burns. Some fraction saves. That's a working funnel; a hard paywall on step one is not.

Detail: the ash from every burn falls and accumulates at the bottom of the screen, persistent across all users. You are always standing in everyone else's ash.

### 3.2 [2] পোষা (The Pet) → **The Diary You Can Only Read After It Dies**

Sudden unexplained death is the right instinct, but it has a failure mode: it can read as a bug, or as random, and random isn't grief — random is noise. Grief has a *shape*. Specifically, grief is the retroactive re-reading of the last few weeks looking for the moment you should have known.

**So: the pet keeps a diary. One short line a day, in its own voice, about you. You cannot read a single word of it while it's alive.**

When it dies, the whole thing unlocks at once.

And the last entries contain the signs you missed.

> দিন ১৯ — আজ একটু ক্লান্ত লাগছে। তুমি অল্প সময় ছিলে।
> *Day 19 — I feel a little tired today. You didn't stay long.*

The warning was there. You visited for forty seconds that day. The diary generates from real behavioral data you're already storing — visit times, session lengths, gaps, when you named it, days you missed — so it is *true*. It's a record of an actual relationship, not a horoscope. Generate it lazily at death from the event log; it costs nothing until someone dies.

This converts a random event into a story with a shape, and it hands you your viral object: screenshots of a dead pet's last three diary lines.

**Two supporting mechanics:**

- **You cannot name it for the first three days.** It's just "it." Naming is the moment attachment becomes irreversible, so make it an earned, deliberate act rather than a form field on signup.
- **The graveyard sorts by date, not by name.** Go to your pet's grave and you find everyone whose pet died the same day. *"আজ ৪৭ জনের পোষা মারা গেছে।"* Grief cohorts. You are never the only one who lost something today, and that is the truest thing the site can say.

---

#### The visual form: জোনাকি — it is a firefly, and it is your only light

**Do not make it cute.** Cute is a toy, and nobody grieves a toy at full strength. Cute is also a mascot, which turns a museum into a brand. And cute is expensive — character art, expression sheets, animation states, a real illustrator on retainer.

On a pitch-black site whose entire palette is *one warm light in a cold room*, the pet has an obvious and much better form: **it is the light.**

A firefly. **জোনাকি.** A single small warm point in an enormous dark room, with a soft bloom around it. Tagore wrote a whole book called *Fireflies*; they are already deep in the Bengali poetic vocabulary, and they are famously, proverbially short-lived. The cultural work is done before you draw anything.

**No face. No eyes. No body. No hunger bar. No stats, ever.**

**Bonding is expressed entirely through motion.** This is the whole design, and it's about eight numbers interpolating over six weeks:

| | Days 1–3 · অচেনা | Days 4–10 · কৌতূহল | Days 11–28 · বাঁধা |
|---|---|---|---|
| Behavior | Flees your cursor | Orbits at a distance | Comes to you, rests |
| Motion | Fast, jittery | Wide slow circles | Slow, small, settled |
| Light | Dim, pale, cold | Warming | Full amber, deep pulse |
| Naming | Locked — it's just "it" | You may name it | — |

**It draws.** Its movement leaves a faint trail of light that slowly fades. That trail is the personality — you can read its mood in the line. Skittish scribble in week one; long calm arcs by week three. You are watching handwriting change.

**And the grave is the last thing it drew, frozen.** Every grave in the communal graveyard is a unique light-scribble — the final gesture of that one firefly, procedurally generated, free, and never repeated. A million tiny drawings by a million dead insects. That is the artifact, and it costs nothing to produce.

**It is your only light source, and this is the part that hurts.** The room is revealed only by the pet's glow. As it bonds and brightens, you gradually see more of where you've been sitting all this time. When it dies, the room goes black and you lose that too. You don't just lose the pet — **you lose the ability to see the room.**

**The decline is the masterstroke, and it's free.** In its last four or five days the firefly gets slower. Dimmer at the edges of its pulse. It moves less. It stays closer to you and rests longer.

**Which is indistinguishable from deep contentment.**

That is exactly what dying looks like from the outside, and exactly what love looks like from the outside, and the user *cannot tell them apart in real time.* No warning is withheld unfairly — the warning is on screen for five days, in plain sight, wearing the costume of the happiest week you've had together. Only the diary tells you afterward which one it was.

**Death has no animation.** You open the tab and the room is black. Four seconds of true silence. Then the last trail fades up, very faint. Then the diary unlocks.

**Why this is also the right call commercially:** the flagship is now the *cheapest* exhibit to build. One canvas, a point light, a trail buffer, and eight parameters. No sprite sheets, no character designer, no animation states, no art commission. Which means **you can move পোষা earlier in the schedule than Phase 3** — and since it's the one exhibit that needs weeks of real elapsed time, moving it earlier is worth a great deal.

On mobile, the cursor is your finger, and it comes to your finger. That is more intimate, not less.

### 3.3 [3] ধরে রাখো (Hold On) → **You Are Sheltered By Everyone Who Let Go**

Right now this is a solo endurance test, and endurance tests plateau — once you know your number, there's no reason to return.

**Make it collective. While you hold, you see faint distant flames — other people holding *right now*, in real time.** And as they let go, you watch their flames go out. The screen slowly empties around you.

Then the mechanic that turns it into art:

**When someone else lets go, your flame gets half a second of shelter.** Their falling shields you. Other people's failure is literally what keeps you alive. And at the end: *"You were sheltered by 214 people who let go before you."*

And the inverse, which is the actual gut-punch — **when *you* let go, someone else's flame steadies.** Your failure was useful to a stranger. Letting go stops being defeat and becomes a gift. That is a genuinely beautiful thing to make someone feel with a button.

**Leaderboard refinement:** raw-seconds rankings invite a weight on the spacebar. Instead run two boards — *longest held* and *let go closest to the storm's peak* — and surface the **median** more prominently than any rank: *"most people let go at 1:47."* Comparison against the median is emotionally legible in a way that "rank 4,412" never is.

**Anti-cheat, cheaply:** the wind is a seeded deterministic function of elapsed time, so it's identical for everyone (fair) and replayable server-side (verifiable). Require occasional micro-input — the wind shoves, you must nudge back — and a taped-down key fails on its own.

### 3.4 [4] চোখের পলকে (In a Blink) → **Blink To Let Them Live**

The current framing — *you physically cannot watch the whole life* — is a strong hook but it's a punishment, and punishment pieces get one visit and a shrug.

**Invert the mechanic and it becomes the thesis of the entire museum.**

**Each blink is a year. The more you blink, the longer the life gets.**

Sit rigid and strain not to blink, trying to hold onto each tableau, and the life ends short — they die young. Relax, breathe, blink normally, and they live to be old. **You must let go, repeatedly, for them to have a full life.** Trying to preserve any single moment is precisely what shortens the whole thing.

Same technology, no extra cost, and it says the thing the whole site is about.

The ending: it shows you a still of the exact tableau you were inside at the moment of the *last* blink. *"This is where you were when it ended."* Different for every person. And the closing card, which is the shareable one:

> **আপনি ৬১ বার চোখের পলক ফেলেছেন। আপনি তাকে ৬১ বছর দিয়েছেন।**
> *You blinked 61 times. You gave them 61 years.*

Personal, specific, non-identifying, and it makes people want to go back and try to give them more.

### 3.5 [5] একসাথে (Together) → **The Rain Bends Around Them**

Three minutes of silence with a stranger is conceptually perfect and, in practice, three minutes of a blank screen. There has to be *presence*, without language.

**For those three minutes, the only thing either of you can do is move. And where a person is, the rain parts around them.**

You can see them — not an avatar, not a name, just a region of the rain bending. That's it. And it is enough: people will circle each other, follow, approach, retreat, hide in a corner, trace shapes. Wordless embodied presence, and the entire drama emerges from two cursor positions over a WebSocket — comfortably inside the free tier.

**Then the one sentence, exchanged simultaneously.** Both of you type. Neither sees the other's until both have sent, or the timer runs out. They appear side by side. You get one read.

**Then both are permanently deleted.** Not archived, not "deleted." Gone. Render the fade word-by-word so it resists a screenshot. One read, then the sentence and the person are both gone at the same instant, which is the point.

**One last detail, and it's the one people will talk about.** After they're gone, you're told exactly two things about them — their local time and their weather. Nothing else, ever.

> *ওখানে তখন ভোর ৪:১২ বাজে। ওখানেও বৃষ্টি হচ্ছিল।*
> *It was 4:12 in the morning where they were. It was raining there too.*

Anonymous and devastatingly specific. **Ship the time alone first** — it comes free from the browser, needs no API, no key, and no vendor, and honestly it already does most of the emotional work by itself. Add weather in a later pass if it earns its keep.

One trap to avoid when you do: **Open-Meteo's free tier is licensed for non-commercial use only,** and once bKash is live you are commercial — the same category error as the Vercel Hobby plan. If you want weather, either budget for their paid tier or use a provider whose free tier explicitly permits commercial use (OpenWeatherMap and WeatherAPI.com are the usual candidates — read their current terms yourself before wiring anything up).

---

## 4. Execution Strategy

### The counterintuitive rule: do not build the flagship first

পোষা needs three to eight weeks of real elapsed time to test *one* death. You cannot iterate on it. If you start there, you will spend two months unable to tell whether the most important thing you're building works.

Build things you can test in a day. Start the pet's clock running in the background early.

### Phase 0 — The Room (weeks 1–2). No exhibits at all.

Build the museum, empty. Design tokens, the fonts, the black, the room tone, the entry ritual, the hallway, the anonymous device identity, the "you were here three days ago" memory.

**Gate: show the empty site to five people. If nobody reacts to a museum with nothing in it, no exhibit will save it.** Fix the room before you hang anything on the walls. This phase is also where you set the bilingual type system and the reduced-motion/sound-off paths — both are agony to retrofit and trivial to start with.

### Phase 1 — ধরে রাখো (weeks 3–4). Your aesthetic proof and your first viral object.

Simplest exhibit technically: one button, one canvas, one number, fully client-side, near-zero infra. But it exercises motion, sound, and duration under real emotional load. If the aesthetic works here it works everywhere.

Ship it standalone. Let it find an audience while you build the rest.

**In parallel, starting now: the bKash/aggregator merchant paperwork.** Longest lead time in the project, and it isn't code.

### Phase 2 — Burn (weeks 5–7).

You learn the dissolve shader and you build the payment rail once — every later monetization reuses it. Ship the free burn first, add Save One Word after it's live.

On the fire: **prototype in 2D canvas with a noise-driven alpha mask before reaching for WebGL.** A flow-noise dissolve with a hot emissive edge burning upward, over a canvas texture of the user's own text, looks ten times more expensive than it is. Escalate to react-three-fiber only if 2D genuinely isn't beautiful enough. Shippable beats pure.

### Phase 3 — পোষা (weeks 6–14, overlapping).

Start the data model and daily loop *during Phase 2*, because you need real time to pass.

**Test it with a dev flag where one day equals one hour.** Seed twenty friends on an accelerated clock and you can watch the entire birth-bond-death-diary arc in three days instead of six weeks. Then run one real cohort at true speed before opening it. The graveyard ships with it — a pet death with nowhere to bury it is unfinished.

### Phase 4 — একসাথে (weeks 15–18).

Realtime is your highest-risk infrastructure, and it has an audience prerequisite: matching fails when nobody's online. It needs Phases 1–3 to have built a population first.

**Solve the empty-lobby problem by scheduling it.** *একসাথে opens at 9pm.* Turning a liability into a nightly event is free and makes it feel like a ritual rather than a broken feature.

### Phase 5 — চোখের পলকে (weeks 19+).

Last, deliberately. A webcam permission prompt is the highest-friction ask on the internet and you should only make it of someone who already trusts you — by exhibit five, they do.

It's also the most content-heavy piece: the real cost here isn't the blink detection, it's producing the tableaux of an entire life as actual visual art. Budget for that as an art commission, not an engineering task.

---

## 5. Risks Worth Naming Now

**The pet carries a duty of care.** Some people will be genuinely affected — that's the point, and it's also a responsibility. You need a quiet, non-clinical offramp: not a crisis banner bolted on, but something honest and in-voice, *"কথা বলতে চান?"*, placed where a person who needs it will find it. Also give people the ability to decline a replacement — "I'd rather not have another" should be a supported ending, not a dead link. This is part of the work's integrity, not a legal checkbox.

**Never store the burn text, and say so on the page.** The strongest version of that promise is one you can keep architecturally rather than by policy.

**Supabase free-tier projects pause after seven days with no API requests**, and you get two projects. Before launch, keep it warm with a GitHub Actions cron. After launch it's a non-issue.

**The launch vector is not Product Hunt.** This is a Bengali-language cultural object. Its audience is Bangladesh, West Bengal, and the diaspora, and that audience lives on Facebook, not on tech-launch sites. Design for that — and note that **every exhibit must output one image worth posting.** The shareable artifact matters more than the site, because the artifact is what travels.

**"Free tier" and "free for commercial use" are different things, and the gap has already bitten this project twice** (Vercel Hobby, Open-Meteo). The moment you accept 10 BDT you are a commercial operator, and a meaningful share of generous free tiers quietly exclude you. Before adding *any* new dependency, read its terms for that specific word. Everything recommended in this document is either OFL/MIT/Apache licensed or explicitly commercial-safe on its free plan — keep that property.

**Scope discipline.** Five exhibits is genuinely a lot for one person. The blueprint above is ordered so that **you have a real, complete, shareable thing at the end of week 4** and again at week 7. If momentum or money runs out at week 10, you still have a museum with two extraordinary rooms — which is much better than five half-built ones.

---

## Open Questions

1. Who makes the tableaux for চোখের পলকে? That's the one genuinely un-automatable art cost in the project.
2. ~~Does the pet have a visual form?~~ **Answered: yes.** It is a firefly of light — see §3.2. Because that turned out to be nearly free to build, **reconsider the phase order and pull পোষা forward**, since it's the only exhibit gated on real elapsed time.
3. Is there a way in for people who don't read Bengali, or is that deliberately not the point? Both answers are defensible; the choice shapes the whole reach of the thing.
4. Does the firefly make sound? A pulse you can barely hear, syncing to its glow, would make its silence at death unbearable. Costs almost nothing. Worth prototyping alongside the visual.

---

*Sources for the platform/pricing claims are listed in the accompanying chat message. Verify the bKash and aggregator commercial terms directly before committing — those move fastest.*
