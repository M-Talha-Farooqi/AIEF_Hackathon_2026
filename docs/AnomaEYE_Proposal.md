# AnomaEYE
### AI-Powered Real-Time Video Anomaly Detection and Incident Response System

- **Submission for:** [4th International AI Championship (AI Showdown), Artificial Intelligence Education Foundation](https://www.aiedu.org.pk/4th-international-ai-championship/)
- **Proposed category:** AI in Urban Tech (smart-city infrastructure, mobility, and civic systems)
- **Keywords:** video anomaly detection, real-time CCTV surveillance, vision-language verification, Safe City, crime detection
- **Team:** M. Talha Farooqi (AI Engineer) · Muhammad Husnain Aslam (Backend Engineer) · Chaudhary Shoaib Hamza (Flutter Developer)
- **Institution:** University of Management and Technology (UMT) · BS Software Engineering · Graduated (July 2026)
- **Project supervisor:** Dr. Syed Farooq Ali
- **Project landing page:** [www.anomaeye.com](https://www.anomaeye.com) · **Pitch deck:** [View on Canva](https://canva.link/6gh1y52uxf0r5n4)
- **Research basis:** STAGE, under review at *Image and Vision Computing* (Elsevier)

---

## 1. Project Overview & Problem Statement

### Overview

We propose **AnomaEYE**, an AI-powered real-time video anomaly detection and incident response system. AnomaEYE will add an intelligence layer on top of the CCTV cameras an organisation already has, so that cameras help identify dangerous or unusual activity **while it is happening**, instead of only storing footage for later review.

The system will score every second of every camera for criminal or anomalous activity, have a second, independent vision-language model verify each detection before anyone is disturbed, and alert the right staff within seconds through a mobile app, a web dashboard, SMS, email and an on-site siren, with the evidence clip attached. Its detection backbone will follow the approach of our research paper **STAGE**, currently under review at *Image and Vision Computing* (Elsevier).

### Problem statement

Most CCTV systems are passive. They record continuously, but they depend on human operators to notice incidents at the right moment. Where there are many cameras, no security team can watch every screen, so fighting, theft, robbery, vandalism, intrusion and accidents are often noticed late, or only when someone reviews footage after a complaint.

Pakistan's cities have invested heavily in cameras but not in watching them. The Lahore Safe City project alone installed 8,023 CCTV cameras at a cost of about Rs 12 billion ([ARY News](https://arynews.tv/punjab-cm-inaugurates-lahore-safe-city-project)), feeding a control-room video wall of roughly 200 screens, and the Punjab Safe Cities Authority is extending coverage to 21 cities ([Business Recorder](https://www.brecorder.com/news/amp/40286307)). The volume of incidents makes the gap costly: Punjab recorded 171,784 reported crimes in 2025, including 25,010 robberies ([The News](https://www.thenews.pk/print/1402651-punjab-records-32pc-drop-in-crime)), and Lahore alone logged 12,484 in the first half of 2025 ([ARY News](https://arynews.tv/lahore-crime-statistics-drop-68-2025-signifying-improvements)). Private premises (housing societies, universities, hospitals, banks, factories) face the same problem at smaller scale: one guard, many screens, no alert until someone complains.

**Why existing solutions fall short.** Commercial video analytics are priced for Western enterprises and tuned to Western footage. Most offer motion or line-crossing alerts that drown operators in false alarms, and they do not close the loop from detection to a verified, logged response that reaches the right person on a phone. Academic anomaly detectors stop at a benchmark score and lose accuracy when moved to new cameras and scenes.

---

## 2. Proposed Solution

AnomaEYE will turn passive CCTV recording into active, verified alerting. It will watch live camera streams and score every second of video for criminal or anomalous activity (fighting, assault, robbery, vandalism, arson, road accidents and similar events). When something is detected, it will do what a good control room does: a second, independent model will look at the clip before anyone is disturbed; the incident will be recorded with a keyframe, a short evidence clip and a one-sentence description; and the right security staff will be alerted at once, with a siren on site for severe cases. Supervisors will follow incidents on a web dashboard, and every alert and human action (acknowledge, escalate, resolve) will be written to an audit trail.

The key design choice is to go beyond object detection. Detecting people in a frame does not say whether their behaviour is normal; AnomaEYE will model **behaviour over time** and localise **when** suspicious activity develops.

**Why AI is necessary.** The bottleneck is human attention across thousands of streams; only a model can watch all of them at once. The bottleneck after detection is trust; only a verification step that can explain what it saw makes it safe to act on a machine alert.

### Proposed workflow

1. A small **camera agent** on site will forward each camera's RTSP stream to a cloud media server, with no re-encoding.
2. The **GPU inference service** will pull each registered camera's stream.
3. A **scene gate** will score every second of every camera and propose candidate events; a **verifier** will confirm or veto each one and describe it in one sentence.
4. A **verified detection** will be delivered to the backend with a keyframe and clip.
5. The **backend** will store the incident, cut the evidence clip from a rolling buffer, and send alerts.
6. **Staff** will be alerted on phone and dashboard; for severe events, a **siren** will sound on site.

### Proposed system architecture

<p align="center"><img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/architecture/system-architecture.png" alt="AnomaEYE proposed system architecture" width="100%"></p>

| Component | Role in the proposed system | Planned tools |
|---|---|---|
| Camera agent | Forward each camera's stream from the site to the cloud, outbound only, without re-encoding | Python, FFmpeg, SRT / RTMP |
| Media server | Receive every stream, serve live view to the apps, keep a 5-minute rolling buffer; archive nothing unless an event occurs | MediaMTX (WebRTC, HLS fallback) |
| Scene gate | Score every second of every camera and propose candidate events with a crime group and class | PyTorch: X3D-M + InternVideo2-CLIP encoders, causal temporal model, ensemble |
| Verifier | Take a second, independent look at each candidate, confirm or veto it, and describe it in one sentence | Qwen3-VL vision-language model (quantised) |
| Evidence and delivery | Attach a keyframe and clip; deliver detections reliably, with retries if the backend is down | FastAPI, local outbox |
| Backend API | Multi-tenant data, incident lifecycle, evidence storage, alert fan-out, live updates, audit log | FastAPI, Firestore, Firebase Auth, Cloud Storage, Redis (Upstash) |
| Alert channels | Reach staff on the channels they enable; sound the site siren for severe events | Firebase Cloud Messaging, Twilio (SMS), Resend (email), ESP32 + relay + 12V siren |
| Mobile app | Live cameras, alerts with the evidence clip, acknowledge / escalate / resolve | Flutter (Android, iOS) |
| Web dashboard | Live incident feed, cameras, staff, alert settings, audit log | React, hosted on Vercel |

### Proposed AI approach

- **Research foundation.** The detection backbone will follow the approach of our paper **STAGE** (see Additional Details), adapted for live, multi-camera serving.
- **Input.** Each camera will be sampled at 8 fps into 2-second clips, one per second, using the whole frame (no centre crop), because CCTV events often happen at the edges of the frame.
- **Scene gate.** Frozen video encoders (X3D-M for spatio-temporal appearance and motion, InternVideo2-CLIP for semantics) will feed a causal temporal model that outputs, every second, an anomaly probability, a coarse group (violence, weapon, fire, vehicle, theft, incident) and a fine crime class. Causal modelling means training on clips and serving on a live rolling window are the same computation. An ensemble of independently trained models will improve stability.
- **Event engine.** Calibrated probabilities with hysteresis and a per-camera adaptive baseline will open and close events, so single noisy seconds do not trigger alerts.
- **Verifier.** A vision-language model will see 8 frames of each candidate and answer whether a crime is visible, which one, and in one sentence what happened. If the verifier is unavailable, only very confident detections will be published, marked "unverified".
- **Why this design.** Scene-level scores alone can overfit to scenes rather than acts (a man beside a motorcycle at night can read as theft); a second model that must explain what it sees removes most of those alarms. The expensive step runs only on candidates, so the always-on part stays cheap.

**Safety and human-in-the-loop.** No alert will leave the system without verification, and nothing will be acted on automatically beyond notifying people: escalation will be a human action in the app, recorded with who did it and when.

---

## 3. Objectives & Expected Outcomes

### Objectives

1. Analyse live CCTV and IP camera streams in real time, using existing cameras.
2. Detect unusual and criminal activity from behaviour over time, not single frames.
3. Identify the time period in which an anomaly is most likely happening.
4. Reduce false alarms with a verification step, so operators keep trusting the alerts.
5. Generate real-time incidents with evidence: keyframe, clip and a one-sentence description.
6. Deliver alerts through a web dashboard, a mobile app, SMS, email and a physical siren.
7. Reduce the need for continuous manual monitoring of every camera feed.
8. Generalise to new environments and camera conditions, guided by the STAGE research.
9. Protect privacy: no face recognition, minimal retention, full audit trail.

### Expected outcomes

- A **working prototype** demonstrating the complete flow at the finale: live camera, normal activity, an anomalous event, AI detection, verification, incident creation, and alerts on the dashboard, the phone and the physical siren.
- **Verified alerts within seconds** of an event's onset, each with evidence attached.
- **A low false-alert rate** on held-out CCTV footage, measured as false alerts per hour of normal video.
- **Measured system performance:** detection-to-phone latency and the number of cameras one GPU can serve.
- **A research contribution:** the STAGE paper (under review at *Image and Vision Computing*), with its approach carried into a practical system.

### Preliminary results (feasibility)

Our early prototype of the detection pipeline already shows the approach is feasible. On held-out test sets never used for training or tuning (an alert counts as correct when it overlaps an annotated event within 5 s):

| Test set | Events caught | False alerts per hour |
|---|---|---|
| UCF-Crime test (290 videos) | 59.6% | 0.50 |
| MSAD multi-scene CCTV (240 videos) | 69.2% | 3.63 |
| CamNuvem robbery CCTV (49 videos) | 100% | 0.00 |

Adding the verifier raised events caught on UCF-Crime from 28.8% to 59.6% at the same false-alert budget. One 16 GB GPU served 15 to 20 cameras, and events opened about 2 seconds after onset, plus 1 to 2 seconds for verification.

---

## 4. Target Users

AnomaEYE is intended for organisations that already have CCTV but still depend on people watching screens.

**Primary users** will be security guards, control-room operators and facility managers, who receive and act on alerts. **Administrators** will manage cameras, staff, alert settings and the audit trail.

| Segment | Examples | Why they need it |
|---|---|---|
| Private premises (first users) | Housing societies, university campuses, hospitals, bank branches, factories and warehouses in Lahore | One guard per wall of screens; incidents are found after the fact |
| Commercial and public spaces | Offices, shopping malls, parking facilities, public transport areas | Many cameras, high footfall, liability for late response |
| Public safety | Punjab Safe Cities Authority control rooms, smart-city and public-safety departments | Thousands of feeds; AnomaEYE would rank which feeds an operator should look at now |

---

## 5. Key Features / Deliverables

1. **Live camera integration.** Support for existing CCTV and IP cameras over RTSP, through a small on-site agent that needs no inbound ports or static address.
2. **AI-based video anomaly detection.** A scene gate that analyses video over time, following the STAGE approach, rather than detecting objects in single frames.
3. **Temporal anomaly localisation.** Per-second anomaly scores that show when suspicious activity starts and ends.
4. **Vision-language verification.** A second model that confirms or vetoes each candidate and writes a one-sentence description.
5. **Incident generation with evidence.** Each incident will record the camera, detection time, anomaly score, crime group and class, video interval, keyframe, evidence clip, description and alert status.
6. **Multi-channel alerts.** Push, SMS and email to the staff who enable them.
7. **Physical alert system.** ESP32, relay and 12V siren for selected high-risk events, with safety controls against unnecessary activation.
8. **Mobile application.** Flutter app for live cameras, alerts with clips, and acknowledge / escalate / resolve actions.
9. **Web dashboard.** Live incident feed, camera and staff management, alert settings and the audit log.
10. **Backend and API layer.** FastAPI backend connecting the AI service, apps, database and alerting, with multi-tenant, role-based access.
11. **Operator feedback loop.** Confirm, dismiss and reclassify actions stored as labelled examples to recalibrate and retrain on each site's own cameras.
12. **Existing CCTV compatibility.** A software layer on top of compatible cameras, not a replacement.

**Deliverables for the finale:** the running system (AI service, backend, web dashboard, mobile app, siren), a live demonstration, an evaluation report with held-out results and measured latency, and this documentation.

---

## 6. Implementation Plan & Timeline

### Development phases

| Phase | Work | Estimated duration |
|---|---|---|
| 1. System design and setup | Finalise the architecture; configure camera streaming through the on-site agent and media server; define the backend, app, AI and IoT interfaces | 1–2 days |
| 2. AI detection pipeline | Integrate the STAGE-based scene gate; per-second scoring on live and recorded streams; event engine with thresholds and hysteresis; test on normal and anomalous scenarios | 2–3 days |
| 3. Verification and evidence | Integrate the vision-language verifier; attach keyframe, clip and description; reliable delivery to the backend | 1–2 days |
| 4. Backend and incident workflow | FastAPI backend; APIs for cameras, incidents and alerts; evidence storage; audit log; push, SMS and email fan-out | 1–2 days |
| 5. Web and mobile interfaces | Web dashboard and Flutter app; incident views; live updates; acknowledge / escalate / resolve; feedback buttons | 1–2 days |
| 6. Physical alert integration | Connect the ESP32, relay and siren through an on-site agent; add safety controls | 1 day |
| 7. Testing, evaluation and demonstration | End-to-end tests; held-out evaluation; latency measurement; threshold tuning; final demo and presentation | 1–2 days |

### Key dates

| Date | Milestone |
|---|---|
| 12 October 2026 | Proposal and repository submitted |
| 16 October 2026 | Shortlist announcement |
| 17–23 October 2026 | Mentorship, evaluation, rehearsal, offline fallback and backup demo video |
| 24 October 2026 | Live demonstration at the finale, FAST-NUCES Lahore |

### Team responsibilities

| Member | Responsibilities |
|---|---|
| M. Talha Farooqi | Scene gate and verifier models, STAGE research, training and evaluation, GPU inference service |
| Muhammad Husnain Aslam | Backend API, multi-tenancy, media pipeline, alert fan-out, siren integration, deployment |
| Chaudhary Shoaib Hamza | Flutter mobile app: live view, alerts, incident actions, onboarding; finale demo build |

### After the championship: 90-day pilot

- **Month 1:** deploy on 8 to 16 cameras at one campus or housing society in shadow mode (alerts logged, not sent) to measure alerts per camera per day; then switch on alerts to two guards and one supervisor.
- **Month 2:** compare alerts against the site's incident log; recalibrate on the site's own footage and operator feedback.
- **Month 3:** enable the siren and supervisor-level escalation; publish the results.

---

## 7. Resources or Requirements

### Hardware

| Item | Purpose |
|---|---|
| CCTV / IP cameras (RTSP) | Video source; existing cameras can be used |
| Always-on computer on the site network | Runs the small camera agent |
| GPU server | AI inference; a 16 GB GPU (for example RTX 5080) for 15–20 cameras, or an 8 GB GPU for about 6 |
| Small cloud VM (Google Cloud) | Backend API and media server |
| ESP32, relay module, 12V siren | On-site physical alarm |
| Internet connection | Outbound only; no inbound ports or static IP at the site |

### Software and technologies

- **AI:** Python, PyTorch, FFmpeg
- **Streaming:** MediaMTX, RTSP, SRT / RTMP, WebRTC with HLS fallback
- **Backend:** FastAPI, Firestore, Firebase Authentication, Cloud Storage, Redis (Upstash)
- **Alerts:** Firebase Cloud Messaging, Twilio (SMS), Resend (email)
- **Apps:** Flutter (Android, iOS); React web dashboard on Vercel
- **Operations:** GitHub, Google Cloud Build, Linux GPU server for inference

### AI / ML components

- X3D-M for spatio-temporal visual features
- InternVideo2-CLIP for semantic video features
- RAFT-small for motion information (STAGE research experiments)
- Causal temporal model with Multiple Instance Learning, ensemble and calibration
- Qwen3-VL vision-language model as the verifier
- Optional weapon detector (gun, knife)

### Datasets

| Dataset | Planned use | Link |
|---|---|---|
| UCF-Crime | Training and held-out testing; 1,900 surveillance videos, 13 anomaly classes | https://www.crcv.ucf.edu/projects/real-world/ |
| MSAD | Multi-scene CCTV anomalies; false-alarm behaviour on unseen scenes | https://msad-dataset.github.io/ |
| XD-Violence | Violence footage for training, calibration and STAGE cross-dataset evaluation | — |
| ShanghaiTech | STAGE cross-dataset evaluation | — |
| CamNuvem, RWF-2000 | Real CCTV robbery and violence footage for testing and calibration | https://github.com/davidhiroshi/CamNuvem |
| SOHAS, YouTube-GDD | Optional weapon detector (gun, knife, and look-alike distractors) | — |
| Operator feedback | Labelled confirm / dismiss / reclassify examples from each site's own cameras | internal |

### Cost

One GPU can serve 15 to 20 cameras, the backend can run on a single small cloud VM, and the apps run on existing phones, so the per-camera monthly cost is expected to be a fraction of one guard's salary.

---

## 8. Potential Challenges or Risks

| Risk | Why it matters | Planned mitigation |
|---|---|---|
| **False positives** | Running, sudden movement or crowds can look abnormal; too many false alarms and operators stop trusting the system | Vision-language verifier on every candidate; calibrated thresholds with hysteresis; human review before escalation |
| **Domain shift** | A model trained on one dataset can lose accuracy on new cameras, angles, lighting and scenes | STAGE research on cross-dataset generalisation; calibration on each site's normal footage before alerts go live; retraining from operator feedback |
| **Real-time performance** | Video models are compute-heavy | Frozen encoders; one shared GPU batch for all cameras; the expensive verifier runs only on candidates |
| **Scaling to many cameras** | Real deployments need many streams at once | Batched inference, additional GPU workers, one tenant per organisation |
| **Privacy and responsible AI** | Surveillance must not become identification or misuse | No face recognition; 5-minute rolling buffer and retention only around confirmed events; private evidence with short-lived links; role-based access and tenant isolation; audit log; human review before serious actions |
| **Network reliability** | Live systems depend on connectivity | Outbound-only camera agent and siren polling; retries through an outbox; SMS as a fallback channel; edge processing as a future step |
| **Alert fatigue** | Unnecessary alerts get ignored | Verification, per-camera adaptive baseline, severity groups, staff-selected channels, shadow mode before go-live |
| **Subtle or short events** | Brief, small person-level events (a few seconds, people far from the camera) are hard to catch, and naming the exact crime is harder than detecting that something is wrong | Headline the crime group in alerts and show the specific class as a tag; improve with site feedback and higher-resolution crops |

---

## 9. Additional Details (Optional)

### Research foundation: STAGE

<table>
  <tr>
    <td width="96" align="center" valign="middle">
      <img src="https://raw.githubusercontent.com/M-Talha-Farooqi/AIEF_Hackathon_2026/main/assets/icons/elsevier.svg" width="80" alt="Elsevier">
    </td>
    <td valign="middle">
      <b>STAGE: Splice-Supervised Temporal Anchoring for Generalization and Explainability in Video Anomaly Detection</b><br>
      <sub>Under review at <b>Image and Vision Computing (IVC)</b>, Elsevier</sub>
    </td>
  </tr>
</table>

STAGE studies weakly supervised video anomaly detection and the problem every real deployment faces: a model trained on one set of cameras has to work on new scenes, angles, lighting and video quality.

- **Latent Temporal Splicing:** anomaly-related latent temporal segments are spliced in during training to create additional learning signals and reduce dependence on source-specific video context.
- **Semantic guidance for donor selection:** semantic video features decide which segments are spliced together.
- **Appearance, motion and semantic streams:** X3D-M, RAFT-small and InternVideo2-CLIP.
- **Multiple Instance Learning** with temporal modelling for anomaly scoring and temporal localisation.
- **Cross-dataset evaluation** on UCF-Crime, MSAD, XD-Violence and ShanghaiTech.

**From paper to product.** AnomaEYE will keep the STAGE approach and adapt it for real-time, multi-camera serving:

| | STAGE paper | AnomaEYE system |
|---|---|---|
| Feature streams | X3D-M + RAFT-small + InternVideo2-CLIP | X3D-M + InternVideo2-CLIP (two frozen video encoders) |
| Temporal model | MIL with temporal modelling on whole videos | Causal temporal model scoring every second over a live rolling window |
| Output | Anomaly scores and temporal localisation | Per-second anomaly probability, a coarse group and a fine crime class |
| Robustness | Single model, cross-dataset evaluation | Ensemble, calibration, hysteresis and a per-camera adaptive baseline |
| Decision | Benchmark scoring | Candidates confirmed or vetoed by a vision-language verifier before any alert |

### What makes AnomaEYE different

- **Detection to response, not detection alone.** Most academic and student systems stop at an anomaly score; AnomaEYE will deliver the verification, evidence, alerting, apps and audit trail needed to act on it.
- **Research-backed and verified.** A detection backbone from research under peer review, plus a second, independent vision-language judgement with a written explanation on every alert.
- **Learns from its operators.** Every confirm, dismiss and reclassify will become a labelled example from the customer's own cameras.
- **Built for Pakistani deployment.** Cheap always-on scoring with expensive verification only on candidates keeps hardware to one GPU; sites need no inbound ports or static addresses; and staff work from a phone app with SMS as a fallback.

### Product vision

AnomaEYE is not meant to replace existing CCTV; it adds an intelligence layer on top of it.

```text
Existing CCTV infrastructure  +  AnomaEYE AI layer  =  Intelligent real-time monitoring
```

Future improvements beyond the pilot: edge deployment so basic detection continues during internet outages, distributed inference across GPU workers, centralised monitoring of many sites for security companies, further privacy-preserving processing, and integration with emergency-response workflows.

### Team

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

- **Institution:** University of Management and Technology (UMT)
- **Degree programme:** BS Software Engineering
- **Graduation:** Graduated (July 2026)
- **Project supervisor:** Dr. Syed Farooq Ali

### Links

- **Project landing page:** [www.anomaeye.com](https://www.anomaeye.com)
- **Pitch deck:** [View on Canva](https://canva.link/6gh1y52uxf0r5n4)
