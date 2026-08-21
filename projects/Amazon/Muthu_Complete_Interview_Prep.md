# Muthu Harish T — Complete Interview Preparation Handbook

**Sections:**
1. Self Introduction
2. General HR / Behavioral Questions
3. Managerial / Situational Questions
4. Technical Projects — DocuTalk, SEAM, Aegis
5. Patent Deep-Dive — REALG
6. Quick-Reference Cheat Sheet

---

# 1. Self Introduction

> Good morning! I'm Muthu Harish T, currently pursuing my bachelor's in Computer Science and Business Systems at PSG Institute of Technology and Applied Research.
>
> I've always been curious about new technologies and enjoy building practical solutions rather than just learning them in theory. Recently, I completed a Software Engineering Internship at Stepping Edge Technologies, where I developed a GPS geofencing-based automated attendance system using n8n workflows. Before that, I worked at IBM SkillBuild and the Edunet Foundation, where I built an AI agent on IBM Cloud.
>
> Some of my key projects include DocuTalk, a RAG-based chatbot for interacting with PDFs using LangChain and local LLMs; Aegis, a FastAPI-based wage insurance platform for gig workers; and SEAM, a biometric authentication system using Face-API with face matching and liveness detection. Through these projects, I've gained hands-on experience with AI, backend development, cloud platforms, and building end-to-end solutions that address real-world problems.
>
> I'm also a Smart India Hackathon 2024 finalist, a winner at multiple hackathons and an Industry-Academia Conclave, and I hold two published patents — REALG and SARS — which have strengthened my problem-solving skills and ability to work effectively under tight deadlines.
>
> Beyond academics, I serve as the CSI Secretary and my class's Placement Representative, where I organize technical events, coordinate with students and faculty, and help drive placement-related activities. I'm also an NSS volunteer, which has taught me the importance of teamwork, leadership, and contributing to the community.
>
> Overall, what drives me is the opportunity to keep learning, solve real-world problems through technology, and take ownership of meaningful work. I'm excited to bring that mindset and contribute to this role.

**Structure to remember when adapting this on the fly:**
- Name
- College & degree
- Area of interest
- Internship
- 1–2 projects
- Leadership
- Career goal

---

# 2. General HR / Behavioral Questions

### 🔹 Tell me about your strengths
Choose 3 strengths with examples. Possible strengths: quick learner, problem-solving, adaptability, team player, leadership, calm under pressure, curious about new technologies.
Always support with an example — e.g., *Leadership → Secretary + Basketball Captain.*

### 🔹 What are your weaknesses?
**Don't say:** "I'm a perfectionist" / "I work too hard."
**Genuine, improvable weaknesses to use:**
- Sometimes I spend too much time exploring different solutions before deciding.
- I'm confident in everything I do, so I try everything, and it backfires a few times — I'm working on analyzing what I can and can't do realistically before diving in. On the flip side, it means I never hesitate to put myself forward or try new things.
- Earlier I hesitated to ask for help, but now I communicate sooner.
- Public speaking used to make me nervous, so I started taking on leadership roles to push through it.

**Always end with:** "I've been actively working on improving it."

### 🔹 Why should we hire you?
Mention: strong technical foundation, fast learner, internship experience, leadership, willingness to learn.
**Don't say:** "Because I'm hardworking."

### 🔹 Why do you want to join our company?
**Structure:** company's work/products → learning opportunities → your skills match → long-term growth.
**Never say:** salary, or brand name alone.

### 🔹 Where do you see yourself in 5 years?
Mention: growing technically, taking ownership, leading projects, continuous learning.
**Don't say:** CEO, or manager immediately.

### 🔹 Tell me about a challenge you faced
Use **STAR** (Situation, Task, Action, Result).
Good examples: difficult project, tight deadline, team disagreement, learning a new technology.

### 🔹 Tell me about a failure
Choose: a competition, a project bug, a missed deadline.
Focus more on **what you learned**.

### 🔹 Tell me about teamwork
Mention: internship experience, or CSI/class association work.
Talk about: communication, listening, helping others.

