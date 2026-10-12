<p align="center">
  <img src="assets/banner.svg" alt="AnomaEYE: real-time CCTV crime detection with verified, multi-agent incident response" width="100%">
</p>

<p align="center">
  <a href="https://www.anomaeye.com"><img src="https://img.shields.io/badge/Live_Product-anomaeye.com-22D3EE?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Visit anomaeye.com"></a>
  &nbsp;
  <a href="https://canva.link/6gh1y52uxf0r5n4"><img src="https://img.shields.io/badge/Pitch_Deck-Canva-8B5CF6?style=for-the-badge&logo=canva&logoColor=white" alt="Open the pitch deck"></a>
  &nbsp;
  <a href="docs/AnomaEYE_Proposal.md"><img src="https://img.shields.io/badge/Proposal-Read-10B981?style=for-the-badge&logo=readdotcv&logoColor=white" alt="Read the proposal"></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/AIEF-4th_International_AI_Championship-1E1B4B?style=flat-square" alt="AIEF 4th International AI Championship">
  <img src="https://img.shields.io/badge/Category-AI_in_Urban_Tech-0E7490?style=flat-square" alt="Category: AI in Urban Tech">
  <img src="https://img.shields.io/badge/Finale-24_Oct_2026_%7C_FAST--NUCES_Lahore-312E81?style=flat-square" alt="Finale: 24 October 2026, FAST-NUCES Lahore">
</p>

<h3 align="center">Thousands of cameras. A handful of eyes.<br>AnomaEYE watches every feed, double-checks every alarm, and gets the right person moving in seconds.</h3>

<br>

## ✦ Overview

**AnomaEYE** turns passive CCTV recording into active, verified security. It watches live camera streams, flags crimes and anomalous events as they happen, has an AI verifier agent take a second look before anyone is disturbed, writes the incident report automatically, alerts guards on their phones, keeps supervisors in the loop on a live dashboard, and for severe, confirmed incidents escalates to the nearest police station or emergency service, with a human approving every external call.

