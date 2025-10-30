# Grok Truth Project  
**Daily Truth Report from Fox News • CNN • MSNBC**

> *“Three networks, one reality — Grok finds the overlap.”*

---

## Mission
Ingest 24/7 streams from **Fox News**, **CNN**, and **MSNBC** (plus any rebranded successors), separate **verifiable signal** from **partisan noise**, and publish a **single, evidence-anchored Daily Truth Report** by **6:00 AM ET**.

---

## Phase 1 – Signal Capture (24/7 Ingestion)

| Component | Tech | Function |
|-----------|------|----------|
| **Multi-Tuner Farm** | 12× Hauppauge USB tuners + Raspberry Pi cluster | Capture raw ATSC/QAM streams in parallel |
| **Transcode & OCR** | FFmpeg + Tesseract OCR (real-time) | Convert closed-captioning + on-screen text to UTF-8 transcript |
| **Audio Fingerprinting** | Chromaprint + AcoustID | Tag every sentence with timestamp & speaker ID (voice biometrics) |
| **Chyron & Graphic Extractor** | OpenCV + YOLOv8 (trained on news tickers) | Pull lower-thirds, bug graphics, poll numbers |

**Output** – 3 × 24-hour JSONL files (one per network):

```json
{timestamp, speaker, verbatim_text, chyron_text, graphic_screenshot_hash}

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

## Phase 3 – Bias & Noise Filters

| Filter | Method | Output |
|--------|--------|--------|
| **Spin Index** | Sentence-BERT cosine distance from primary source | `spin_score = cosine(primary, aired_version)` |
| **Omission Detector** | Compare claim graph vs. Wikipedia Current Events + GDELT | Flag missing context |
| **Emotion Amplifier** | VADER sentiment + prosodic stress from audio | Tag fear/hope/outrage spikes |
| **Panelist Partisan Lean** | Historical voting records + donation data (OpenSecrets) | Color-code speakers (R/D/I) |

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

## Phase 5 – Delivery & Feedback Loop

| Channel | Format |
|---------|--------|
| **Truth.App** (iOS/Android) | Push at 6:05 AM + live claim tracker |
| **X Bot** `@GrokTruth` | Thread with infographics |
| **API** | WebSocket for third-party fact-checkers |
| **Human Override Panel** | 3 rotating journalists (left / right / center) — can flag errors within 60 min |