### 🔹 Leadership experience
Talk about: CSI Secretary, Placement Representative, NSS volunteer.
Mention: coordinating people, conflict resolution, planning events.

### 🔹 Conflict in a team
Remember: listen first, understand everyone's viewpoint, focus on the solution, communicate respectfully. Never blame people.

### 🔹 Tight deadlines
Say: prioritize work, break into tasks, communicate early, deliver important work first.

### 🔹 How do you learn new technology?
Documentation → official docs → YouTube → build a small project to solidify it.

### 🔹 Biggest achievement
Options: hackathon win/finalist placement, patent publication, leadership role, project completion.
Talk about: impact and learning.

### 🔹 Why Computer Science?
Talk about: problem solving, technology, building products, curiosity.
**Not:** "Good salary."

### 🔹 What motivates you?
Solving problems, learning, building useful software, seeing users benefit.

### 🔹 What do you do outside academics?
Basketball, chess, table tennis, NSS, coding contests — shows you're well-rounded.

### 🔹 Why should we not reject you?
Positive attitude, fast learner, adaptable, team player, good technical base.

### 🔹 Are you comfortable relocating?
Keep it simple: Yes, flexible, open to opportunities.

### 🔹 Any questions for us?
Always ask something. Examples:
- What does success look like for this role?
- What technologies does the team work with?
- What learning opportunities are available?
- How is feedback given to interns/new hires?

**Never** ask about salary in the first HR round unless they bring it up.

---

# 3. Managerial / Situational Questions

**If your teammate isn't working?**
Talk privately → understand the reason → help if needed → escalate only if necessary.

**Team disagreement?**
Listen → compare ideas → use facts → choose the best solution.

**Project fails?**
Identify root cause → inform stakeholders → fix quickly → learn for future.

**Multiple deadlines?**
Prioritize → estimate effort → communicate → deliver critical work first.

**Manager criticizes your work?**
Accept feedback → ask clarifying questions → improve → don't argue.

**You don't know something?**
Admit honestly → learn quickly → ask for guidance if needed. Never pretend to know.

**Someone else gets credit?**
Team success matters more → continue contributing → discuss professionally if necessary.

---

# 4. Technical Projects

## 4.1 DocuTalk — AI PDF Chatbot (Local RAG)

**Tech Stack:** Chainlit (UI) · LangChain (orchestration) · Ollama running Qwen (local LLM) · Nomic Embed (local embeddings) · ChromaDB (vector store) · Python
> Currently runs **entirely locally** — no cloud/HF deployment yet; be upfront about this if asked.

**The Problem:** People have long PDFs and want quick, trustworthy answers without reading the whole document — and ideally without paying for an API key.

**The Solution (structure, briefly):**
1. User uploads PDF(s) → Chainlit UI.
2. LangChain splits text into chunks.
3. Each chunk embedded via Nomic Embed (local, CPU).
4. Vectors stored in ChromaDB.
5. At query time, question is embedded → similarity search retrieves top-k relevant chunks.
6. Retrieved chunks + question sent to Qwen (via Ollama) → grounded answer generated.
7. Answer displayed with source citations.

