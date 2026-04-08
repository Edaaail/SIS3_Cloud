# CFO Bot - System Specification v1.0 (SSOT)

## 1. Supported Cloud Components

| Component | Description | Unit |
|-----------|-------------|------|
| Compute | Virtual machines (vCPU + RAM) | per hour |
| Storage | Block storage (SSD) | per GB per month |
| Network | Outbound data transfer | estimated as 20% of compute cost |

## 2. Mathematical Cost Models

### 2.1 Compute Cost Formula

Monthly Compute = (VM_Count × vCPU_per_VM × CPU_Price + VM_Count × RAM_per_VM × RAM_Price) × 730


Where:
- `730` = hours per month (24 × 30.42)
- `CPU_Price` = $0.040 per vCPU per hour
- `RAM_Price` = $0.010 per GB per hour

### 2.2 Storage Cost Formula

Monthly Storage = max(0, Storage_GB - 30) × Storage_Price

Where:
- `Storage_Price` = $0.10 per GB per month
- `30` = free tier limit (first 30 GB free)

### 2.3 Network Transfer Cost Formula

Monthly Transfer = Monthly Compute × 0.2

*(Estimated as 20% of compute cost)*

### 2.4 Total Monthly Cost

Total = Monthly Compute + Monthly Storage + Monthly Transfer


## 3. Supported Cloud Providers

| Provider | CPU Price (per vCPU/hour) | RAM Price (per GB/hour) | Storage Price (per GB/month) |
|----------|---------------------------|-------------------------|------------------------------|
| AWS | $0.040 | $0.010 | $0.10 |
| Google Cloud | $0.045 | $0.009 | $0.08 |
| Azure | $0.042 | $0.011 | $0.09 |

## 4. Input Constraints (Validation Rules)

| Parameter | Min | Max | Error Message |
|-----------|-----|-----|---------------|
| Virtual Machines count | 1 | 50 | "От 1 до 50" |
| vCPU per VM | 1 | 16 | "От 1 до 16" |
| RAM per VM (GB) | 1 | 64 | "От 1 до 64" |
| Storage (GB) | 10 | 5000 | "От 10 до 5000" |

## 5. UI/UX Requirements

### 5.1 Chat Interface
- Sequential question flow (one question at a time)
- Bot messages appear on the left, user messages on the right
- Input field with number validation
- Send button and Enter key support
- Reset button to start over

### 5.2 Visualization
- Bar chart showing comparison of all 3 providers
- Chart updates automatically after calculation
- Chart library: Chart.js

### 5.3 Data Persistence
- LocalStorage for saving calculation history
- Maximum 10 saved calculations
- Click on history item to load previous calculation

### 5.4 Export
- CSV export with all parameters and costs

### 5.5 Responsive Design
- Desktop: two columns (chat + chart/history)
- Mobile: single column (stacked)

## 6. Architectural Constraints

| Constraint | Description |
|------------|-------------|
| Backend | No backend. Pure client-side JavaScript |
| API calls | No external API calls for pricing |
| Hosting | Static files only (Firebase Hosting or Netlify) |
| Dependencies | Chart.js only (external CDN) |

## 7. Edge Cases Handling

| Case | Expected Behavior |
|------|-------------------|
| Empty input | "Please enter a value" |
| Non-numeric input | "Please enter a number" |
| Value below min | "Enter X to Y" |
| Value above max | "Enter X to Y" |
| Save before calculation | "No data to save" |
| Storage exactly 30 GB | Storage cost = $0 (free tier) |