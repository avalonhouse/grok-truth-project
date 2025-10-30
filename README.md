# Grok Truth Project  
**Daily Truth Report from Fox News • CNN • MSNBC**

> *“Three networks, one reality — Grok finds the overlap.”*

---

## Mission
Ingest **24/7 live streams** from **Fox News**, **CNN**, and **MSNBC** (and any rebranded successors), extract **verifiable signal** from **partisan noise**, and publish a **single, evidence-anchored Daily Truth Report** by **6:00 AM ET** every day.

---

## Phase 1 – Signal Capture (24/7 Ingestion Layer)

| Component | Tech | Function |
|-----------|------|----------|
| **Multi-Tuner Farm** | 12× Hauppauge USB tuners + Raspberry Pi cluster | Capture raw ATSC/QAM streams in parallel |
| **Transcode & OCR** | FFmpeg + Tesseract OCR (real-time) | Convert closed-captioning + on-screen text to UTF-8 transcript |
| **Audio Fingerprinting** | Chromaprint + AcoustID | Tag every sentence with timestamp & speaker ID (voice biometrics) |
| **Chyron & Graphic Extractor** | OpenCV + YOLOv8 (trained on news tickers) | Pull lower-thirds, bug graphics, poll numbers |

**Output** – 3 × 24-hour JSONL files (one per network):

```json
{
  "timestamp": "2025-10-30T19:12:00Z",
  "speaker": "Sean Hannity",
  "verbatim_text": "FEMA just spent $59 million on luxury hotels for migrants in New York City.",
  "chyron_text": "BREAKING: $59M TO MIGRANT HOTELS",
  "graphic_screenshot_hash": "sha256:abc123..."
}

---

### **PART 2 of 3** – Paste Right After Part 1
```markdown
---

## Phase 2 – Claim Extraction & Fact-Chain Graph

1. **Claim Atomizer** (spaCy + custom NER)  
   - Split transcript into atomic claims: `"[Entity] [Relation] [Value]"`  
   - *Example*: `“FEMA → allocated → $59 million to NYC migrant hotels”`

2. **Deduplication & Cross-Network Merge**  
   - Fuzzy match identical claims (±5% Levenshtein distance)  
   - Build **Claim Graph**:  
     - Nodes = claims  
     - Edges = network + timestamp

3. **Evidence Triage Engine**  
   - For every claim, spawn micro-queries to:  
     - **Primary sources**: SEC filings, FEC, court dockets, BLS, NOAA, C-SPAN archives  
     - **Real-time X search**: keyword + geofence + verified accounts  
   - Confidence score: `P(evidence) ∈ [0,1]`

---

## Phase 3 – Bias & Noise Filters

| Filter | Method | Output |
|--------|--------|--------|
| **Spin Index** | Sentence-BERT cosine distance from primary source | `spin_score = cosine(primary, aired_version)` |
| **Omission Detector** | Compare claim graph vs. Wikipedia Current Events + GDELT | Flag missing context |
| **Emotion Amplifier** | VADER sentiment + prosodic stress from audio | Tag fear/hope/outrage spikes |
| **Panelist Partisan Lean** | Historical voting records + donation data (OpenSecrets) | Color-code speakers (R/D/I) |

---

## Phase 4 – Truth Synthesis (6:00 AM ET)

### Daily Truth Report Format  
**(3-page PDF + interactive dashboard)**

#### Page 1 – **The Signal**
- **Top 5 Verified Claims** (≥0.95 confidence) with primary-source hyperlinks  
- **Timeline Heatmap**: when each network first aired the claim  
  - e.g., Fox @ 19:03 | CNN @ 19:07 | MSNBC @ 19:12

#### Page 2 – **The Noise**
- **Spin Leaderboard**: which network distorted a claim the most  
- **Omission Radar**: critical context left out by ≥2 networks  
- **Outrage Meter**: 24-hour peak in emotional language

#### Page 3 – **The Gray Zone**
- Claims with `0.3 < P < 0.7` confidence  
- Live-updating evidence tracker (pulls new filings, tweets, etc.)

---

### **PART 3 of 3** – Paste Last
```markdown
---

## Phase 5 – Delivery & Feedback Loop

| Channel | Format |
|---------|--------|
| **Truth.App** (iOS/Android) | Push at 6:05 AM + live claim tracker |
| **X Bot** `@GrokTruth` | Thread with infographics |
| **API** | WebSocket for third-party fact-checkers |
| **Human Override Panel** | 3 rotating journalists (left / right / center) — can flag errors within 60 min |

---

## Sample Output (Oct 30, 2025 – Hypothetical)

### **SIGNAL**  
1. **Verified**: FEMA allocated $59 M to NYC migrant hotels *(primary: DHS budget line 73-B)*  
   - Fox: 19:12 | CNN: 19:45 | MSNBC: 20:03  
2. **Verified**: Trump rally in Duluth drew 18 k *(Duluth PD permit + drone count)*  

### **NOISE**  
- **Spin Champion**: MSNBC framed FEMA funds as “luxury” (`spin_score 0.83`)  
- **Omitted**: All networks skipped that **94%** of FEMA relief went to hurricane victims  

### **GRAY**  
- “Iran moved ballistic missiles to launch sites” – awaiting satellite confirmation (`P=0.41`)

---

## Open-Source Stack

```text
ingest/        → FFmpeg + Tesseract
claims/        → spaCy + Sentence-BERT
evidence/      → Selenium scrapers + APIs
graph/         → Neo4j
ui/            → Svelte + D3


**GitHub**: [github.com/avalonhouse/grok-truth-project](https://github.com/avalonhouse/grok-truth-project)  
**License**: MIT

---

## Launch Plan (30 Days, $0–$5K)

| Week | Goal | Who Does It | Cost |
|------|------|-------------|------|
| **Day 1–3** | Validate & Rally | You + X | $0 |
| **Day 4–10** | Open-Source MVP | Volunteer devs (HN, X, Reddit) | $0 |
| **Day 11–20** | Ingest Prototype | Freelancer (Upwork) | $1–3K |
| **Day 21–30** | Public Beta | You + community | $0–2K |

### Your Role (2 Hours/Week)
| Task | Time |
|------|------|
| Post updates on X | 15 min |
| Merge GitHub PRs (click “Approve”) | 10 min |
| Answer “Is this real?” → link to repo | 5 min |

---

## Funding (Optional Scaling)

| Source | Amount | How |
|--------|--------|-----|
| **GitHub Sponsors** | $500–$5K/mo | Add button |
| **X Community Notes** | $1K–$10K | Apply as civic project |
| **Civic Tech Grants** | $25K–$100K | Knight Foundation, Omidyar |

---

## How to Contribute
1. **Star** the repo  
2. **Fork** → implement a module (see `CONTRIBUTING.md`)  
3. Open a **Pull Request** – CI runs unit tests automatically  

---

*Built with*  
<image-card alt="Python" src="https://img.shields.io/badge/python-3.11-blue" ></image-card>  
<image-card alt="Docker" src="https://img.shields.io/badge/docker-%230db7ed.svg" ></image-card>  
<image-card alt="GitHub Actions" src="https://img.shields.io/badge/GitHub_Actions-2088FF?logo=github-actions&logoColor=white" ></image-card>

---

**Ready to launch?**  
Reply **“LAUNCH”** and I’ll generate the **first PR** with a working `ingest/` module.

---

**You don’t need to code. You just need to start.**  
**The world needs this. Let’s build it.**
