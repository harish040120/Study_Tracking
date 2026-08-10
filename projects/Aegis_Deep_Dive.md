# Aegis — Parametric Wage Insurance for Gig Workers
## Complete Technical Deep-Dive & Interview Preparation Guide

---

## 1. Tech Stack

| Layer | Technology | Why it was chosen |
|---|---|---|
| Worker Mobile App | Flutter | Single codebase for Android/iOS; direct UPI deep-linking; works for a low-friction, phone-first gig worker persona |
| Admin Dashboard | React 19 + Tailwind CSS v4 | Real-time ops console for the Aegis team to monitor and simulate scenarios |
| Backend API | Python 3.11 + FastAPI | Async support, clean integration with ML models, native background scheduling |
| Data Hub | Node.js + Express | Aggregates external weather/AQI APIs and holds scenario-simulation state separately from the core backend |
| ML Models | scikit-learn — XGBoost, Isolation Forest | Risk scoring (structured tabular data) + anomaly-based fraud detection |
| Database | PostgreSQL | Relational integrity for workers, policies, payouts, sessions, and audit logs |
| Scheduler | APScheduler (Python) | Runs a background job every 60 seconds to auto-evaluate and trigger payouts |
| Payments | Razorpay (Test Mode / UPI Payouts) | Instant, no-claim-filing payout mechanism straight to the worker's UPI ID |
| Weather Data | OpenWeatherMap API | Rainfall, temperature, wind — objective external disruption signals |
| Air Quality | WAQI API | AQI-based disruption detection |
| Auth | JWT Bearer Tokens | Stateless, secure session handling across mobile + admin |
| Deployment | Docker Compose | Multi-service orchestration (backend, data hub, DB, admin dashboard) |

---

## 2. The Problem Statement (PS)

**Guidewire DEVTrails 2026 — Phase 3 | Persona: Food Delivery Gig Workers**

India has **5+ million active food delivery partners**. They only earn when they're actively riding — there's no base salary, no sick pay, no employer safety net. A single disruption event (heavy rain, extreme heat, a red-alert pollution day, a sudden curfew, a cyclone) can wipe out an entire evening's earnings, and the worker absorbs **100% of that loss** with zero recourse.

Real numbers from the persona (Shiva, a Zomato delivery partner in Chennai): he earns ₹700–₹1,000/day; a single flooded evening during the Northeast monsoon costs him ₹400–₹600 — and workers like him **lose 20–30% of monthly income** to disruptions they have no control over.

Traditional insurance doesn't work for this group because it's:
- **Slow** — manual claims take days/weeks.
- **Subjective** — requires proving loss with paperwork most gig workers don't keep.
- **Expensive to administer** — not viable at the scale/premium size a gig worker can afford.

**In plain words:** *"Build an insurance system that automatically pays a delivery worker within minutes of a real, verifiable disruption event — without them ever filing a claim — while staying financially sustainable and resistant to fraud."*

---

## 3. The Solution — High-Level Summary

Aegis is a **parametric insurance platform** — meaning payouts are triggered by **objective, measurable external data** (rainfall in mm, AQI level, IMD alerts, wind speed) crossing a predefined threshold, combined with **proof that the worker's income was actually impacted** — rather than requiring the worker to prove their individual loss after the fact.

Core pieces of the solution:
1. A **Flutter mobile app** where a worker registers once, picks a plan, and pays a small weekly micro-premium (₹34) via UPI.
2. A **zone-locking system** that ties the worker's coverage to their real operating area (with anti-fraud drift protection).
3. A **60-second background scheduler** that continuously checks every eligible worker against live weather/AQI/civic data and an ML-driven risk model — and **auto-approves and pays out** the moment both an external trigger *and* a business-impact trigger are confirmed.
4. **Two-gate parametric triggers** (external disruption + income drop) so payouts are fair and hard to game.
5. A **three-layer fraud detection system** (Isolation Forest ML model + rule engine + planned GNN fraud-ring detection) that holds suspicious claims for review instead of blocking or silently rejecting them.
6. **Instant UPI payout via Razorpay** — money lands in the worker's account without them ever opening a claims form.
7. An **Admin Dashboard** for the Aegis operations team to monitor live risk, simulate disruption scenarios for testing, and review flagged/held payouts.