> **See the full flow live at [www.anomaeye.com](https://www.anomaeye.com)**

<br>

## 📊 Why it matters

<table>
  <tr>
    <td align="center" width="33%">
      <h1>8,000+</h1>
      <sub>Safe City CCTV cameras in Lahore alone, watched from one control room</sub>
    </td>
    <td align="center" width="33%">
      <h1>171,784</h1>
      <sub>crimes reported across Punjab in 2025, including 25,010 robberies</sub>
    </td>
    <td align="center" width="33%">
      <h1>1 : many</h1>
      <sub>one guard per wall of screens on most private premises; footage is reviewed only after the fact</sub>
    </td>
  </tr>
</table>

<sub>Sources: <a href="https://arynews.tv/punjab-cm-inaugurates-lahore-safe-city-project">ARY News</a>, <a href="https://www.thenews.pk/print/1402651-punjab-records-32pc-drop-in-crime">The News</a>.</sub>

<br>

## ⚡ What AnomaEYE does

<table>
  <tr>
    <td width="33%" valign="top">
      <h3>🎥 Real-time detection</h3>
      Weakly supervised deep model scores every few seconds of every stream for fights, robbery, assault, vandalism and other anomalies.
    </td>
    <td width="33%" valign="top">
      <h3>🧠 Verifier agent</h3>
      A vision-language agent double-checks each flagged clip and explains what it sees, cutting the false alarms that make operators stop trusting analytics.
    </td>
    <td width="33%" valign="top">
      <h3>📝 Instant incident reports</h3>
      Summary, severity, camera, time and key frames, written automatically the moment an incident is confirmed.
    </td>
  </tr>
  <tr>
    <td width="33%" valign="top">
      <h3>📱 Mobile alerts</h3>
      Flutter app for security staff: push alert, clip playback, confirm or dismiss, one-tap call.
    </td>
    <td width="33%" valign="top">
      <h3>🖥️ Supervisor dashboard</h3>
      Next.js web dashboard with live incidents, timeline, analytics and site escalation policy.
    </td>
    <td width="33%" valign="top">
      <h3>🚓 Verified escalation</h3>
      Severe, confirmed incidents reach the nearest police station or Rescue 15, only after human approval, with a full audit trail.
    </td>
  </tr>
</table>

<br>

## 🔄 How it works

| Step | What happens |
|:---:|---|
| **1** | Live CCTV / RTSP streams are sampled into short video windows |
| **2** | A ResNet-50 encoder and a multiple-instance-learning scorer rate each segment from 0 to 1 |
| **3** | Windows above the site threshold go to the **Verifier Agent** for a second, explainable look |
| **4** | Confirmed events become an incident report from the **Incident Report Agent** |
| **5** | The **Escalation Agent** applies site policy: guard, then supervisor, then police or emergency services |
| **6** | Every verdict and every human action is written to an append-only audit log |

<br>

## 🏗️ System architecture

<p align="center">
  <img src="assets/architecture/system-architecture.png" alt="AnomaEYE system architecture" width="100%">
</p>

<br>

## 🧰 Built with

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" alt="PyTorch">
  <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" alt="OpenCV">
  <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white" alt="Flutter">
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white" alt="Next.js">
</p>

<br>

## 🛡️ Responsible by design

- **No face recognition** and no identity matching.
- **Human in the loop:** nothing leaves the site without a person confirming it.
- **Minimal retention:** clips are kept only for flagged incidents, for a configurable window.
- **Accountable:** every model output and human decision is logged and attributable.

<br>

## 👥 Team

<table align="center">
  <tr>
    <td align="center" width="260" valign="top">
      <img src="assets/team/talha.png" width="150" alt="M. Talha Farooqi"><br><br>
      <b>M. Talha Farooqi</b><br>
      <sub>AI Engineer</sub><br><br>
      <a href="https://github.com/M-Talha-Farooqi"><img src="assets/icons/github.svg" width="30" alt="GitHub"></a>&nbsp;
      <a href="https://www.linkedin.com/in/m-talha-farooqi"><img src="assets/icons/linkedin.svg" width="30" alt="LinkedIn"></a>&nbsp;
      <a href="mailto:the.mtalhafarooqi@gmail.com"><img src="assets/icons/email.svg" width="30" alt="Email"></a>
    </td>
    <td align="center" width="260" valign="top">
      <img src="assets/team/husnain.png" width="150" alt="Muhammad Husnain Aslam"><br><br>
      <b>Muhammad Husnain Aslam</b><br>
      <sub>Backend Engineer</sub><br><br>
      <a href="https://github.com/heyhusn"><img src="assets/icons/github.svg" width="30" alt="GitHub"></a>&nbsp;
      <a href="https://www.linkedin.com/in/husnain-aslam-0a959a1a7"><img src="assets/icons/linkedin.svg" width="30" alt="LinkedIn"></a>&nbsp;
      <a href="mailto:mhusnainaslam2003@gmail.com"><img src="assets/icons/email.svg" width="30" alt="Email"></a>
    </td>
    <td align="center" width="260" valign="top">
      <img src="assets/team/shoaib.png" width="150" alt="Chaudhary Shoaib Hamza"><br><br>
      <b>Chaudhary Shoaib Hamza</b><br>
      <sub>Flutter Developer · Founder, Big Brains</sub><br><br>
      <a href="https://github.com/theshoaibhamza"><img src="assets/icons/github.svg" width="30" alt="GitHub"></a>&nbsp;
      <a href="https://www.linkedin.com/in/shoaibhamza"><img src="assets/icons/linkedin.svg" width="30" alt="LinkedIn"></a>&nbsp;
      <a href="mailto:chaudharyshoaibhamza@gmail.com"><img src="assets/icons/email.svg" width="30" alt="Email"></a>
    </td>
  </tr>
</table>

<br>

## 🏆 AIEF 4th International AI Championship

AnomaEYE is competing in the **[AI Showdown, 4th International AI Championship](https://www.aiedu.org.pk/4th-international-ai-championship/)** organised by the Artificial Intelligence Education Foundation, in the **AI in Urban Tech** category. Grand finale: **24 October 2026, FAST-NUCES Lahore**.

<br>

<p align="center">
  <a href="https://www.anomaeye.com"><img src="assets/icons/website.svg" width="34" alt="Website"></a>&nbsp;&nbsp;
  <a href="https://canva.link/6gh1y52uxf0r5n4"><img src="https://img.shields.io/badge/Pitch_Deck-8B5CF6?style=flat-square&logo=canva&logoColor=white" height="34" alt="Pitch deck"></a>
  <br><br>
  <sub>Built in Lahore, Pakistan 🇵🇰 · © 2026 Team AnomaEYE</sub>
</p>