**Top Interview Q&A:**
1. **What is RAG and why use it?** — Retrieves real document content first, then generates an answer grounded in it, avoiding hallucination and needing no fine-tuning per document.
2. **Why chunk instead of embedding the whole PDF?** — Finer-grained chunks give more precise retrieval; a whole-PDF vector blurs unrelated topics together.
3. **Why Nomic Embed + Ollama/Qwen specifically?** — Both run locally and free, keeping the entire pipeline offline, private, and cost-free — a deliberate trade-off of raw power for zero cost/latency-to-cloud.
4. **Why ChromaDB?** — Lightweight, easy local setup, no external infra needed — fits a local-first project.
5. **How does multi-PDF support work?** — All chunks from every uploaded file are embedded into the same collection with source metadata, so retrieval spans all documents and citations trace back to the right file.
6. **What's a known limitation?** — If no relevant chunk exists for a question, the LLM might hallucinate instead of saying "I don't know" — mitigated by explicit prompt instructions to only answer from context.
7. **What's Advanced RAG, and what would you add next?** — Hybrid search (vector + keyword/BM25), re-ranking with a cross-encoder, query rewriting/HyDE, and MMR for diverse retrieval — solid next steps beyond this baseline implementation.
8. **What is LangGraph and how does it differ from a normal LangChain chain?** — A chain is a linear sequence of steps; LangGraph models cyclical, stateful workflows (e.g., retrieve → self-check sufficiency → retry if needed) as a graph — useful for building a more "agentic" version of DocuTalk.
9. **How would evaluation work for this pipeline?** — Separately measure retrieval quality (precision/recall on labeled question-chunk pairs) and generation quality (RAGAS or LLM-as-judge scoring) — not yet built into this project, but the natural next step.
10. **One-line pitch:** *"DocuTalk is a fully local RAG-based PDF chatbot — LangChain orchestrates chunking and retrieval, Nomic embeddings vectorize content, ChromaDB stores it, and Ollama's Qwen model generates grounded, source-cited answers, entirely offline."*

---

## 4.2 SEAM — Secure Encryption and Authentication Model (Aadhaar Face Auth)

**Context:** Smart India Hackathon 2024, PS ID SIH1670 — "Develop a functional solution that incorporates the security of the ML model."

**Tech Stack:** React + Vite + Material UI · Face-API.js (TensorFlow.js) · **SSD MobileNet V1** (main face-detection model) + 68-point landmark model + face recognition model · Node.js on Azure · Azure SQL DB · AES-256-CBC encryption, JS obfuscation, SRI, hash verification · Local Binary Patterns (LBP) for anti-spoofing · OWASP ZAP + Lighthouse for testing.

**The Problem (two halves):**
1. Build a real-time, browser-based Aadhaar face authentication system that tells a live person apart from someone spoofing with a photo/video.
2. **The real crux:** secure the ML model itself against theft, tampering, and reverse engineering — not just "add HTTPS."

**The Solution (structure, briefly):**
1. User enters Aadhaar number → validated against DB.
2. Webcam captures live frame → SSD MobileNet V1 detects the face → 68-point landmarks extracted.
3. Liveness detection (blink/motion) + LBP texture analysis run in parallel to catch spoofing.
4. Face descriptor (128-D vector) generated and compared to the registered descriptor via Euclidean distance (threshold 0.45).
5. Model files are AES-256-CBC encrypted at rest; hash verification checks integrity before the browser trusts a downloaded model; code is obfuscated.
6. Sensitive data (Aadhaar, descriptors) is never cached — loaded into backend RAM only per-request, then wiped ("Just-in-Time" handling); only model files are cached.
7. Every attempt is logged (name, Aadhaar, IP, timestamp) to an Admin Dashboard.

**Top Interview Q&A:**
1. **What's the actual core of the PS you focused on?** — Model security: AES-256 encryption, obfuscation, and hash-based integrity checks — not just the face-matching itself.
2. **Why SSD MobileNet V1?** — Single-shot detector (fast, one-pass) on a MobileNet backbone using depthwise separable convolutions — keeps the model in the 5–7MB budget for slow 3G/4G downloads.
3. **How is spoofing detected?** — Two layers: liveness (blink/motion over frames) + LBP (texture histograms distinguish real skin from flat printed/screen textures).
4. **Why AES-256-CBC specifically, and is it the best choice?** — CBC prevents identical-plaintext-block leakage (unlike ECB); an honest critique is that GCM would be stronger since it bundles authentication with encryption — we added integrity separately via hashing instead.
5. **What's the difference between encryption and obfuscation?** — Encryption makes data unreadable without a key (reversible); obfuscation makes code hard to reverse-engineer but is still technically executable/readable with effort — used together for data-at-rest vs. runnable logic.
6. **Why the caching redesign?** — Originally cached user data too, causing memory bloat and privacy risk; changed to cache only (non-sensitive) model files, cutting storage from ~63.6MB to ~24.3MB in testing.
7. **How was security validated?** — OWASP ZAP attack simulations; early scans flagged missing security headers, later scans post-fix showed injection attempts marked "out of scope."
8. **Known edge cases?** — Mask → fails (landmarks obscured); multiple faces → rejected; twins → falls back to Aadhaar number since face descriptors can be near-identical; extreme angles → fails.
9. **What's a real limitation of the anti-spoofing approach?** — LBP + liveness catches 2D photo/screen attacks well, but wouldn't reliably catch deepfakes or 3D masks — would need depth-sensing cameras or deepfake-trained classifiers as a next step.
10. **One-line pitch:** *"SEAM is a browser-based Aadhaar face authentication system where the real challenge was securing the ML model itself — SSD MobileNetV1 for detection, LBP + liveness for anti-spoofing, and AES-256 encryption + hash verification + obfuscation to protect the model from theft or tampering."*