---

## 4. Who Are the Users? (Personas)

| User | Role | What they interact with |
|---|---|---|
| **Gig Worker** (e.g., Shiva) | The insured customer — a food delivery partner | Flutter mobile app: registers, pays premium, receives payouts, views coverage/history — **completely passive after setup** |
| **Aegis Operations/Admin Team** | Internal team monitoring the system's health and fraud | React Admin Dashboard: live metrics, scenario simulation, payout/fraud review, manual Razorpay trigger |
| **The System Itself (Automated Actor)** | The 60-second scheduler + ML models | No human interface — runs continuously in the backend, silently evaluating and paying eligible workers |
| **External Data Providers** | Not "users" in the UI sense, but critical actors | OpenWeatherMap, WAQI, (conceptually) IMD/CPCB — feed the objective disruption signals that trigger the whole pipeline |
| **Payment Gateway (Razorpay)** | Executes the actual money movement | Called internally by the backend once a payout is approved; not directly seen by the worker beyond the UPI credit notification |

---

## 5. Overall Workflow — Simple Diagram (Whole System at a Glance)

```
 GIG WORKER                         AEGIS BACKEND                        ADMIN / OPS
 (Flutter App)                    (FastAPI + Scheduler)                (React Dashboard)
 ─────────────                    ────────────────────                ─────────────────
      │                                    │                                   │
 1. Register + Zone Lock  ───────────────► │                                   │
      │                                    │                                   │
 2. Pick Plan + Pay ₹34 ───────────────►   │                                   │
      │                                    │                                   │
      │                        3. Every 60s: Scheduler scans                  │
      │                           all active workers                          │
      │                                    │                                   │
      │                        4. Pulls live weather/AQI            ◄── watches/simulates
      │                           from Data Hub                        scenarios here
      │                                    │                                   │
      │                        5. Runs ML models:                             │
      │                           Risk Score + Income Drop +                  │
      │                           Fraud Score                                 │
      │                                    │                                   │
      │                        6. Gate 1 (external event) +                   │
      │                           Gate 2 (income impact) both pass?           │
      │                                    │                                   │
      │                        7. Fraud score < 0.3?                          │
      │                             │                    │                    │
      │                          YES│                  NO│                    │
      │                             ▼                    ▼                    │
      │                    8a. Auto-approve      8b. Hold for review ───────► │ reviews +
      │                        + Razorpay payout      (0.3–0.7)               │  approves/appeals
      │                             │                    │                    │
 9. Push notification: ◄────────────┘                    │                    │
    "₹480 sent to UPI"                                    │                   │
      │                                                    │                   │
      │                                        10. Worker notified:          │
      │                                            "Claim under review"      │
      ▼                                                    ▼                   ▼
 Worker checks Payouts tab                    Worker appeals within 48h    Admin monitors
 in app — sees PAID status                                                fraud dashboard
```

**Key takeaway to say in an interview:** *"From the worker's point of view, the entire experience after onboarding is passive — they just ride, and money appears when something goes wrong. All the intelligence — detecting the disruption, confirming it hurt their income, checking for fraud, and paying out — happens automatically in the backend every 60 seconds."*

---

## 6. Feature-by-Feature Deep Dive

### 6.1 Zone Locking & Drift Detection

**What it does:** During registration, the app captures the worker's GPS and locks them to a home zone (e.g., "Chennai-Central"). This zone determines which local disruption data (rain, AQI, curfew) applies to them.

**Why it's needed:** Coverage has to be tied to a real, verifiable operating area — otherwise a worker could falsely claim to be in a disrupted zone they weren't actually working in.

**How drift is handled (anti-fraud, but fair to legitimate movement):**
- The app pings GPS in the background periodically.
- If the worker's location differs from their locked zone, the system doesn't change anything immediately — it just starts a timer (`zone_change_detected_at`).
- Only if the worker stays in the new zone for **more than 30 minutes AND has actual delivery orders there** does the system promote them to the new zone.
- Otherwise, it's treated as just passing through, and nothing changes.

