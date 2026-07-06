<div align="center">

# 🚨 SurgeAlert

### *Turn a citizen's phone report into an instant, location-aware dispatch record.*

**Automatically identifying the nearest police station and hospital for every incident — in real time.**

[![Live Demo](https://img.shields.io/badge/🌐_Live_Demo-surge--alert.vercel.app-b70011?style=for-the-badge)](https://surge-alert.vercel.app)
[![API Docs](https://img.shields.io/badge/📡_API_Docs-Swagger_UI-0d9488?style=for-the-badge)](https://surgealert-api-171959923793.asia-south1.run.app/docs)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](#license)

---

<img src="https://img.shields.io/badge/React_19-61DAFB?style=flat-square&logo=react&logoColor=black" />
<img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white" />
<img src="https://img.shields.io/badge/TailwindCSS_v4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white" />
<img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" />
<img src="https://img.shields.io/badge/Firebase-FFCA28?style=flat-square&logo=firebase&logoColor=black" />
<img src="https://img.shields.io/badge/Google_Cloud_Run-4285F4?style=flat-square&logo=google-cloud&logoColor=white" />

</div>

---

## 📌 The Problem

When an accident or traffic incident occurs, the person best positioned to report it — the bystander or victim on scene — has **no fast, structured way to alert the right responders**.

- 📞 Calling emergency lines is slow and requires the caller to describe their location precisely.
- 🗺️ The caller must figure out *which* police station or hospital is actually closest.
- ⏱️ This delay compounds in dense urban traffic, where **minutes matter** and misrouted dispatch can worsen outcomes.

## 💡 The SurgeAlert Solution

SurgeAlert **removes that burden entirely**. A citizen submits a report in seconds — the platform captures precise GPS coordinates automatically, and the backend takes over:

1. **Queries live geospatial data** (OpenStreetMap) to pinpoint the nearest relevant emergency services.
2. **Produces a structured, machine-readable dispatch record** for each responder — capturing name, address, coordinates, and status.
3. For **accidents** → both police and hospital are identified. For **traffic incidents** → police only.

> The result: **"citizen notices incident" → "responders have an actionable, location-verified alert"** compressed into a single automated step.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔘 **One-Tap Reporting** | Report a traffic jam or accident with type, description, GPS coordinates, and address in seconds. |
| 📸 **Photo Evidence Upload** | Attach images directly to a report, stored securely in Firebase Cloud Storage with public retrieval URLs. |
| 📍 **Auto Nearest-Responder Resolution** | Every report triggers a live geospatial lookup to find the closest police station and/or hospital. |
| 📋 **Structured Dispatch Records** | Each identified responder is logged as a notification document (name, address, coordinates, status) ready for downstream alerting. |
| 📡 **Real-Time Report Feed** | View all incidents newest-first, or filter by type, for live situational awareness. |
| 🛡️ **Resilient by Design** | If geospatial lookup fails, the report is still saved and the failure is logged — no report is ever lost. |
| ☁️ **Cloud-Native & Serverless** | FastAPI + Firebase + Cloud Run, scaling automatically with zero idle cost. |

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│                          CITIZEN (Browser)                          │
│                                                                      │
│   ┌─────────────┐    ┌──────────────┐    ┌────────────────────┐     │
│   │  Landing     │───▶│  Report Flow │───▶│  Submit + Confirm  │     │
│   │  Page        │    │  (3 Steps)   │    │  (GPS + Photos)    │     │
│   └─────────────┘    └──────────────┘    └────────┬───────────┘     │
│                                                    │                 │
│   ┌─────────────────────────────────────────────── │ ──────────┐    │
│   │              Dashboard (Command Center)        │           │    │
│   │              Live Feed + Map Preview            │           │    │
│   └─────────────────────────────────────────────── │ ──────────┘    │
└──────────────────────────────────────────────────── │ ───────────────┘
                                                      │
                                              POST /reports
                                         (multipart/form-data)
                                                      │
                                                      ▼
┌──────────────────────────────────────────────────────────────────────┐
│                        BACKEND (FastAPI)                             │
│                   Google Cloud Run (asia-south1)                     │
│                                                                      │
│   ┌──────────────┐    ┌──────────────────┐    ┌─────────────────┐   │
│   │  Validate    │───▶│  Store Report in │───▶│  Upload Images  │   │
│   │  Request     │    │  Firestore       │    │  to Cloud Store │   │
│   └──────────────┘    └──────────────────┘    └────────┬────────┘   │
│                                                         │            │
│                                                         ▼            │
│                              ┌───────────────────────────────────┐   │
│                              │   Geospatial Lookup (OSM)         │   │
│                              │   → Nearest Police Station        │   │
│                              │   → Nearest Hospital (if ACCIDENT)│   │
│                              └──────────────┬────────────────────┘   │
│                                              │                       │
│                                              ▼                       │
│                              ┌───────────────────────────────────┐   │
│                              │   Create Dispatch Records         │   │
│                              │   (Notification Documents)        │   │
│                              └───────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| **React 19** | Component-based UI framework |
| **Vite** | Lightning-fast dev server & build tool |
| **TailwindCSS v4** | Utility-first CSS with custom M3 design tokens |
| **React Router DOM v7** | Client-side SPA routing |
| **Google Material Symbols** | Icon system (Outlined & Filled) |
| **Google Fonts (Inter)** | Typography |

### Backend
| Technology | Purpose |
|---|---|
| **FastAPI** | High-performance Python API framework |
| **Firebase Firestore** | NoSQL document database for reports & dispatch records |
| **Firebase Cloud Storage** | Secure image/video evidence storage |
| **OpenStreetMap (Overpass API)** | Live geospatial queries for nearest responders |
| **Google Cloud Run** | Serverless container deployment (asia-south1) |

---

## 📁 Project Structure

```
SurgeAlert-Rep/
├── Frontend/
│   ├── public/                  # Static assets (favicon, icons)
│   ├── src/
│   │   ├── components/          # Shared UI components
│   │   │   ├── TopAppBar.jsx    # Responsive top navigation
│   │   │   ├── BottomNavBar.jsx # Mobile-only bottom nav
│   │   │   ├── SideNavBar.jsx   # Desktop dashboard sidebar
│   │   │   └── Footer.jsx      # Global footer
│   │   ├── pages/               # Route-level page components
│   │   │   ├── Landing.jsx      # Hero + nearby reports feed
│   │   │   ├── ReportFlow.jsx   # 3-step emergency questionnaire
│   │   │   ├── ReportDetails.jsx# Mobile report form
│   │   │   ├── IncidentForm.jsx # Desktop report form
│   │   │   ├── Confirmation.jsx # Success + dispatch status
│   │   │   └── Dashboard.jsx    # Command center view
│   │   ├── services/
│   │   │   └── api.js           # Centralized API service layer
│   │   ├── utils/
│   │   │   └── time.js          # Relative timestamp utility
│   │   ├── App.jsx              # Root component + routing
│   │   ├── main.jsx             # React entry point
│   │   └── index.css            # Design system + Tailwind config
│   ├── .env                     # Environment variables
│   ├── vercel.json              # Vercel proxy rewrites
│   ├── vite.config.js           # Vite + Tailwind + proxy config
│   └── package.json
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- **Node.js** ≥ 18
- **npm** ≥ 9

### Installation

```bash
# Clone the repository
git clone https://github.com/ShailjaSrivastava/SurgeAlert-Rep.git
cd SurgeAlert-Rep/Frontend

# Install dependencies
npm install

# Start the development server
npm run dev
```

The app will be running at `http://localhost:5173`.

### Environment Variables

Create a `.env` file inside the `Frontend/` directory:

```env
# Points to the Vercel proxy which forwards to the backend
VITE_API_URL=/api
```

For local development hitting the backend directly:
```env
VITE_API_URL=https://surgealert-api-171959923793.asia-south1.run.app
```

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/reports` | Submit a new incident report (multipart/form-data) |
| `GET` | `/reports` | List all reports (filter by `type`, `latitude`, `longitude`) |
| `GET` | `/reports/{report_id}` | Get a single report by ID |
| `GET` | `/health` | Basic liveness check |
| `GET` | `/health/firestore` | Firestore connectivity check |

> 📡 Full interactive API documentation: [Swagger UI](https://surgealert-api-171959923793.asia-south1.run.app/docs)

### Report Submission Payload

```
user_id:     string   (required) — ID of the reporting user
type:        string   (required) — "TRAFFIC" or "ACCIDENT"
description: string   (required) — Description of the incident
latitude:    number   (required) — GPS latitude (-90 to 90)
longitude:   number   (required) — GPS longitude (-180 to 180)
address:     string   (required) — Human-readable address
images:      File[]   (optional) — Photo/video evidence
```

---

## 🤖 AI & Automation Innovation

SurgeAlert's core innovation is **automated geospatial reasoning applied to emergency response**:

Instead of a caller manually identifying and contacting the nearest station or hospital, the system resolves this **in real time** from the incident's coordinates and generates a ready-to-act dispatch record automatically.

### 🔮 Future Roadmap
- **Vision-Language Model Integration** — Use a VLM to read uploaded incident photos and auto-generate a scene summary (severity, hazards, people involved) for responders *before they arrive*.
- **Real-time Push Notifications** — Alert nearby registered responders via WebSockets.
- **Heatmap Analytics** — Aggregate geotagged data into live accident/congestion hotspot maps for city planners.

---

## 📊 Expected Impact

| Metric | Impact |
|---|---|
| ⏱️ **Response Time** | Cuts the gap between incident and dispatch by automating responder identification |
| 📈 **Reporting Volume** | Lowers the barrier — no phone call, no need to know which station is nearest |
| 🗺️ **Urban Planning** | Aggregated data provides live, geotagged feeds of accident & congestion hotspots |
| 🏥 **Dispatch Accuracy** | Machine-resolved nearest responder vs. human guesswork |

---

## 👥 Team

Built with ❤️ for public safety.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<div align="center">

**SurgeAlert** — *Because every second counts.* 🚨

</div>
