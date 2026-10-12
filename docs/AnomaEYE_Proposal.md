# AnomaEYE — Real-Time CCTV Crime Detection with Verified Alerts and Multi-Channel Incident Response

**Submission for:** [4th International AI Championship (AI Showdown), Artificial Intelligence Education Foundation](https://www.aiedu.org.pk/4th-international-ai-championship/)
**Category:** AI in Urban Tech (smart-city infrastructure, mobility, and civic systems)
**Keywords:** video anomaly detection, real-time CCTV surveillance, vision-language verification, Safe City, crime detection
**Live product (full flow):** [www.anomaeye.com](https://www.anomaeye.com)
**Pitch deck:** [View on Canva](https://canva.link/6gh1y52uxf0r5n4)

---

## Team

<table>
  <tr>
    <td align="center" width="260" valign="top">
      <img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/team/talha.png" width="140" alt="M. Talha Farooqi"><br><br>
      <b>M. Talha Farooqi</b><br>
      <sub>AI Engineer</sub><br><br>
      <a href="https://github.com/M-Talha-Farooqi"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/github.svg" width="28" alt="GitHub"></a>&nbsp;
      <a href="https://www.linkedin.com/in/m-talha-farooqi"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/linkedin.svg" width="28" alt="LinkedIn"></a>&nbsp;
      <a href="mailto:the.mtalhafarooqi@gmail.com"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/email.svg" width="28" alt="Email"></a>
    </td>
    <td align="center" width="260" valign="top">
      <img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/team/husnain.png" width="140" alt="Muhammad Husnain Aslam"><br><br>
      <b>Muhammad Husnain Aslam</b><br>
      <sub>Backend Engineer</sub><br><br>
      <a href="https://github.com/heyhusn"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/github.svg" width="28" alt="GitHub"></a>&nbsp;
      <a href="https://www.linkedin.com/in/husnain-aslam-0a959a1a7"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/linkedin.svg" width="28" alt="LinkedIn"></a>&nbsp;
      <a href="mailto:mhusnainaslam2003@gmail.com"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/email.svg" width="28" alt="Email"></a>
    </td>
    <td align="center" width="260" valign="top">
      <img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/team/shoaib.png" width="140" alt="Chaudhary Shoaib Hamza"><br><br>
      <b>Chaudhary Shoaib Hamza</b><br>
      <sub>Flutter Developer · Founder, Big Brains</sub><br><br>
      <a href="https://github.com/theshoaibhamza"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/github.svg" width="28" alt="GitHub"></a>&nbsp;
      <a href="https://www.linkedin.com/in/shoaibhamza"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/linkedin.svg" width="28" alt="LinkedIn"></a>&nbsp;
      <a href="mailto:chaudharyshoaibhamza@gmail.com"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/email.svg" width="28" alt="Email"></a>
    </td>
  </tr>
</table>

**Institution / programme:** _[University, degree programme, expected graduation 2026 — required for eligibility proof]_
**Faculty supervisor:** _[Name, department — add if applicable]_

---

## 1. Problem statement

Pakistan's cities have invested heavily in cameras but not in watching them. The Lahore Safe City project alone installed 8,023 CCTV cameras at a cost of about Rs 12 billion ([ARY News](https://arynews.tv/punjab-cm-inaugurates-lahore-safe-city-project)), feeding a control room video wall of roughly 200 screens, and the Punjab Safe Cities Authority is now extending camera coverage to 21 cities ([Business Recorder](https://www.brecorder.com/news/amp/40286307)). A few dozen operators cannot watch thousands of feeds; in practice most footage is recorded, not monitored, and is reviewed only after a crime has been reported.

The volume of incidents makes this gap costly. Punjab recorded 171,784 reported crimes in 2025, including 25,010 robberies ([The News](https://www.thenews.pk/print/1402651-punjab-records-32pc-drop-in-crime)); Lahore alone logged 12,484 reported crimes in the first half of 2025 ([ARY News](https://arynews.tv/lahore-crime-statistics-drop-68-2025-signifying-improvements)). Private premises (housing societies, universities, hospitals, banks, factories) run their own camera networks with the same problem: one guard, many screens, no alert until someone complains.

**Why existing solutions fail here.** Commercial video analytics are priced for Western enterprises and tuned to Western footage; most offer motion or line-crossing alerts that drown operators in false alarms; and none close the loop from detection to a verified, logged response that reaches the right person on a phone. Academic anomaly detectors stop at a score on a benchmark.

## 2. Proposed solution

AnomaEYE turns passive CCTV recording into active, verified alerting. It watches live camera streams, scores every second of video for criminal or anomalous activity (fighting, assault, robbery, vandalism, arson, road accidents and similar events), and when something is detected it does what a good control room would do: a second, independent model looks at the clip before anyone is disturbed, the incident is recorded with a keyframe, a short evidence clip and a one-sentence description of what happened, and the right security staff are alerted at once by push notification, SMS and email, with a siren on site for severe cases. Supervisors follow it on a web dashboard, and every alert and every human action (acknowledge, escalate, resolve) is written to an audit trail.

**Why AI is necessary.** The bottleneck is human attention across thousands of streams; only a model can watch all of them at once. The bottleneck after detection is trust; only a verification step that can explain what it saw makes it safe to act on a machine alert.

## 3. System architecture

<p align="center"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/architecture/system-architecture.png" alt="AnomaEYE system architecture" width="100%"></p>

| Component | Single responsibility | Inputs | Outputs | Tools |
|---|---|---|---|---|
| Camera agent | Forwards each camera's stream from the site to the cloud without re-encoding it | RTSP cameras on the site LAN | SRT / RTMP stream to the media server | Python, FFmpeg |
| Media server | Holds every camera stream, serves live view to the apps, keeps a 5-minute rolling buffer; nothing is archived unless an event occurs | camera streams | WebRTC / HLS for viewers, RTSP for inference, clips on demand | MediaMTX |
| Scene gate | Scores every second of every camera and proposes candidate events | 2-second clips at 8 fps | per-second anomaly probability, coarse group, crime class | PyTorch: X3D-M + InternVideo2 encoders, causal temporal transformer (8-model ensemble) |
| Verifier | Takes a second, independent look at each candidate and vetoes false alarms | 8 frames of the candidate | confirm / veto, crime type, confidence, one-sentence description | Qwen3-VL-8B vision-language model (4-bit) |
| Evidence and delivery | Attaches a keyframe and clip, delivers the detection reliably | confirmed events | DetectionEvent to the backend, with retries if it is down | FastAPI, SQLite outbox |
| Backend API | Multi-tenant data, event lifecycle, evidence storage, alert fan-out, live updates, audit log | detections, user actions | events, alerts, WebSocket updates | FastAPI, Firestore, Firebase Auth, Cloud Storage, Redis |
| Alert channels | Reach staff on the channels they enabled; sound the site siren for severe events | event + recipient preferences | push, SMS, email, siren command | Firebase Cloud Messaging, Twilio, Resend, ESP32 siren with an on-site agent |
| Mobile app | Live cameras, alerts with the evidence clip, acknowledge / escalate / resolve | events, streams | staff actions | Flutter (Android, iOS) |
| Web dashboard | Staff, cameras, events, settings, audit log, live incident feed for administrators | all events | supervisor actions | React, Vite, hosted on Vercel |

**Safety and human-in-the-loop.** The verifier must confirm a candidate before any alert leaves the system; what it rejects is kept for audit, never published. No alert is acted on automatically beyond notifying people: escalation is a human action in the app, recorded with who did it and when. Destructive actions (deleting a camera or evidence) require a recent re-authentication. Every model verdict and human action is written to an append-only audit log per organisation.

## 4. Data

| Dataset | Use | Size / licence | Link |
|---|---|---|---|
| UCF-Crime | Training and held-out testing of the scene gate; event timestamps from HIVAU-70k | 1,900 untrimmed surveillance videos, 13 anomaly classes; research use | https://www.crcv.ucf.edu/projects/real-world/ |
| MSAD | Multi-scene CCTV anomalies: training and held-out testing of false-alarm behaviour on unseen scenes | 240 test videos; research use | https://msad-dataset.github.io/ |
| CamNuvem | Real CCTV robbery footage, held-out test | 49 test videos; research use | https://github.com/davidhiroshi/CamNuvem |
| XD-Violence, RWF-2000 | Additional violence footage for training and calibration | research use | — |
| SOHAS, YouTube-GDD | Weapon detector (gun, knife, plus distractors such as phones and wallets) | CC BY-SA 4.0 / Apache-2.0 | — |
| Operator feedback | Every confirm / dismiss / reclassify from the apps is stored as a labelled example for recalibration and retraining on the customer's own cameras | grows with deployment; no licence restriction | internal |

Training uses video-level and event-time labels only; no frame-level annotation was required. Faces are never stored or matched, and clips are kept only around a confirmed event.

## 5. Models and methods

- **Input protocol.** Each camera is sampled at 8 fps into 16-frame clips (2 seconds), one clip per second, with the whole frame resized to 224 × 224 — no centre crop, because CCTV events happen at the edges of the frame.
- **Scene gate.** Two frozen video encoders (X3D-M for motion, InternVideo2-CLIP for semantics) feed a causal temporal transformer that outputs, every second, an anomaly probability, a coarse group (violence, weapon, fire, vehicle, theft, incident) and a fine class (13 UCF-Crime classes). Eight independently trained models are averaged. Causal attention means training on clips and serving on a live rolling window are the same computation.
- **Event engine.** Smoothed, calibrated probabilities with hysteresis open an event after two consecutive hops and close it after five, with a per-camera adaptive baseline. A candidate opens at p ≥ 0.50.
- **Verifier.** A vision-language model (Qwen3-VL-8B, 4-bit) sees 8 frames of each candidate and answers whether a crime is visible, which one, and in one sentence what happened. A candidate is published if the verifier agrees (p ≥ 0.05) or the gate is very sure (peak ≥ 0.86). If the verifier is unavailable, the gate alone publishes at a stricter threshold and the alert is marked "unverified".
- **Compute.** Every camera shares one GPU batch, so a single 16 GB GPU serves roughly 15 to 20 cameras; the expensive verifier runs only on candidates, a few times a day per camera.
- **Why this design.** Scene-level scores alone overfit to scenes rather than acts (a man beside a motorcycle at night reads as theft); a second model that must explain what it sees removes most of those alarms. Everything in the pipeline is cheap except the step that runs rarely.

## 6. Evaluation and results

Held-out test sets, never used for training, model selection or calibration. An alert counts as correct when it overlaps an annotated event within 5 s; false alerts are counted per hour of normal video. v2 is the previously deployed model; v4 is the current gate + verifier system.

| Test set | v2 events caught | v2 false alerts / h | v4 events caught | v4 false alerts / h |
|---|---|---|---|---|
| UCF-Crime test (290 videos) | 46.8% | 0.17 | 59.6% | 0.50 |
| MSAD multi-scene CCTV (240 videos) | 25.2% | 0.00 | 69.2% | 3.63 |
| CamNuvem robbery CCTV (49 videos) | not tested | — | 100% | 0.00 |

| Metric | Value | Notes |
|---|---|---|
| Validation operating point | 71.9% of events caught at 0.23 false alerts / h | chosen on a scene-disjoint validation split |
| Verifier discrimination (true vs. false candidates) | AUC 0.83 | Qwen3-VL-4B; the deployed 8B model is the larger profile |
| Time to open an event | about 2 s after onset, plus 1–2 s verification | measured on test videos |
| Weapon detector | mAP50 0.88 (gun 0.83, knife 0.99) | optional per camera; off by default on low-resolution feeds |
| Inference capacity | 15–20 cameras per 16 GB GPU (RTX 5080) | about 6 on an 8 GB RTX 3070 |
| Detection-to-phone latency, end to end | _[to be measured on the finale build]_ | camera → GPU → backend → push |

**Ablation.** Gate alone vs. gate + verifier on UCF-Crime test: 28.8% → 59.6% of events caught at the same 0.5 false alerts / h budget. On CamNuvem: 96.4% → 100%.

**Known failure cases.** (1) Scene priors: the gate learned that a person handling a motorcycle at a petrol station at night is usually theft; the verifier now vetoes most of these. (2) Short, subtle person-level events (median 5–6 s assaults and thefts, people 20–40 px tall) are still missed. (3) Naming the crime is the weakest part: the group ("violence", "theft") is reliable, the specific class is right about half the time, so the apps headline the group and show the class as a tag. (4) Public test sets are small; the number that matters is alerts per camera per day on a customer's own cameras, which the pilot below measures.

## 7. Prototype status (what works today)

- [x] Scene gate trained on CCTV crime datasets, calibrated on unseen scenes
- [x] Vision-language verifier gating every alert, with a one-sentence description
- [x] Real-time inference over live RTSP cameras on a GPU host; file analysis for recorded clips
- [x] Evidence: keyframe and clip per event, stored privately and shared by short-lived links
- [x] Backend API: multi-tenant organisations, roles, event lifecycle, audit log, live updates
- [x] Alerts over push, SMS, email and an on-site siren
- [x] Flutter mobile app: live cameras, alerts with clips, acknowledge / escalate / resolve
- [x] Web dashboard for administrators: staff, cameras, events, settings, audit
- [x] Operator feedback endpoint (confirm / dismiss / reclassify) and event log for retraining
- [ ] Feedback buttons in the apps wired to that endpoint — for the finale
- [ ] 72-hour shadow deployment on a customer's cameras to measure alerts per camera per day — for the finale
- [ ] Automated hand-over to police or emergency services — not built; escalation is a human action with an audit record

**See the full flow live:** [www.anomaeye.com](https://www.anomaeye.com) · **Pitch deck:** [View on Canva](https://canva.link/6gh1y52uxf0r5n4)

## 8. Real-world impact and deployment path

**First users.** Private-premises security teams where one guard watches many screens: housing societies, university campuses, hospitals, bank branches and factories in Lahore. **Second user:** Punjab Safe Cities Authority control rooms, as an assistive layer that ranks which feeds an operator should look at now.

**90-day pilot plan.** Month 1: deploy on 8 to 16 cameras at one campus or housing society in shadow mode (alerts logged, not sent) to measure alerts per camera per day; then switch on alerts to two guards and one supervisor. Month 2: compare alerts against the site's incident log; recalibrate on the site's own footage and operator feedback. Month 3: enable the siren and supervisor-level escalation; publish results.

**Cost.** One 16 GB GPU serves 15 to 20 cameras; the backend runs on a single small cloud VM; apps run on existing phones. Per-camera monthly cost is a fraction of one guard's salary.

**Sustainability and ethics.** No face recognition and no identity matching. Video is kept in a 5-minute rolling buffer; only the window around a confirmed event is stored, privately, for a configurable retention period. Bias risk: the training data is largely non-Pakistani footage, so the system is calibrated on each site's own normal footage before alerts are switched on, and operator feedback retrains it on local conditions. Misuse risk: tenant isolation, roles and the audit log make every alert and every action attributable to a person.

## 9. Novelty

- **Detection to response, not detection alone.** Most academic and student systems stop at an anomaly score; AnomaEYE ships the verification, evidence, alerting, apps and audit trail needed to act on it, and it is deployed end to end today.
- **Verified alerts.** A second, independent vision-language judgement with a written explanation gates every alert, which is what cuts the false-alarm problem that makes operators ignore analytics.
- **It learns from its operators.** Every confirm, dismiss and reclassify becomes a labelled example from the customer's own cameras — the only data with no licence restriction and an exact match to the deployment.
- **Built for Pakistani deployment.** Cheap always-on scoring with expensive verification only on candidates keeps the hardware to one GPU; a site needs no inbound ports or static addresses; and the staff experience is a phone app with SMS as a fallback, matching how security teams here actually work.

## 10. Team roles and timeline

| Member | Built | Before 24 Oct |
|---|---|---|
| M. Talha Farooqi | Scene gate and verifier models, training and evaluation pipeline, GPU inference service | Shadow-deployment measurements, evaluation tables, pitch |
| Muhammad Husnain Aslam | Backend API, multi-tenancy, media pipeline, alert fan-out, siren, deployment | Feedback endpoints in both apps, load testing, audit views |
| Chaudhary Shoaib Hamza | Flutter app: live view, alerts, event actions, onboarding | Confirm / dismiss / reclassify in the app, finale demo build, offline fallback |

**Timeline:** 12 Oct submission (this document and repository); 13 to 16 Oct feedback buttons and shadow deployment; 16 Oct shortlist; 17 to 23 Oct rehearsal, offline hardening, backup video; 24 Oct finale at FAST-NUCES Lahore.