**Follow-up questions:**
- *"Why not just always trust the GPS in real time?"* → Because that would let bad actors "spoof" or briefly drive through a disrupted zone to falsely claim coverage there; requiring sustained presence *plus* real order activity in that zone makes the zone-change signal much harder to fake.
- *"What database fields track this state?"* → `zone` (locked), `lat_last`/`lon_last` (latest GPS), and a pending-promotion mechanism keyed on `zone_change_detected_at` and `pending_new_zone`.
- *"What if GPS itself is spoofed (fake GPS apps)?"* → This is a known limitation — GPS spoofing detection (e.g., cross-checking GPS consistency against movement patterns, or device sensor fingerprinting) is a natural extension; the current system relies on the *order-activity correlation* as one layer of defense against pure GPS spoofing, since orders require real interaction with delivery platforms.

**Topics to study:**
- Geofencing basics
- State machines (the registration_step progression: PHONE → OTP → PROFILE → INCOME → LOCATION → DONE)
- Debouncing / time-windowed confirmation patterns (used broadly in fraud-prevention systems)

---

### 6.2 Real-Time Risk Metrics & Live Monitoring

**What it does:** While a worker is online, the app polls a `/live-metrics/{worker_id}` endpoint every 30 seconds, which returns a live **risk score**, **income drop %**, and any **active alert** for their zone.

**How it's computed on the backend:**
1. Fetch the worker's locked zone + latest GPS.
2. Call the Node.js Data Hub for live weather/AQI at that location.
3. Feed that environmental data into the ML models (XGBoost risk classifier, income-drop regressor, Isolation Forest fraud detector).
4. Check the `disruption_alerts` table for any active alert in that zone.
5. Return the combined payload — and atomically mark any matched alert as claimed.

**Follow-up questions:**
- *"Why poll every 30 seconds instead of using push/websockets?"* → Simpler to implement and reliable across variable mobile network conditions typical for gig workers; 30 seconds is frequent enough for weather-driven disruptions (which build up over tens of minutes, not seconds) without excessive battery/data drain — a reasonable trade-off, though WebSockets would reduce redundant polling at scale.
- *"What's the difference between the risk score and the income drop metric?"* → Risk score (from XGBoost) is a general model of how disrupted conditions are in the zone right now (weather severity, AQI, etc.); income drop % is a separate regression estimating the actual earning impact on *this specific worker* given their historical baseline — the two are combined as the two "gates" (see Trigger System below) so a bad environment alone isn't enough; it must also correlate with real income impact.
- *"What happens to the GPS ping and metrics polling if the app is backgrounded?"* → Both timers are set up in the widget lifecycle and are explicitly cancelled in `dispose()` to avoid memory leaks and stale background calls once the screen is no longer active.

**Topics to study:**
- Polling vs WebSockets vs Server-Sent Events (trade-offs)
- Regression vs classification models (income-drop regressor vs risk classifier)
- Timer/lifecycle management in Flutter (`initState`/`dispose`)

---

### 6.3 The 60-Second Auto-Trigger Scheduler

**What it does:** A background job (via `APScheduler`) runs every 60 seconds, queries a database view of **eligible workers**, runs the full risk/fraud analysis for each, and automatically initiates a Razorpay payout for anyone who passes both gates.

**Eligibility is filtered entirely at the database level** via a view (`v_auto_trigger_candidates`) that only returns workers who:
- Have completed registration (`registration_step = 'DONE'`)
- Have an ACTIVE policy
- Haven't already been paid today
- Aren't banned