---

## 4.3 Aegis — Parametric Wage Insurance for Gig Workers

**Context:** Guidewire DEVTrails 2026, Persona: Food Delivery Gig Workers.

**Tech Stack:** Flutter (worker app) · React 19 + Tailwind (admin dashboard) · FastAPI + Python 3.11 (backend) · Node.js Data Hub (external API aggregation) · XGBoost + Isolation Forest (ML) · PostgreSQL · APScheduler (60-second auto-trigger loop) · Razorpay (UPI payouts) · OpenWeatherMap + WAQI (weather/AQI data) · JWT auth.

**The Problem:** 5M+ Indian gig delivery workers earn only when riding; a single disruption (rain, heat, AQI, curfew) wipes out income with zero recourse — and traditional insurance is too slow, subjective, and expensive for this group.

**The Solution (structure, briefly):**
1. Worker registers once via Flutter app, locks a home zone via GPS, pays a ₹34/week micro-premium via UPI.
2. A 60-second background scheduler continuously scans eligible workers, pulling live weather/AQI data.
3. **Two-gate trigger:** Gate 1 = objective external disruption (rain/AQI/wind threshold + official alert); Gate 2 = actual income/activity drop for that worker.
4. Both gates pass → fraud score checked (Isolation Forest + rule engine); if <0.3 → auto-approved.
5. Razorpay sends an instant UPI payout — no claim filed, no form, no adjuster.
6. Held claims (fraud score 0.3–0.7) get a 4-hour review window and can be appealed within 48h — never silently rejected.

**Users:** the Gig Worker (passive, just rides), the Aegis Ops/Admin team (monitors + runs scenario simulations), the automated scheduler itself (no UI), external data providers (weather/AQI APIs), and Razorpay (executes payment).

**Top Interview Q&A:**
1. **What is parametric insurance, and why fit gig workers better than traditional insurance?** — Payout triggers on an objective, measurable condition instead of requiring a manual claim — much faster and doesn't need paperwork trails gig workers don't have.
2. **Why two gates (external + income-drop) instead of paying on weather alone?** — Avoids "basis risk" — someone sitting at home during rain shouldn't get paid just because it rained; requiring a correlated income-drop signal ensures the payout reflects a genuine loss.
3. **Why XGBoost for risk scoring?** — Strong on structured/tabular data (work history, activity, weather features), fast, and reasonably interpretable versus deep learning.
4. **Why Isolation Forest for fraud, not a supervised classifier?** — Fraud is rare and constantly evolving — labeled fraud data is scarce, so an unsupervised anomaly detector (isolating statistically unusual points) fits a cold-start fraud problem better.
5. **Why "hold for review" instead of auto-rejecting flagged claims?** — False positives are a real risk; silently rejecting a genuine worker's claim is a serious fairness failure — holding with a transparent 48-hour appeal path protects real workers while still stopping fraud from being auto-paid.
6. **Why lock the payout hourly rate to a 12-week trailing average?** — Prevents gaming (working less right before an anticipated disruption to inflate "loss") and smooths out a naturally slow week.
7. **How is double-payment prevented given the 60-second recurring scheduler?** — An idempotency check against the `payments` table (existing SUCCESS record for that payout_id) before ever calling Razorpay.
8. **Why zone-locking with a 30-minute + real-orders drift rule?** — Prevents a worker from briefly driving through a disrupted zone to falsely claim coverage there — requires sustained presence AND real order activity before promoting the zone.
9. **What's "adverse selection" and how does Aegis prevent it?** — The risk of people enrolling only right before a known disruption; mitigated via enrollment-lock periods and strict policy-activation timing rules.
10. **One-line pitch:** *"Aegis is a parametric wage-insurance platform for gig workers — a 60-second scheduler checks live weather/AQI against each worker's real income impact using XGBoost, and the moment both an objective trigger and a genuine income-drop are confirmed — and a fraud check passes — the payout lands in their UPI account via Razorpay, with zero manual claims."*

