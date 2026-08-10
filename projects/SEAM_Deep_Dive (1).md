# SEAM — Secure Encryption and Authentication Model
## Complete Technical Deep-Dive & Interview Preparation Guide

---

## 1. Tech Stack

| Layer | Technology | Why it was chosen |
|---|---|---|
| Frontend | React + Vite | Fast dev builds, component-driven UI for the auth flow |
| UI Framework | Material UI | Pre-built, accessible components for forms/cards |
| Face Detection & Recognition | Face-API.js (built on TensorFlow.js) | Runs entirely in-browser, no server round-trip needed for inference |
| Detection Model | **SSD MobileNet V1** | Lightweight single-shot detector, fits the 5–7 MB size budget |
| Landmark Model | Face Landmark 68-point model | Locates eyes, nose, jawline — used for liveness + alignment |
| Recognition Model | Face Recognition model (128-D descriptor) | Converts a face into a numeric vector for comparison |
| Anti-Spoofing | Local Binary Patterns (LBP) + liveness (blink/motion) | Texture-based + behavior-based spoof detection |
| Backend | Node.js on Azure App Services | Hosts APIs, model files, and business logic |
| Database | Azure SQL Database | Stores Aadhaar numbers, face descriptors (encrypted) |
| Model Security | AES-256-CBC encryption, JS obfuscation, SRI, hash verification | Protects the ML model + code from theft/tampering |
| Testing/Security Audit | OWASP ZAP | Automated penetration testing |
| Performance Testing | Google Lighthouse | Simulated 3G/4G/5G network scoring |
| Deployment | Web app + Chrome extension (Chrome Web Store) | Cross-context accessibility |

---

## 2. The Problem Statement (PS)

**SIH1670** — *"Develop a functional solution that incorporates the security of the ML model."*

Two halves to this, and both had to be solved together:

1. **Functional half:** Build a real-time, browser-based Aadhaar face authentication system — a user enters their Aadhaar number, shows their face to a webcam, and gets authenticated (like a "Face ID" but for Aadhaar), while distinguishing a live person from someone spoofing with a photo/video.
2. **Security half (the actual crux of the PS):** The ML model used for this face matching is valuable and sensitive — if attackers can steal, tamper with, or reverse-engineer it, they could bypass authentication entirely or extract how the matching logic works. So the model itself needed **encryption, tamper-detection, and protection from reverse engineering** — not just the usual "secure the API with HTTPS" approach.

In plain words: *"Make a fast face-login system for Aadhaar that works in any browser, catches fake faces, and — critically — can't be hacked, copied, or tampered with at the model level."*

---

## 3. The Solution — High-Level Summary

I built a system where:
- The **face detection + recognition runs client-side** (in the browser) using Face-API.js models, so there's no lag from server round-trips for every frame.
- **Model files are encrypted (AES-256-CBC) at rest** on the backend and only decrypted/verified when legitimately requested.
- Before the browser trusts a downloaded model file, it **verifies its hash** against a server-side manifest — if a file was tampered with in transit or on disk, the hash won't match and the app rejects it.
- **Liveness detection + LBP texture analysis** run alongside face matching to reject spoofing attempts (photos, videos, printed faces).
- Sensitive data (Aadhaar numbers, face descriptors) is **never cached** — it's loaded into backend RAM only for the duration of a single request and wiped immediately after ("Just-in-Time" data handling), while only the (non-sensitive) model files are cached in the browser for performance.
- Every authentication attempt — success or failure — is **logged with metadata** (name, Aadhaar number, IP, timestamp) into an admin dashboard for auditing.
- The whole system was **stress-tested** with OWASP ZAP (attack simulation) and Lighthouse (network performance across 3G/4G/5G).

---

## 4. Feature-by-Feature Deep Dive

### 4.1 Face Detection — SSD MobileNet V1

**What it does:** Scans the webcam frame and draws a bounding box around any face(s) present — this is the very first step before anything else can happen.