**Follow-up questions:**
- *"Why filter eligibility with a SQL view instead of doing it in application code?"* → Pushing filtering logic to the database is more efficient (avoids pulling irrelevant rows into app memory), keeps the eligibility rules centralized and auditable in one place (the view definition), and is easy to reuse across multiple endpoints/services without duplicating logic.
- *"Why 60 seconds specifically — why not real-time or every 5 minutes?"* → 60 seconds balances responsiveness (a worker isn't stuck for long after a real disruption starts) against system load — since weather-based disruptions evolve over many minutes, sub-minute granularity isn't strictly necessary, while checking every 5+ minutes could leave a worker without help during an active flood for too long.
- *"What happens if the scheduler job itself crashes mid-run for one worker?"* → Each worker's analysis is wrapped in its own try/except inside the loop, so one worker's failure (e.g., a bad API response) doesn't stop the whole batch from processing the remaining workers — the exception is logged and the loop continues.
- *"Isn't running this in the same process as the API risky (blocking the event loop)?"* → It's a fair concern — `APScheduler`'s `BackgroundScheduler` runs in a separate thread from the main async event loop, and a new asyncio event loop is explicitly created for the job's async work, which keeps it isolated from the main API request-handling loop.

**Topics to study:**
- Cron-style/interval-based job scheduling (APScheduler basics)
- Database views for encapsulating query logic
- Async/await in Python, and running async code inside a scheduled sync job (`asyncio.new_event_loop()`)
- Idempotency in scheduled jobs (avoiding duplicate payouts — see Razorpay section)

---

### 6.4 Parametric Trigger System — The Two-Gate Model

**What it does:** A payout only fires when **both gates pass** — this is the actual core insurance logic.

- **Gate 1 — External Disruption Signal:** An objective, third-party-verifiable event is happening (e.g., rainfall >65mm in 3 hours *plus* an IMD orange/red alert; AQI >300; wind >60km/h with a cyclone alert).
- **Gate 2 — Business Impact Signal:** The worker's actual income/activity shows a correlated drop (e.g., order volume drop >30%, activity drop >25%).

**Trigger examples with payout %:**

| Category | Trigger | Threshold | Payout % |
|---|---|---|---|
| Weather | Heavy Rainfall | >65mm/3hrs + IMD orange/red alert | 80% |
| Weather | Severe Flooding | >120mm/6hrs | 100% |
| Weather | Extreme Heat | >41°C for 4+ hrs + activity drop >25% | 75% |
| Weather | Cyclone/Storm | Wind >60km/h + IMD cyclone alert | 100% |
| Environment | Hazardous AQI | AQI >300 + order drop >30% | 80% |
| Civic | Curfew/Section 144 | Confirmed zone curfew | 90% |
| Platform | Zone Suspension | Platform officially halts zone | 85% |

If multiple triggers fire simultaneously, the worker receives the **single highest payout**, not a sum of all triggers — this keeps the model financially sustainable.

**Follow-up questions:**
- *"Why require both gates instead of paying out purely on weather data?"* → Paying purely on weather alone would be simpler but riskier — someone could be sitting at home during heavy rain (not actually losing delivery income) and still get paid; requiring a correlated business-impact signal (income/activity drop) ensures the payout reflects an *actual* loss, not just the presence of bad weather. This is the concept of minimizing "basis risk" — the mismatch between the trigger and the real loss.
- *"What's 'basis risk' and how does Aegis minimize it?"* → Basis risk is the gap between what the parametric trigger measures and what the insured party actually experiences — e.g., paying a flat amount for "rain in the city" when only some neighborhoods actually flooded. Aegis reduces this via hyper-local zone-based triggers (not city-wide), GPS-based worker validation, and requiring the real-time activity/income-drop signal alongside the weather trigger.
- *"Why cap it at the single highest trigger instead of stacking payouts?"* → Because stacking would let one disruption event (e.g., a cyclone that also causes flooding and AQI spikes) trigger multiple independent payouts for what is fundamentally one loss event — capping at the highest single trigger keeps payouts proportional and protects the insurance pool from being over-drawn by correlated, simultaneous triggers.

**Topics to study:**
- Parametric (index-based) insurance fundamentals — how it differs from indemnity-based insurance
- Basis risk in insurance
- Multi-condition/AND-gate trigger logic design

---

### 6.5 Financial Sustainability — Premiums & Payout Sizing

**What it does:**
- Workers pay a small **weekly micro-premium (₹34)** via UPI auto-pay.
- Premiums are **dynamically priced** based on season (e.g., monsoon vs summer), zone risk level, and worker activity.
- Payout amount is calculated from the worker's **verified hourly rate** (locked from a 12-week trailing earnings average, so one bad week doesn't reduce future payouts) × hours disrupted × the trigger's payout percentage — **capped by their plan's payout cap**.