---

# 5. Patent Deep-Dive — REALG (Real-time Emergency Alert and Location Geofencing)

**Filed:** 14 May 2025 | **Published:** 13/06/2025, Publication No. 24/2025 | **Application No.:** 202541046344
**Applicant:** PSG Institute of Technology and Applied Research
**Inventors:** Dr. R Manimegalai, Dr. J Nagarjun, Kovarthan Manikandan, Deepak Chandrasekar, **Muthu Harish T**, Abhimanya S
**IPC Classification:** G08B0007060000 (alarms), H04W0004021000 (location-based wireless services), A42B0003040000 & A42B0003300000 (protective wearable-related), G06Q0050080000 (construction-industry business methods)

## STAR Explanation

### Situation
Construction sites are high-risk environments — workers operate near heavy machinery, cranes, restricted zones, and unpredictable weather. Existing safety systems are largely **reactive**: no real-time compliance monitoring, unauthorized zone entry often goes undetected until an incident, workers have limited situational awareness of nearby hazards, and when an emergency happens there's no fast, accurate way to locate the affected worker or notify supervisors instantly. Traditional protocols respond *after* accidents rather than predicting and preventing them.

### Task
Design and build an end-to-end IoT safety system that could:
1. Let a worker instantly raise an emergency alert from anywhere on-site.
2. Pinpoint their exact real-time location during an emergency.
3. Detect and warn workers before they enter high-risk/restricted zones.
4. Give supervisors a centralized, real-time monitoring dashboard.
5. Use predictive/ML models to anticipate risk (zone entry, unsafe crane conditions, adverse weather) rather than just react.
6. Do all this with low-cost, low-power, long-range hardware suited to large sites with unreliable Wi-Fi/cellular coverage.

### Action — The Four-Part System
1. **Wearable Worker Device** — Arduino Pro Mini + LoRa module (long-range, low-power, works without Wi-Fi/cellular) + GPS module + two push-buttons (pressed *simultaneously* to prevent accidental triggers, escalating an SOS) + LED/buzzer for local confirmation feedback.
2. **Supervisor Device** — Receives real-time SOS alerts + GPS coordinates via LoRa P2P from any worker device; instantly shows who raised the alert and where, cutting response time.
3. **LoRa-Enabled Geofencing Cones** — Deployed around high-risk zones (crane operation areas, excavation pits, restricted zones); use **RSSI (signal-strength) proximity detection** — direction-independent, works without line-of-sight, unlike ultrasonic/IR sensors.
4. **REALG Website (Central Dashboard)** — Predictive movement modeling (ML flags workers heading into high-risk zones), crane-operation safety module (live wind speed/direction via OpenWeather API), satellite aerial site-planning view, and AI-driven rainfall/weather forecasting.

Together: **detect → alert → locate → respond → predict → prevent** — a closed-loop safety system.

