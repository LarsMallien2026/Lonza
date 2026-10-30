# IBM Bob Level 3 | 20-Minute Hands-On

**Theme:** Climb the AI Maturity Curve — **Assist → Delegate → Tailor** — while you stay in control at every step.
**Codebase:** Galaxium Travels (React/TypeScript frontend, Python/FastAPI backend, SQLite).
**Source material:** <https://ibm.github.io/bob-l3/>

> Bob is non-deterministic. Your output will differ from the examples. That is expected. If a result diverges, refine the prompt, edit by hand, or continue if the code still compiles.

---

## Time plan

| Min | Block | Stage | Goal |
|-----|-------|-------|------|
| 0–1 | Pre-flight check | — | Environment is ready |
| 1–6 | Exercise 1: Literate Coding | Assist | Add email validation, review the diff |
| 6–15 | Exercise 2: Plan → Review → Agent | Delegate | Plan seat classes, approve, implement the backend |
| 15–19 | Exercise 3: `/init` and Bob Rules | Tailor | Give Bob project knowledge and standards |
| 19–20 | Wrap-up | — | Recap and discussion |

**Prerequisite:** Complete the [Setup chapter](https://ibm.github.io/bob-l3/setup/) before the session. You need an authenticated Bob IDE, Python 3.8+, Node.js 18+, the cloned repo, and the app running.

---

## 0:00–0:01 Pre-flight check

1. In Bob IDE, open the `galaxium-travels` folder (Explorer, **Ctrl+Shift+E**).
2. Confirm the app loads at `http://localhost:5173/`.
3. If it is not running, execute `./start.sh` from the repo root (macOS/Linux). On Windows, follow the manual steps in the Setup chapter.

```bash
git clone -b bob-learning-path-branch https://github.com/IBM/galaxium-travels
cd galaxium-travels && ./start.sh
```

---

## 0:01–0:06 Exercise 1 | Assist: Literate Coding (5 min)

**Concept:** Write a plain-language instruction at an exact spot in a file. Bob edits that spot and shows a diff. Nothing changes until you accept.

### Steps

1. Open `booking_system_frontend/src/components/user/UserIdentification.tsx`.
2. Press **Cmd/Ctrl + I**, or click the magic wand, to enable Literate Coding mode.
3. Put the cursor on an empty line after the `useState` declarations (about line 19). Keep a blank line above and below. Type:

   ```
   validate email addresses
   ```

   Do not prefix it with `//` or `#`. Comment markers can cause `oldString`/`newString` errors.
4. Note the two buttons that appear: **Clear All** and **Generate**.
5. Click **Generate** (or **Cmd/Ctrl + Enter**).
6. Review the diff:
   - Green lines are additions, for example an `isValidEmail` helper and an `if (!isValidEmail(...))` check in `handleSubmit`.
   - Red lines are removals. Your instruction line is one of them.
   - Use the up/down arrows to step through each block.
7. Click **Accept All**.

### Verify (about 1 min)

1. Restart the app if the dev server does not hot-reload.
2. In the browser, go to **Flights → Book Now** and open the Sign In panel.
3. Enter a name and `$$$$$` as the email. It should be rejected.
4. Enter `name@ibm.com`. It should be accepted.

### Checkpoint

- What two options appeared after you typed the instruction?
- What happened to the instruction line after acceptance?

### Stretch (if time remains)

In `src/pages/Flights.tsx`, put the cursor on the empty line above `loadFlights` and enter: `The backend is unreliable at times. We need a retry logic here.`

---

## 0:06–0:15 Exercise 2 | Delegate: Plan → Review → Agent (9 min)

**Concept:** For multi-file work, Bob plans first in read-only **Plan mode**. You review the plan. Only then does **Agent mode** write code, in a fresh conversation.

### Part A — Plan mode (4 min)

1. Open the Agentic Sidebar and set the mode to **Plan**.
2. Keep permissions on manual or read-only for this exercise. Plan mode cannot edit code, but you are practicing the habit of dialing autonomy deliberately.
3. Send this prompt:

   ```
   I want different classes of seats for Galaxium Travels.
   There is supposed to be an Economy class, a Business class, and a Galaxium class.
   Help me make a plan to implement this.
   ```

4. Bob runs the `create-plan` skill and asks clarifying questions. Choose:
   - Pricing: **multiplier on the base price** (Economy 1x, Business 1.5x, Galaxium 2.5x)
   - Availability: **separate seat count per class per flight**
   - Class selection: **in the BookingModal**
   - Booking history: **show seat class**
   - FlightCard: **show per-class availability**
5. Bob writes one or more plan `.md` files, such as an implementation plan and an architecture plan. Record their names.

### Part B — Review (2 min)

Read the plan before approving it. The plan is what Bob will build verbatim.

Check that:
- The database change splits seats into `economy_seats`, `business_seats`, and `galaxium_seats`, and adds `seat_class` to bookings.
- Cancellation restores the seat to the correct class column.
- The testing section makes sense.

If something is wrong, correct it now with a follow-up message. Fixing a plan is cheaper than fixing code.

### Part C — Agent mode in a new conversation (3 min)

1. Click **New Task (+)** to clear the context window.
2. Switch the mode to **Agent**.
3. Send (adjust the filenames to match your plan):

   ```
   Implement the backend portion of @/seat-classes-plan.md.
   Cover the data model (models.py, seed.py), schemas, and booking service.
   Do not touch the frontend yet.
   ```

4. Bob creates a Todo list. Approve each sub-task when asked, reading each diff before you approve.
5. Review the summary. Expect changes such as:
   - `models.py`: three seat columns on `Flight`, `seat_class` on `Booking`
   - `seed.py`: seeded per-class counts
   - `services/booking.py`: validates `seat_class`, decrements the right column, restores it on cancel

> The full lab continues with the frontend sub-tasks (`BookingModal`, `FlightCard`, `BookingCard`). Do those after the session if you want to see the feature end to end.

### Checkpoint

- Which skill did Bob run first?
- Why start a new conversation before implementing?
- Which auto-approval setting suits day-to-day work? (Answer: hybrid — auto-approve reads/writes, confirm command execution.)

---

## 0:15–0:19 Exercise 3 | Tailor: `/init` and Bob Rules (4 min)

**Concept:** LLMs are stateless. `/init` writes durable project knowledge. Rules encode standards that apply to every conversation.

### Part A — `/init` (1.5 min)

1. Open a **New Task (+)** in **Agent** mode.
2. Run:

   ```
   /init
   ```

3. Approve the file writes. Bob creates:

   | File | Purpose |
   |------|---------|
   | `AGENTS.md` | General project guidance |
   | `.bob/rules-agent/AGENTS.md` | Coding gotchas |
   | `.bob/rules-ask/AGENTS.md` | Context for questions |
   | `.bob/rules-plan/AGENTS.md` | Architecture constraints |

4. Open `.bob/rules-agent/AGENTS.md` and find one gotcha you did not know. For example, cancellation is a `POST`, not a `DELETE`.

### Part B — Rules (2.5 min)

1. In Explorer, right-click `.bob` → **New Folder…** → `rules`.
2. Right-click `rules` → **New File…** → `basic_rules.md`.
3. Click **New Task (+)**, stay in **Agent** mode, and send:

   ```
   Please add the following rules to @/.bob/rules/basic_rules.md and configure them so they work:
   - Always include concise JSDoc strings for every public function.
   - Be very concise in your wording.
   - Write a summary of every interaction into the folder `internal-monologue/`. Name the file starting with a timestamp, followed by a concise description of the interaction. Example: 2026-01-15_update-readme.md
   ```

4. Approve the actions. Confirm that an `internal-monologue/` folder with a timestamped file appears in the project root.
5. If the rules are ignored, send: `My project level rules in @/.bob/rules/basic_rules.md are not being listened to, please configure them properly`.

Commit `.bob/rules/` to git. Every teammate who clones the repo gets the same behavior.

### Checkpoint

- What is the name of the file that holds the three rules?
- What appeared in the project root after the first interaction?

---

## 0:19–0:20 Wrap-up

| Stage | What you did | Where control lived |
|-------|--------------|---------------------|
| Assist | Literate Coding: email validation | You accepted or rejected each diff |
| Delegate | Plan mode → Agent mode: seat classes (backend) | You approved the plan before any code was written |
| Tailor | `/init` and Bob Rules | You wrote the standards down and committed them |

**Key takeaways**
- Precise prompts give better results.
- Plan → review → new conversation → Agent is the standard feature workflow.
- Keep conversations small. Quality drops past about 100K tokens of context.
- `AGENTS.md` and rules persist knowledge and standards across conversations.

**Next:** Module 3.3 Custom Modes (for example, a `product-manager` mode), and Module 4 Context Window, Bob Shell, and MCP.

---

## Facilitator notes

- **Tokens:** Exercise 2 is the largest token consumer. Trial accounts have 40 Bobcoins.
- **Time risk:** If Agent mode runs long, stop after sub-task 1 (data model). Discuss the rest instead of waiting.
- **Rules ignored:** Workspace-level rules are sometimes overlooked. Ask Bob to configure them into the mode-specific `AGENTS.md` files.
- **Safety:** Do not set full auto-approval for command execution outside a controlled demo.