```
Payout = (Verified Hourly Rate × Disruption Hours Lost) × Trigger Payout %
       = (₹225/hr × 2 hours) × 0.80 = ₹360 (example, capped by plan)
```

- Sustainability is modeled with a **Benefit-Cost Ratio (BCR ≈ 0.65)**, stress-tested against monsoon-scenario payout volumes.

**Follow-up questions:**
- *"What is a Benefit-Cost Ratio, and why 0.65 specifically?"* → BCR here roughly represents the ratio of expected payouts to premiums collected over time; a BCR below 1.0 (like 0.65) means the pool collects more in premiums than it pays out on average, leaving margin for operational costs, reserves, and profitability — critical for the system to remain solvent even in a heavy monsoon season with above-average claims.
- *"Why lock the hourly rate from a 12-week average instead of using real-time earnings?"* → It prevents gaming the system — if payouts were based on very recent earnings, a worker could work less right before an anticipated disruption to inflate their "loss," or a naturally slow week could unfairly lower their payout; a trailing 12-week average smooths out both normal variance and manipulation attempts.
- *"How does dynamic pricing avoid unfairly penalizing risk-prone zones/seasons?"* → It's a genuine trade-off — pricing higher in monsoon season or high-risk zones reflects real actuarial risk (similar to how car insurance varies by area), but the team also uses enrollment-lock periods (see Adverse Selection below) rather than pure price hikes to manage risk, keeping premiums broadly affordable.

**Topics to study:**
- Basic insurance/actuarial concepts: premium, payout cap, loss ratio, Benefit-Cost Ratio
- Trailing averages for smoothing volatile input data
- Adverse selection (see next section)

---

### 6.6 Adverse Selection Prevention

**What it does:** Prevents workers from enrolling *only* right before a known disruption (e.g., signing up the moment a cyclone warning is issued) to game the system.

**How:** Enrollment lock periods before major forecasted alerts, and strict policy-activation rules (a policy likely needs to be active for some minimum period before it can be used to claim against a newly-forecast event).

**Follow-up questions:**
- *"What is adverse selection, in general insurance terms?"* → It's the tendency for people who know they're higher-risk (or who anticipate an imminent loss event) to disproportionately seek coverage right before that risk materializes, skewing the risk pool unfavorably for the insurer — a classic insurance problem, and Aegis's enrollment-lock approach is a standard mitigation technique (similar to waiting periods in real-world health/travel insurance).
- *"How would you detect someone trying to game this — e.g., signing up right as a forecast drops?"* → Timestamp comparison between policy `coverage_start` and the alert's forecast/issued timestamp — if enrollment happens after a relevant alert is already public/forecast, that policy should not be eligible to claim against that specific event, which is effectively what "strict policy activation rules" enforces.

**Topics to study:**
- Adverse selection (classic insurance/economics concept)
- Waiting periods / cooling-off periods in insurance products

---

### 6.7 Fraud Detection — Three-Layer System

**What it does:** Every payout candidate is scored for fraud risk before being auto-approved, using three layers:

**Layer 1 — Isolation Forest (behavioral, unsupervised ML):**
Scores each claim across GPS consistency, movement pattern, order activity, device fingerprint, and behavioral anomalies (e.g., logging in only during disruptions).

**Layer 2 — Post-model rule engine (explicit business rules):**
```python
# Example rules
if rain_1h > 20 and movement_km < 0.5 and hours_worked_today > 3:
    adjusted_score = max(adjusted_score, 0.45)   # GPS_STATIC_DURING_DISRUPTION

if not order_history_presence and rain_1h > 15:
    adjusted_score = max(adjusted_score, 0.40)   # NO_ORDERS_BEFORE_DISRUPTION

if claim_velocity > 20:  # >20 claims from same zone in 15 min
    adjusted_score = max(adjusted_score, 0.65)   # HIGH_CLAIM_VELOCITY_RING
```