**Why SSD MobileNet V1 specifically:**
- **SSD (Single Shot Detector)** predicts bounding boxes and confidence scores in a single forward pass through the network — much faster than two-stage detectors (like Faster R-CNN) which first propose regions and then classify them.
- **MobileNet V1** is the backbone — it uses **depthwise separable convolutions** instead of standard convolutions, drastically cutting the number of parameters and computation, which is why the whole model stays in the 5–7 MB range needed for fast downloads on 3G/4G.

**Follow-up questions you could get:**
- *"Why not use a more accurate model like ResNet or YOLOv5?"* → Those are much heavier (tens to hundreds of MB) and would blow the size/latency budget for a browser-based, low-bandwidth use case; SSD MobileNetV1 trades a little raw accuracy for massive gains in speed and size, which is the right trade-off for real-time client-side inference.
- *"What is a depthwise separable convolution and why does it reduce size?"* → It splits a standard convolution into two steps — a depthwise convolution (filters each input channel separately) followed by a pointwise (1×1) convolution (combines the outputs) — this factorization needs far fewer multiplications than a standard convolution, cutting both compute and parameter count.
- *"What happens if the model detects multiple faces?"* → In our boundary-case testing, multiple detected faces cause the system to reject authentication (see Boundary Cases table) rather than guessing which face is the real user — a security-conscious design choice.
- *"What's the confidence threshold for detection, and what happens below it?"* → Below the threshold, no bounding box is drawn and the UI shows "No face detected," blocking the user from proceeding to the compare step.

**Topics to study:**
- Convolutional Neural Networks (CNN) basics
- Single Shot Detectors vs two-stage detectors (SSD vs Faster R-CNN)
- MobileNet architecture (depthwise separable convolutions)
- Non-Maximum Suppression (NMS) — how multiple overlapping boxes are reduced to one

---

### 4.2 Facial Landmark Detection — 68-Point Model

**What it does:** Once a face is detected, this model pinpoints 68 specific (x, y) coordinates on the face — eyes, eyebrows, nose bridge, nose tip, mouth corners, jawline.

