# grok-truth-project
Grok monitors the news networks and distills the signal from the noise, fact from fiction.

Grok Truth Project: Daily Truth Report Pipeline
Mission: Ingest 24/7 streams from Fox News, CNN, and MSNBC (plus any rebranded successors), separate verifiable signal from partisan noise, and distill a single, evidence-anchored Daily Truth Report by 6:00 AM ET.

Phase 1: Signal Capture (24/7 Ingestion Layer)






























ComponentTechFunctionMulti-Tuner Farm12× Hauppauge USB tuners + Raspberry Pi clusterCapture raw ATSC/QAM streams in parallelTranscode & OCRFFmpeg + Tesseract OCR (real-time)Convert closed-captioning + on-screen text to UTF-8 transcriptAudio FingerprintingChromaprint + AcoustIDTag every spoken sentence with timestamp & speaker ID (using voice biometrics)Chyron & Graphic ExtractorOpenCV + YOLOv8 (trained on news tickers)Pull lower-thirds, bug graphics, poll numbers
Output: 3 × 24-hour JSONL files (one per network) with fields:
{timestamp, speaker, verbatim_text, chyron_text, graphic_screenshot_hash}

Phase 2: Claim Extraction & Fact-Chain Graph

Claim Atomizer (spaCy + custom NER)

Split transcript into atomic claims: "[Entity] [Relation] [Value]"
Example: “Kamala Harris → raised → $1 billion in 48 hours”


Deduplication & Cross-Network Merge

Fuzzy match identical claims across networks (±5% Levenshtein)
Build Claim Graph: nodes = claims, edges = network + timestamp


Evidence Triage Engine

For every claim, spawn micro-queries to:

Primary sources (SEC filings, FEC, court dockets, BLS, NOAA, etc.)
Archived C-SPAN / government pressers
Real-time X search (keyword + geofence + verified accounts)


Confidence score: P(evidence) ∈ [0,1]




Phase 3: Bias & Noise Filters






























FilterMethodOutputSpin IndexMeasure semantic distance from primary source using Sentence-BERTspin_score = cosine(primary, aired_version)Omission DetectorCompare claim graph to known event corpus (Wikipedia Current Events + GDELT)Flag missing contextEmotion AmplifierVADER sentiment + prosodic stress from audioTag fear/hope/outrage spikesPanelist Partisan LeanHistorical voting records + donation data (OpenSecrets)Color-code speakers

Phase 4: Truth Synthesis (6:00 AM ET)
Daily Truth Report Format (3-page PDF + interactive dashboard)
Page 1 – The Signal

Top 5 Verified Claims (≥0.95 confidence) with primary-source hyperlinks
Timeline Heatmap: when each network first aired the claim (Fox @ 19:03, CNN @ 19:07, MSNBC @ 19:12)

Page 2 – The Noise

Spin Leaderboard: which network distorted a claim the most
Omission Radar: critical context left out by ≥2 networks
Outrage Meter: 24-hour peak in emotional language

Page 3 – The Gray Zone

Claims with 0.3 < P < 0.7 confidence
Live-updating evidence tracker (pulls new filings, tweets, etc.)


Phase 5: Delivery & Feedback Loop

























ChannelFormatTruth.App (iOS/Android)Push at 6:05 AM + live claim trackerX Bot (@GrokTruth)Thread with infographicsAPIWebSocket for third-party fact-checkersHuman Override Panel3 rotating journalists (left, right, center) can flag errors within 60 min

Sample Output (Oct 30, 2025 – Hypothetical)
SIGNAL

Verified: FEMA allocated $59 M to NYC migrant hotels (primary: DHS budget line 73-B).

Fox: 19:12 ✅ | CNN: 19:45 ✅ | MSNBC: 20:03 ✅


Verified: Trump rally in Duluth drew 18 k (Duluth PD permit + drone count).

NOISE

Spin Champion: MSNBC framed FEMA funds as “luxury” (spin_score 0.83).
Omitted: All networks skipped that 94% of FEMA relief went to hurricane victims.

GRAY

“Iran moved ballistic missiles to launch sites” – awaiting satellite confirmation (P=0.41).