**Layer 3 — GNN (Graph Neural Network) fraud-ring detection (Phase 3, forward-looking):**
Models workers as graph nodes; shared signals (same device, same GPS cluster, coordinated claim timing) become edges — a coordinated fraud ring shows up as an unusually dense cluster in the graph.

**Fraud score thresholds → action:**

| Score | Action | Outcome |
|---|---|---|
| < 0.3 | Auto-approved | Instant UPI payout (e.g., under 10 seconds) |
| 0.3–0.7 | Held for review | Resolved within 4 hours; worker notified; can appeal |
| > 0.7 | Blocked | Admin alert + manual review; worker notified with reason |

**Critically: a flagged claim is always held, never silently rejected** — worker can appeal within 48 hours.

**Follow-up questions:**
- *"Why combine an unsupervised ML model (Isolation Forest) with hand-written rules instead of relying on ML alone?"* → Isolation Forest is good at catching *statistically unusual* patterns broadly, but explicit rules encode domain knowledge that's hard for an unsupervised model to learn on its own — like "a real delivery worker wouldn't be perfectly stationary for hours during heavy rain while claiming they were working." Combining both gives interpretability (you can literally name the rule that fired) alongside general anomaly detection.
- *"Why Isolation Forest specifically, and not something like a supervised classifier?"* → Fraud is rare and constantly evolving, so labeled fraud examples are scarce — Isolation Forest is unsupervised (works by isolating anomalous points that require fewer random splits to separate from the rest of the data) and doesn't require a labeled fraud dataset to start working, which fits a cold-start fraud detection problem well.
- *"What's the intuition behind how Isolation Forest actually isolates anomalies?"* → It builds random decision trees that repeatedly split the data on random features/thresholds; anomalous points (which are "few and different") tend to get isolated into their own leaf node in far fewer splits than normal points, so a shorter average path length across many trees indicates a higher anomaly score.
- *"Why never silently reject, only 'hold'?"* → Because false positives are a real risk with automated fraud detection — silently rejecting a legitimate worker's claim (like Shiva's, if it had been mis-scored) would be a serious trust and fairness failure; holding for review with a transparent appeal path protects genuine workers from algorithmic mistakes while still stopping fraud from being auto-paid.
- *"What is claim velocity, and why is it a strong fraud ring signal?"* → It's the rate of claims coming from the same zone within a short time window (e.g., >20 claims in 15 minutes); a real, organic disruption typically produces a more gradual, spread-out pattern of claims as different workers experience impact at slightly different times, whereas a coordinated fraud ring often submits near-simultaneously because they're triggered together rather than by genuinely independent real-world experiences.

**Topics to study:**
- Isolation Forest algorithm (how anomaly scoring via path length works)
- Unsupervised vs supervised anomaly detection
- Graph Neural Networks — basic intuition (nodes/edges, message passing) for fraud-ring detection
- Precision/recall trade-offs and why "hold, don't reject" matters for fairness

---

### 6.8 Instant Payout — Razorpay Integration

**What it does:** Once a payout is approved (both gates pass + fraud score < 0.3), the backend calls Razorpay's Payout API to send money directly to the worker's UPI ID — no manual claim form, no bank details re-entry.

**Key engineering details:**
- **Idempotency check** — before firing a payout, the system checks the `payments` table for an existing SUCCESS record tied to that `payout_id`, so the same payout is never triggered twice (critical given the 60-second scheduler could, in theory, process the same worker again if something went wrong).
- **Simulation mode** — if no real Razorpay API key is configured (`RAZORPAY_KEY_ID` unset), the system generates a simulated payment reference (`rzp_sim_...`) instead, so the entire pipeline is testable end-to-end without real money movement — important for hackathon/demo environments.
- Amount is converted to **paise** (`amount * 100`) since Razorpay's API expects the smallest currency unit, consistent with how most payment gateways handle currency to avoid floating-point rounding issues.

