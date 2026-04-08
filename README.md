# CFO Bot — Cloud Cost Estimator

https://cfo-bot-project.web.app

A single-page web application that calculates and compares monthly cloud infrastructure costs across AWS, Google Cloud, and Azure in real time.

**Live Demo:** https://cfo-bot-project.web.app

---

## What it does

Users answer 4 sequential questions (VM count, vCPU per VM, RAM per VM, storage capacity). The bot instantly calculates monthly costs for all three providers and displays:

- Cost breakdown per provider
- Bar chart comparison
- LocalStorage history (last 10 calculations)
- CSV export functionality

**Supported cloud providers:**
- AWS
- Google Cloud
- Microsoft Azure

---

## Tech stack

| Layer | Technology |
|-------|------------|
| UI Framework | Vanilla HTML/CSS/JS |
| Styling | Custom CSS (Flexbox/Grid) |
| Charts | Chart.js CDN |
| Persistence | LocalStorage |
| Hosting | Firebase Hosting |

---

## Local setup

# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/cfo-bot-tsis3.git
cd cfo-bot-tsis3

# 2. Serve locally
npx serve .

# 3. Open browser at http://localhost:3000

Build & deploy
# Install Firebase CLI (if not installed)
npm install -g firebase-tools

# Login to Firebase
firebase login

# Deploy to Firebase Hosting
firebase deploy

Project structure
cfo-bot/
├── index.html              # Main application (all-in-one file)
├── firebase.json           # Firebase hosting configuration
├── .firebaserc             # Firebase project reference
├── .gitignore              # Git ignore rules
├── docs/
│   ├── SSOT_specification.md        # Phase 1: System specification
│   ├── implementation_plan.md       # Phase 2: Implementation plan
│   └── test_specifications.md       # Phase 2: Test specifications
└── .firebase/
    └── hosting.cache       # Firebase cache file

Features
✅ Sequential chat-based questions (4 steps)

✅ Real-time calculation on final step

✅ Bar chart comparison (Chart.js)

✅ LocalStorage history (last 10 calculations)

✅ CSV export with all parameters

✅ Reset conversation button

✅ Responsive design (desktop + mobile)

✅ Input validation per question (different ranges)

Cost model
For each provider, monthly cost = Compute + Storage
Compute = (VM_Count × vCPU × CPU_Price + VM_Count × RAM × RAM_Price) × 730 hours
Storage = Storage_GB × Storage_Price (first 30 GB free for AWS only)


### Pricing table

| Provider | CPU ($/vCPU/hour) | RAM ($/GB/hour) | Storage ($/GB/month) |
|----------|-------------------|-----------------|----------------------|
| AWS | 0.040 | 0.010 | 0.10 |
| Google Cloud | 0.045 | 0.009 | 0.08 |
| Azure | 0.042 | 0.011 | 0.09 |

### Validation rules

| Parameter | Min | Max |
|-----------|-----|-----|
| VM count | 1 | 50 |
| vCPU per VM | 1 | 16 |
| RAM per VM (GB) | 1 | 64 |
| Storage (GB) | 10 | 5000 |