**Why it matters here:**
- Used for **face alignment** — normalizing the face's orientation before generating its descriptor, so a slightly tilted head doesn't throw off the match.
- Used for **liveness detection** — tracking eye-landmark positions over consecutive frames to detect blinking (a live face's eye-landmark distance changes rhythmically; a photo's doesn't).

**Follow-up questions:**
- *"How exactly do you detect blinking from landmarks?"* → By calculating the Eye Aspect Ratio (EAR) — the ratio of vertical eye-landmark distances to horizontal ones — which drops sharply when the eyes close and recovers when they open; monitoring this ratio over time reveals blink patterns.
- *"Why 68 points specifically and not more/fewer?"* → 68-point landmark detection is a well-established standard (from the iBUG 300-W dataset conventions) that balances enough facial detail for alignment/liveness against inference speed; more points (e.g., 194) would add compute cost without proportional benefit for this use case.
- *"What if the person is wearing glasses or a mask — does landmark detection still work?"* → From boundary testing: glasses alone still detect and authenticate fine; a mask covering key landmarks (nose, mouth) fails authentication because those points aren't reliably locatable.

**Topics to study:**
- Facial landmark detection algorithms (regression-based landmark models)
- Eye Aspect Ratio (EAR) formula for blink detection
- Face alignment / affine transformation basics

---

### 4.3 Face Recognition (Descriptor Generation & Matching)

**What it does:** Converts the aligned face into a **128-dimensional numeric vector (descriptor)** — essentially a unique "fingerprint" of that face. This is done both for the live webcam capture and for the pre-registered Aadhaar photo, and the two vectors are compared.

**How matching works:**
- Euclidean distance is calculated between the live descriptor and the stored descriptor.
- A **similarity threshold of 0.45** is used — if the distance is below this threshold, it's considered a match (lower distance = more similar).

**Follow-up questions:**
- *"Why Euclidean distance and not cosine similarity?"* → Face-API.js's underlying model (based on a FaceNet-style architecture) is trained specifically so that Euclidean distance between embeddings directly correlates with facial similarity — it's the natural metric for this embedding space, whereas cosine similarity is more common for text/semantic embeddings.
- *"Why 0.45 as the threshold — how was that chosen?"* → It's a balance between False Acceptance Rate (FAR) and False Rejection Rate (FRR) — a lower threshold (stricter) reduces false acceptances (impostors getting in) but increases false rejections (real users getting blocked); 0.45 was tuned empirically during testing to balance both for this dataset.
- *"What's a false positive/negative in this context, and which is worse for Aadhaar auth?"* → A false positive (accepting an impostor) is far worse for an identity system than a false negative (rejecting a real user and asking them to retry) — so the threshold is generally tuned conservatively toward fewer false acceptances.
- *"How would twins be handled, since their faces are nearly identical?"* → Documented as a known boundary case — the system falls back to relying on the Aadhaar number itself as the distinguishing factor rather than facial similarity alone, since two identical twins can have descriptor distances below the threshold for each other.

**Topics to study:**
- Face embeddings / FaceNet-style triplet loss training
- Euclidean distance vs cosine similarity for embeddings
- False Acceptance Rate (FAR) / False Rejection Rate (FRR) trade-offs
- ROC curves and threshold tuning in biometric systems

---

### 4.4 Anti-Spoofing — Liveness Detection + LBP Texture Analysis

**What it does:** Two complementary techniques working together to reject non-live inputs (printed photos, phone/screen replays, video loops):

1. **Liveness detection (behavioral):** Monitors blinking and head movement over multiple frames. A static photo shows zero natural micro-movement/blinking over time → flagged as spoof due to inactivity.
2. **LBP — Local Binary Patterns (textural):** Analyzes the micro-texture of the skin surface. Real skin has natural texture variation (pores, subtle shading); a printed photo or screen has flatter, more repetitive/pixel-uniform texture patterns that LBP encodings can distinguish statistically.

**How LBP works (conceptually):** For each pixel, LBP compares it to its 8 surrounding neighbors — if a neighbor's intensity is greater, it's marked 1, else 0 — producing an 8-bit binary pattern per pixel. These patterns are turned into a histogram summarizing the texture of a region. Real faces and printed/screen faces produce statistically different histograms.

**Follow-up questions:**
- *"What kind of spoof attacks were tested, and did they succeed?"* → Two attack types tested: a photo displayed on a phone screen, and a physical printed photo. Both were correctly flagged as "Spoof Detected — Not Authenticated," confirming the anti-spoofing layer worked as intended.
- *"Would LBP alone be enough, without liveness detection?"* → No — LBP is vulnerable to high-quality prints/screens that closely mimic real texture, and liveness detection catches a different attack vector (a video replay might have realistic texture *and* motion, but liveness checks like natural randomness in blink timing add another layer). Using both in combination is more robust than either alone.
- *"What's the 'Post Liveliness' inactivity check you mention?"* → After a session is authenticated, the system also monitors for extended inactivity (e.g., the user freezing/not moving at all for a long window) and automatically expires the session, redirecting to the home page as an additional safety measure against session hijacking or someone leaving an authenticated session open.
- *"How would you defend against a deepfake or 3D mask attack, which this current LBP+liveness approach might not catch?"* → Honest answer: this is a real limitation — LBP-based anti-spoofing is a first-generation defense good against 2D photo/screen attacks, but sophisticated deepfakes or 3D silicone masks would need more advanced defenses like depth-sensing cameras (structured light/IR, like Face ID) or deep-learning-based liveness classifiers trained specifically on deepfake datasets — a good improvement to mention as future work.

**Topics to study:**
- Local Binary Patterns (LBP) algorithm and texture histograms
- Presentation Attack Detection (PAD) in biometrics — general taxonomy of spoof types (print, replay, mask, deepfake)
- Liveness detection techniques (active vs passive)

---

### 4.5 Model Security — Encryption, Obfuscation, Integrity Checks

**This is the actual core of the Problem Statement**, so expect deep questioning here.

**What was implemented:**
1. **AES-256-CBC encryption:** Model files are encrypted before being stored on the backend. AES (Advanced Encryption Standard) with a 256-bit key in CBC (Cipher Block Chaining) mode means each block of data is XORed with the previous ciphertext block before encryption, so identical plaintext blocks don't produce identical ciphertext — preventing pattern analysis.
2. **Code obfuscation:** Backend JavaScript is run through obfuscation tools (Babel + JS obfuscators) so that even if someone accesses the deployed code, variable names, control flow, and logic are scrambled into something extremely hard to reverse-engineer.
3. **Hash-based integrity verification:** Before the browser trusts a downloaded model file, it computes/checks a hash against a server-provided manifest. If a file was altered (by an attacker mid-transit, or corrupted on disk), the hash mismatch causes the app to reject it rather than silently loading a compromised model.
4. **Sub-Resource Integrity (SRI):** For any externally-loaded scripts/stylesheets, SRI hashes ensure the browser only executes them if they match the expected hash — protecting against a compromised CDN or man-in-the-middle script injection.
5. **DDoS Protection, HTTPS, CORS:** Azure's built-in DDoS protection stops volumetric attacks; HTTPS encrypts all data in transit; CORS configuration ensures only authorized domains/origins can call the backend APIs.

**Follow-up questions:**
- *"Why CBC mode specifically, and not ECB or GCM?"* → ECB (Electronic Codebook) is insecure because identical plaintext blocks always produce identical ciphertext, leaking patterns. CBC fixes that by chaining blocks together. GCM (Galois/Counter Mode) is actually a stronger modern choice since it provides both encryption *and* built-in authentication (integrity) in one step — a fair critique/improvement point to acknowledge if asked, since we added integrity separately via hashing rather than using an authenticated encryption mode.
- *"Where is the AES decryption key stored, and isn't that itself a vulnerability?"* → This is the classic "key management" problem in client-adjacent security — the key needs to live on the backend (never shipped to the browser), and access to it is restricted to the backend service identity on Azure; a good honest answer acknowledges that ultimately, whoever controls the backend infrastructure controls the key, so this really protects against *external* attackers/theft of the model at rest, not an attacker who fully compromises the server.
- *"What's the difference between encryption and obfuscation, and why do you need both?"* → Encryption makes data unreadable without a key (mathematically reversible with the right key); obfuscation makes *code* hard to understand/reverse-engineer but is still ultimately executable/readable with enough effort — it's a deterrent, not a hard security guarantee like encryption. We use encryption for the model *data* at rest and obfuscation for the logic/code that a user's browser must still be able to run.
- *"How does hash verification actually stop a tampered file from being used?"* → The server publishes a manifest of expected cryptographic hashes (e.g., SHA-256) for each model shard. After downloading a file, the client recomputes its hash and compares it to the manifest — any single-bit change in the file produces a completely different hash (avalanche effect), so tampering is detected with near-certainty before the model is ever loaded into memory.
- *"What did OWASP ZAP find, and how did you fix it?"* → Early scans flagged issues like missing security headers (e.g., anti-clickjacking headers, X-Content-Type-Options) and cache-control misconfigurations. These were fixed by adding proper HTTP security headers and cache directives; a later ZAP scan on the updated version showed injection attempts marked "out of scope," meaning the attack surface was successfully reduced.

**Topics to study:**
- Symmetric encryption fundamentals: AES, block cipher modes (ECB vs CBC vs GCM)
- Key management basics (where keys should/shouldn't live)
- Hashing vs encryption (one-way vs reversible)
- SHA-256 and the avalanche effect
- Code obfuscation vs minification vs encryption — different purposes
- OWASP Top 10 vulnerabilities (at least be able to name a few: injection, broken auth, security misconfiguration, XSS)
- Sub-Resource Integrity (SRI) — how the `integrity` attribute works in HTML

---

### 4.6 Just-in-Time Data Handling & Caching Strategy

**What it does:**
- **Only ML model files are cached** in the browser (via Cache API) — since these are non-sensitive, static assets, caching them dramatically speeds up repeat visits without any privacy risk.
- **User/session data (Aadhaar numbers, face descriptors) is never cached.** It's loaded into Azure backend RAM only for the duration of an active authentication request, then cleared immediately after the response is sent.

**Why this mattered:** An earlier version of the system *did* cache data records retrieved during Aadhaar validation, which caused two problems: (1) growing storage usage leading to memory management issues, and (2) a slower website overall, in addition to (3) an unnecessary privacy exposure — sensitive data sitting in browser storage longer than needed.

**Follow-up questions:**
- *"How do you 'clear' data from backend RAM — is that automatic in Node.js?"* → Node.js's garbage collector reclaims memory once there are no more references to an object; the implementation ensures request-scoped variables holding sensitive data go out of scope immediately after the response is sent, so nothing persists beyond that request's lifecycle — no explicit caching layer (like Redis) is used to store this data, unlike the model files.
- *"What's the actual measured improvement from this caching change?"* → From the demo screenshots: storage usage dropped from 63.6 MB (with data caching) to 24.3 MB (model-only caching) for a comparable session — roughly a 60%+ reduction, along with a faster perceived load time on repeat visits.
- *"Isn't caching only the model still a security risk since it's the 'protected' asset?"* → No — the model files, while proprietary, are protected via the encryption + hash-verification described above; caching the *encrypted/verified* model is safe because a cached copy is still checked for integrity, whereas caching *raw user PII* has no equivalent protection layer once it's sitting in browser storage.

**Topics to study:**
- Browser Cache API vs localStorage/IndexedDB — different persistence models
- Node.js garbage collection basics (reference counting / mark-and-sweep concept)
- Data minimization principle (a core privacy-by-design concept, relevant to GDPR/data protection thinking)

---

### 4.7 Logging, Monitoring & Admin Dashboard

**What it does:** Every authentication attempt (successful or failed) is logged with:
- User name, Aadhaar number, image reference
- Description (e.g., "Authenticated successfully" / "Authentication failed")
- IP address
- Timestamp

This feeds an **Admin Dashboard / User Logs Dashboard** where logs can be searched by Aadhaar number, useful for auditing and investigating suspicious activity (e.g., repeated failed attempts from one IP).

**Follow-up questions:**
- *"Isn't logging Aadhaar numbers and face images itself a privacy/compliance risk?"* → Yes, and this is a fair critique — in a production system, this data would need to be handled under strict data protection regulations (in India's context, the DPDP Act / UIDAI's own data-handling norms), likely requiring encryption of logs at rest, strict access control to the admin dashboard, log retention limits, and possibly storing only hashed/tokenized identifiers rather than raw Aadhaar numbers in logs.
- *"How would you detect brute-force or credential-stuffing style attacks from these logs?"* → By monitoring for patterns like many failed attempts against the same Aadhaar number or from the same IP within a short time window, and adding rate-limiting or temporary lockouts once a threshold is crossed — this wasn't fully implemented but is a natural extension of the existing logging infrastructure.
- *"What's the difference between the two log tables shown (one with 'IsMatched: True/False' and one with 'Description/IPAddress')?"* → One appears to be a raw match-result log (the biometric comparison outcome itself), and the other is a higher-level authentication-event log (with richer context like IP and human-readable description) — separating raw ML output from business-level audit trail is generally good practice, since they serve different purposes (debugging the model vs auditing user activity).

**Topics to study:**
- Basic security logging/audit trail best practices
- Data protection regulations relevant to biometric data (India's DPDP Act, GDPR as a general reference point)
- Rate limiting / brute-force protection concepts

---

### 4.8 Performance & Compatibility Testing

**What it does:** Used **Google Lighthouse** to simulate 3G/4G/5G network conditions and measure Core Web Vitals-style metrics:
- **FCP (First Contentful Paint)**, **LCP (Largest Contentful Paint)**, **SI (Speed Index)**, **TBT (Total Blocking Time)**, **CLS (Cumulative Layout Shift)**

**Follow-up questions:**
- *"What was your Lighthouse score, and what was the bottleneck?"* → From the demo, an overall score of 66, with **TBT (Total Blocking Time) scoring very poorly (metric score of 1)** despite good FCP/LCP/CLS — indicating the main-thread JavaScript execution (likely the ML model inference itself) was blocking user interaction responsiveness; this points to model inference being the main performance bottleneck rather than network loading.
- *"How would you fix a poor TBT score in a scenario like this?"* → Move heavy computation (model inference) off the main thread using Web Workers, so the UI stays responsive while inference runs in the background — a concrete, technically correct improvement to mention if asked.

**Topics to study:**
- Core Web Vitals (FCP, LCP, TBT, CLS, SI) — what each measures and why it matters
- Main thread blocking in JavaScript and Web Workers as a solution
- Network throttling / emulation for performance testing

---

## 5. Full List of Edge Cases to Know (Boundary Case Table)

| # | Case | What Happens | Why |
|---|---|---|---|
| 1 | Person wearing spectacles | Authenticated | Landmarks around eyes still detectable through most glasses |
| 2 | Person wearing a mask | Not Authenticated | Nose/mouth landmarks obscured — descriptor can't be reliably generated |
| 3 | Mask + specs together | Not Authenticated | Compounded landmark occlusion |
| 4 | Multiple faces in frame | Not Authenticated | System can't reliably determine which face belongs to the claimant; rejected as a security precaution |
| 5 | Bright lighting | Authenticated | Model is reasonably robust to lighting variance |
| 6 | Dark lighting | Authenticated | Same — though real-world extreme darkness would likely degrade this further (a good point to acknowledge as a limitation) |
| 7 | Non-human face / no face | Not Authenticated | No face detected → blocked before matching even starts |
| 8 | Anti-spoofing: photo on phone | Not Authenticated (Spoof Detected) | LBP + liveness catch the flat/static nature |
| 8 | Anti-spoofing: physical printed photo | Not Authenticated (Spoof Detected) | Same reasoning |
| 9 | Post-liveliness: user freezes after auth | Session expires due to inactivity | Extra safety layer against session hijacking |
| 10 | Twins | Authenticated via Aadhaar number, not face alone | Descriptor distance may be below threshold for both twins — a known limitation of face-only biometrics |
| 11 | Extreme camera angles | Not Authenticated | Landmark detection and alignment degrade sharply off-axis |
| 12 | Partial occlusion (hand covering face) | Not Authenticated | Key landmarks blocked |

**Extra edge cases worth mentioning even if not explicitly tested (shows depth):**
- Very low bandwidth / model fails to fully download (partial file) → should be caught by hash verification failing.
- Camera permission denied by the browser → should show a clear error state rather than a silent failure.
- Aadhaar number valid but no registered face on file → should show a distinct "no face on record" error rather than a generic failure.
- Concurrent authentication attempts for the same Aadhaar from two locations → a potential fraud signal worth flagging (not currently handled, good "future work" answer).
- Aging / significant appearance change (e.g., years since Aadhaar photo was taken, weight change, beard grown/shaved) → face descriptor drift over time is a known real-world biometric challenge (good to mention as a limitation).

---

## 6. Master List — Concepts to Study Before the Interview

**Computer Vision / ML:**
- CNNs, SSD object detection, MobileNet (depthwise separable convolutions)
- Facial landmark detection, Eye Aspect Ratio (EAR)
- Face embeddings (FaceNet-style), Euclidean distance vs cosine similarity
- FAR/FRR trade-offs, threshold tuning
- Local Binary Patterns (LBP), Presentation Attack Detection (PAD)

**Security:**
- AES encryption, block cipher modes (CBC vs ECB vs GCM)
- Hashing (SHA-256) and the avalanche effect
- Code obfuscation vs encryption vs minification
- Sub-Resource Integrity (SRI)
- OWASP Top 10, basics of using OWASP ZAP
- Key management fundamentals

**Systems/Performance:**
- Browser Cache API vs localStorage/IndexedDB
- Node.js event loop & garbage collection basics
- Core Web Vitals (FCP, LCP, TBT, CLS)
- Web Workers for offloading heavy computation

**Privacy/Compliance (good to have opinions on):**
- Data minimization principle
- India's DPDP Act / UIDAI norms (high-level awareness)
- Biometric data handling best practices

---

## 7. One-Line Elevator Pitch (memorize this)

> "SEAM is a browser-based Aadhaar face authentication system I built for Smart India Hackathon, where the real challenge wasn't just matching faces — it was securing the ML model itself. I used SSD MobileNetV1 for lightweight face detection, LBP and liveness checks to stop spoofing, and protected the model with AES-256 encryption, hash-based integrity verification, and code obfuscation, while keeping sensitive user data out of any cache entirely."