**Follow-up questions:**
- *"Why is idempotency so important here specifically?"* → Because the trigger is a *scheduled, repeating* job (every 60 seconds) rather than a one-time user action — without an idempotency check, a transient error after a successful payout call (e.g., the DB update failing right after Razorpay confirms) could cause the same worker to be paid twice on the next scheduler cycle; checking for an existing SUCCESS payment record before firing prevents this.
- *"Why store money as paise/integers instead of rupees as floats?"* → Floating-point numbers can introduce small rounding errors when doing repeated arithmetic on decimal currency values; working in the smallest indivisible unit (paise, as an integer) avoids that entire class of bugs, which is standard practice across most payment systems (Stripe, Razorpay, etc. all follow this convention).
- *"What would happen if the Razorpay call succeeds but your database update fails right after?"* → This is a classic distributed-systems consistency risk — a fair, honest answer is that this scenario could theoretically create a state where money was sent but not recorded as PAID; a production-grade fix would involve a reconciliation job that periodically cross-checks Razorpay's own transaction records against the local `payments` table to catch and correct any such mismatches — a good "what I'd improve" answer.

**Topics to study:**
- Idempotency in payment systems (a very common interview topic for anything touching money)
- Integer vs floating-point representation for currency
- UPI payments basics (VPA / virtual payment address concept)
- Distributed system consistency issues around "at-least-once" vs "exactly-once" operations

---

### 6.9 Admin Scenario Simulation

**What it does:** Lets the Aegis operations team simulate real-world disruption scenarios (heavy rain, severe flood, hazardous AQI, GPS fraud, etc.) for a specific worker through the Admin Dashboard — without needing an actual weather event to occur — to test the entire pipeline end-to-end.

**How it works:** The Node.js Data Hub keeps an **in-memory override state** per active worker/scenario. When the admin selects a preset (or manually adjusts sliders for rain, AQI, earnings, etc.) and hits Apply, subsequent calls to `/api/risk-data` for that worker return the simulated values instead of real API data — and the 60-second scheduler picks this up naturally on its next cycle, running the *real* trigger/fraud/payout logic against *simulated* inputs.

**Follow-up questions:**
- *"Why is this valuable beyond just manual QA testing?"* → It lets the team validate the full pipeline — weather signal → ML scoring → gate logic → fraud check → Razorpay payout — under controlled, repeatable conditions (e.g., a "severe flood" preset every time produces the same inputs), which is essential for demoing the system's behavior reliably and for regression testing after code changes, without waiting for real disruptions.
- *"Why keep the override state in the Node.js hub rather than directly in the FastAPI backend?"* → The Data Hub is already the layer responsible for aggregating "environmental" data (real or simulated) before handing it to the backend — keeping the override logic there means the FastAPI backend's core trigger/fraud/payout logic doesn't need to know or care whether the data it receives is real or simulated, which is a clean separation of concerns.

**Topics to study:**
- Separation of concerns in system design (data-source abstraction)
- Feature flagging / scenario-injection patterns for testing

---

## 7. Full System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                     WORKER'S FLUTTER APP                             │
│  Registration → Zone Lock → Plan → Home → Alerts → Coverage → Payouts│
└──────────────────────┬──────────────────────────────────────────────┘
                       │ REST API (JWT Auth)
                       ▼
┌─────────────────────────────────────────────────────────────────────┐
│               FASTAPI MODEL BACKEND (Port 8010)                     │
│  /register  /analyze  /live-metrics  /session-ping  /razorpay/payout│
│  60-second APScheduler loop → run_analysis_internal()               │
│  XGBoost Risk | Income Regressor | Isolation Forest Fraud           │
└──────────────────────┬──────────────────────────────────────────────┘
                       │ Internal HTTP
          ┌────────────┴────────────┐
          ▼                         ▼