### Result
- Faster emergency response (instant location + alert instead of manual search/report).
- Improved situational awareness for both workers and supervisors.
- Proactive risk prevention instead of pure accident-response.
- Centralized site planning and decision-making via the dashboard.
- A safer, more compliant, low-cost, long-range solution built for connectivity-poor construction environments.
- **Filed as a patent** (14 May 2025) — recognizing the novelty of the integrated LoRa-based, infrastructure-free approach.

## Why This Was Patentable (the key point to emphasize)
Not any single component (Arduino, LoRa, GPS, NFC are all off-the-shelf) — the patentable novelty is the **specific combination and method**: LoRa RSSI used specifically for direction-independent, infrastructure-free geofencing on a construction site, combined with the dual-button anti-false-trigger SOS mechanism and gateway-free peer-to-peer emergency communication. Alternatives (Infrared, LiDAR, Ultrasonic, Ultra-Wideband) were explicitly evaluated and ruled out in the documentation — demonstrating a deliberate, justified engineering/inventive step, not just an obvious combination of existing parts.

## Follow-Up Questions You May Be Asked

**Hardware / Wearable Device**
1. Why was LoRa chosen over Wi-Fi, Bluetooth, or cellular (GSM/4G)? — Long range (1–5km), very low power, no cellular subscription or site-wide Wi-Fi infrastructure needed; critical for large/remote sites with unreliable coverage.
2. What's the range/data rate of the LoRa module, and how does it hold up in an obstructed environment? — LoRa trades data rate for range and penetration; signal range issues in confined spaces were addressed by integrating larger antennas.
3. How long does the wearable's battery last? — ~32 days on a 2000mAh LFP battery (~62.44 mAh/day), thanks to sleep-mode power optimization on GPS/LoRa when idle.
4. What's the GPS accuracy, and how does it perform with poor satellite visibility? — If GPS is unavailable, the SOS is still sent immediately without coordinates rather than delaying the alert — prioritizing that *something* reaches the supervisor fast.
5. What do the two push buttons correspond to, and how are false alarms prevented? — Simultaneous press of both buttons is required — a deliberate, deliberate-effort action that filters out nearly all accidental triggers common with single-button designs.
6. Is the wearable device rated for dust/water? — Not explicitly documented in current prototype materials; a fair answer is that IP-rating hardening would be part of moving from prototype to field-ready hardware.

**Network / Geofencing Cones**
7. Is the cone network a mesh, star, or point-to-point topology? — Primarily direct LoRa P2P between worker device ↔ cone ↔ supervisor device rather than a full mesh; the cone acts as a local relay/geofence marker rather than routing across multiple hops.
8. How many cones are needed to cover a typical site, and how is precision achieved? — Coverage depends on cone RSSI-threshold range, which is dynamically adjustable per site-specific safety requirements — more cones for denser hazard-zone coverage.
9. What happens if a cone loses power? — Not explicitly documented; a reasonable answer is that a supervisor would notice the absence of expected periodic signals/battery-status pings as a failover indicator — an area for future hardening.

**Software / ML**
10. What ML approach predicts worker movement into high-risk zones, and what data trains it? — Positional/trend data from worker device pings correlated against marked zone boundaries; exact model architecture would need documentation reference, but conceptually it's a movement-trend classifier flagging likely zone entry before it happens.
11. How accurate are the predictive models, and how were they validated? — Documented primarily through demo/prototype testing rather than a large-scale statistical validation yet — an honest limitation to acknowledge.
12. Where does wind speed/direction data come from? — The OpenWeather API, combined with site conditions and historical patterns.
13. How far in advance can the rainfall/weather AI reliably predict? — Uses OpenWeather API's forecast data combined with ML analysis of site conditions/historical patterns for near-term (same-day/next-few-hours) operational recommendations.
14. How is the satellite aerial view sourced? — Aerial/satellite imagery uploaded by supervisors combined with the site blueprint for the Site Planning feature — not a live drone feed.

