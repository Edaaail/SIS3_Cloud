# CFO Bot - Test Specifications

This document outlines comprehensive test cases for the CFO Bot logic based on the requirements designated in SSOT v1.0.

## 1. Input Validation Tests

| Test ID | Parameter | Input Value | Expected Form Validation Error |
|---------|-----------|-------------|--------------------------------|
| VAL-01  | VM count  | (Empty)     | "Please enter a value" |
| VAL-02  | VM count  | `abc`       | "Please enter a number" |
| VAL-03  | VM count  | `0`         | "От 1 до 50" |
| VAL-04  | VM count  | `51`        | "От 1 до 50" |
| VAL-05  | vCPU / VM | `0`         | "От 1 до 16" |
| VAL-06  | vCPU / VM | `17`        | "От 1 до 16" |
| VAL-07  | RAM / VM  | `0`         | "От 1 до 64" |
| VAL-08  | RAM / VM  | `65`        | "От 1 до 64" |
| VAL-09  | Storage   | `9`         | "От 10 до 5000" |
| VAL-10  | Storage   | `5001`      | "От 10 до 5000" |
| VAL-11  | Any       | Valid value | Application accepts input and asks the next question sequence |

## 2. Calculation Accuracy Tests

**Scenario 1: Standard Multi-Node Execution**
- **Inputs:** VM_Count=2 | vCPU=4 | RAM=8 | Storage=100
- **AWS Computations**:
  - Compute: `(2 * 4 * $0.040 + 2 * 8 * $0.010) * 730` = `$350.40`
  - Storage: `max(0, 100 - 30) * $0.10` = `$7.00`
  - Transfer: `$350.40 * 0.2` = `$70.08`
  - Total AWS: `$427.48`
- **GCP Computations**:
  - Compute: `(2 * 4 * $0.045 + 2 * 8 * $0.009) * 730` = `$367.92`
  - Storage: `70 * $0.08` = `$5.60`
  - Transfer: `$367.92 * 0.2` = `$73.58`
  - Total GCP: `$447.10`
- **Azure Computations**:
  - Compute: `(2 * 4 * $0.042 + 2 * 8 * $0.011) * 730` = `$373.76`
  - Storage: `70 * $0.09` = `$6.30`
  - Transfer: `$373.76 * 0.2` = `$74.75`
  - Total Azure: `$454.81`
- **Expected Result:** Chart natively reflects these exact sums.

**Scenario 2: Low Resource Boundary / Free Tier Limit**
- **Inputs:** VM_Count=1 | vCPU=1 | RAM=1 | Storage=30
- **Storage Cost Calculation:** `max(0, 30 - 30) = 0`
- **Expected Result:** Storage costs accurately amount to `$0` across providers.

## 3. UI/UX Functionality Tests

| Test ID | Action | Expected Application State |
|---------|--------|----------------------------|
| UI-01 | Send input using `Enter` key focus | Input is validated and appended as User Message |
| UI-02 | Click `Reset` button mid-sequence | Chat window flushes content, Bot initiates first question |
| UI-03 | Resize viewport explicitly < 768px | Layout transforms smoothly into a mobile-friendly vertical stack |
| UI-04 | Resize viewport explicitly > 768px | Interface displays a persistent two-column structure split |

## 4. Edge Cases & State Constraints Matrix

| Test ID | Trigger Event | Correct Response |
|---------|---------------|------------------|
| EDG-01 | Click "Save" prior to completing calculation steps | Present user error indicator: "No data to save" |
| EDG-02 | Fulfill >10 successive calculation requests | LocalStorage shifts indices, drops earliest calculations, and maintains exactly `10` |
| EDG-03 | Click on distinct list element in `History` panel | Chart rerenders reflecting earlier dataset; sequence restarts optionally reflecting these parameters |
| EDG-04 | Validate Download Blob (Click Export CSV) | Creates structured file downloading sequentially matched metadata (Cost, RAM, VMs mapped) |