┌──────────────────┐     ┌───────────────────────────────────────────┐
│   POSTGRESQL     │     │    NODE.JS DATA HUB (Port 3015)           │
│   workers        │     │  /api/risk-data  /api/scenario            │
│   policies       │     │  OpenWeatherMap + WAQI + Scenario State   │
│   payouts        │     └───────────────────────────────────────────┘
│   payments       │
│   disruption_    │     ┌───────────────────────────────────────────┐
│   alerts         │     │    REACT ADMIN DASHBOARD (Port 2000)      │
└──────────────────┘     │  Dashboard | Scenario | Payouts | Fraud   │
                          └───────────────────────────────────────────┘
                                          ▲
                                          │ (external, not truly a "user")
                                    Razorpay Payout API
                                    (UPI credit to worker)
```

---

## 8. Edge Cases to Know

| # | Case | Handled By |
|---|---|---|
| 1 | Worker briefly drives through another zone | Zone drift requires >30 min presence + real orders before promotion |
| 2 | Worker is stationary during a claimed disruption | Isolation Forest + `GPS_STATIC_DURING_DISRUPTION` rule → held for review |
| 3 | Worker opens the app only when disruptions are declared, no order history | `NO_ORDERS_BEFORE_DISRUPTION` rule → held for review |
| 4 | Coordinated fraud ring submits many claims in the same zone within minutes | `HIGH_CLAIM_VELOCITY_RING` rule + planned GNN clustering → held for review |
| 5 | Multiple simultaneous triggers (e.g., cyclone causes flood + AQI spike) | Highest single payout only, not stacked — protects pool sustainability |
| 6 | Worker signs up right after a cyclone warning is already public | Enrollment lock / policy activation rules should prevent claiming against pre-existing forecasts |
| 7 | Scheduler job fails for one worker mid-batch | Try/except per worker — rest of the batch still processes |
| 8 | Same payout accidentally triggered twice (e.g., scheduler re-run) | Idempotency check against `payments` table before calling Razorpay |
| 9 | No Razorpay API key configured (dev/demo environment) | Falls back to simulation mode with a fake payment reference |
| 10 | Worker disputes a held/blocked claim | 48-hour appeal window; never silently rejected |
| 11 | A bad recent week (low earnings) right before a disruption | Payout uses a locked 12-week trailing average, not the most recent week, to avoid unfair reduction |
| 12 | GPS spoofing (fake location apps) | Partially mitigated via order-activity correlation; acknowledged as an area for further hardening (device fingerprinting, sensor consistency checks) |

---

## 9. Master List — Concepts to Study Before the Interview

**Insurance / Domain Concepts:**
- Parametric (index-based) vs indemnity-based insurance
- Basis risk
- Adverse selection & waiting periods
- Benefit-Cost Ratio / loss ratio basics
- Premium pricing fundamentals (risk-based/dynamic pricing)

**Machine Learning:**
- XGBoost (gradient boosting on tabular data) — why it's a strong fit for structured risk scoring
- Regression vs classification (income-drop regressor vs risk/fraud classifiers)
- Isolation Forest — anomaly detection via random partitioning and path length
- Graph Neural Networks — basic intuition for fraud-ring/relationship detection

**Backend / Systems Engineering:**
- Scheduled/background jobs (APScheduler, cron concepts)
- Idempotency in payment systems
- Database views for encapsulating business logic
- Async/await and event loops in Python
- Separation of concerns (Data Hub vs core backend vs admin dashboard)
- Currency handling (integer/paise vs float) and why it matters

**Mobile/Frontend:**
- Polling vs push (WebSockets) trade-offs
- Timer lifecycle management (avoiding leaks in Flutter)
- Geofencing / GPS-based state management

**Payments:**
- UPI basics (VPA)
- Reconciliation between a payment gateway and an internal ledger

---

## 10. One-Line Elevator Pitch (memorize this)

> "Aegis is a parametric wage-insurance platform I built for gig delivery workers — instead of making someone file a claim after a disruption, a 60-second background scheduler continuously checks live weather and AQI data against each worker's real income impact using an XGBoost risk model, and the moment both an objective external trigger and a genuine income-drop are confirmed — and an Isolation Forest fraud check passes — the payout is sent instantly to their UPI account via Razorpay, with zero manual claims involved."*