**System-Wide**
15. How does the system scale to very large sites or multiple sites simultaneously? — Cones/devices can be deployed per-zone as needed; the REALG website acts as the centralized aggregation layer across all devices/zones reporting in.
16. What's the end-to-end latency from button-press to supervisor alert? — Not formally benchmarked in current documentation, but designed to be near-instant since it's a direct P2P LoRa hop with no gateway/internet round-trip in the critical path.
17. How is data privacy/security handled for continuous GPS tracking? — Not the primary focus of current documentation (SEAM's project has the deeper security treatment) — a fair acknowledgment that production deployment would need explicit data-retention and access-control policies for worker location data.
18. What's the per-unit cost, and is it feasible at scale? — ₹799 (worker device), ₹1,099 (supervisor device), ₹699 (cone) at prototype pricing — viable for bulk deployment, with further cost reduction possible via custom PCBs instead of dev boards (a listed future improvement).
19. How does the system handle multiple simultaneous emergency alerts? — Each alert carries a distinct worker device ID, so the supervisor device/dashboard can display and track multiple concurrent alerts independently rather than overwriting or conflating them.
20. What are the current limitations, and what's next? — Doesn't address unconscious workers (relies on active button press) — a natural next step is adding accelerometer-based fall/impact detection for automatic "man-down" alerts without requiring conscious action.

## Patent-Process Concepts Worth Knowing
| Concept | Simple Explanation |
|---|---|
| **Filing** | Formal submission of the patent application — starts the process. |
| **Publication** | Patent office makes the application public (~18 months after filing, or earlier on request) — REALG's Publication No. 24/2025 reflects this. **Publication ≠ Grant.** |
| **Request for Examination (RFE)** | A separate active step that starts substantive review of novelty/non-obviousness/utility. |
| **Grant** | Final approval of patent rights — only after examination is satisfactorily completed. |
| **IPC (International Patent Classification)** | Hierarchical system categorizing inventions by technical field (e.g., G08B = alarms) for search/examination. |
| **Inventive Step / Non-Obviousness** | Legal requirement that the invention isn't just an obvious combination of known elements — the toughest bar for combination-of-existing-parts inventions like REALG. |
| **Prior Art** | Existing public knowledge/inventions examiners compare your application against. |

**Current status if asked directly:** REALG is a filed and published patent application — meaning it's publicly visible and under/awaiting examination, **not yet a granted patent**. Being accurate about this distinction shows maturity and honesty rather than overstating the achievement.

**One-line pitch:** *"REALG is a patent-filed, LoRa-based IoT safety system for construction sites — a wearable, a supervisor device, and smart geofencing cones that communicate peer-to-peer with no cellular or Wi-Fi dependency, using RSSI signal strength for direction-independent hazard detection and a deliberate dual-button design to prevent false emergency alerts."*

---

# 6. Quick-Reference Cheat Sheet

| Project/Patent | Domain | Core Tech | One-Line Hook |
|---|---|---|---|
| **DocuTalk** | AI / NLP (RAG) | LangChain, ChromaDB, Ollama (Qwen), Nomic Embed | Local, offline PDF chatbot with source citations |
| **SEAM** | Identity Security / Biometrics | Face-API.js, SSD MobileNetV1, Azure, AES-256, LBP | Securing the ML model behind Aadhaar face auth |
| **Aegis** | FinTech / InsurTech | FastAPI, XGBoost, Isolation Forest, Razorpay | Auto-triggered, no-claim parametric wage insurance |
| **REALG (Patent)** | IoT / Construction Safety | LoRa, RSSI geofencing, Arduino, GPS | Infrastructure-free emergency alert + geofencing |
| **SARS (Patent)** | EdTech / Assistive Hardware | Raspberry Pi, OCR, Speech-to-Text, SymPy | Self-assisted exam device for SLD students |

**Before any interview, review in this order:**
1. Self-introduction (memorize, but sound natural, not scripted).
2. Top 3 strengths + 1 honest weakness, each with a concrete example.
3. One STAR story ready for "challenge," one for "failure," one for "teamwork/leadership."
4. One-line pitch for each project/patent — be ready to go 3 levels deep on whichever one the interviewer picks.
5. 2–3 thoughtful questions to ask the interviewer at the end.
