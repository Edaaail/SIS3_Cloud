# CFO Bot Implementation Plan

Implementing CFO Bot, a purely client-side web application for calculating and comparing cloud computing costs across AWS, Google Cloud, and Azure. The bot will operate via a sequential chat interface and visualize the cost comparisons using Chart.js.

## User Review Required
> [!IMPORTANT]
> The application is planned to rely exclusively on Vanilla JavaScript, HTML, and CSS (with Chart.js loaded via CDN), strictly keeping it as a client-side execution to eliminate any backend dependencies. Please review this to ensure it aligns with any potential longer-term deployment needs.

## Proposed Changes

### Frontend Assets
#### [NEW] [index.html](file:///c:/Users/Edil/Desktop/workspace/index.html)
- Semantic HTML5 structure.
- Responsive layout implementing two-column structure (Chat on left, Chart/History on right) defaulting to a stacked layout on mobile breakpoints.
- Essential container elements for Chatbot timeline, user input field, and action buttons. 
- Script import for Chart.js.

#### [NEW] [css/style.css](file:///c:/Users/Edil/Desktop/workspace/css/style.css)
- Custom Vanilla CSS establishing a modern, vibrant aesthetic (e.g. glassmorphism or clean dark mode).
- Flexbox/Grid usage to manage responsive alignments.
- Smooth transition states and hover micro-animations for elements like buttons or the chat sequence.
- Error styling for active validation constraints.

### Application Logic (Vanilla JS)
#### [NEW] [js/app.js](file:///c:/Users/Edil/Desktop/workspace/js/app.js)
- **State Management**: A controller logic to keep track of current question step, ongoing inputs, and storing calculated outputs into LocalStorage (capped at 10 items).
- **Cost Calculation Engine**: Compute model formulas for AWS, GCP, and Azure according to the v1.0 SSOT, factoring in vCPU, RAM, storage free tier limits, and estimated network transfer costs.
- **Bot Dialog Engine**: DOM manipulation logic for incrementally inserting chat bubbles on screen (Bot vs. User side) and waiting for inputs sequentially.
- **Validation Handlers**: Implements Min/Max limits, empty field checking, and numeric parsers mapping specific Russian error strings as detailed in the SSOT.
- **Data Visualization**: Logic to instantiate, push data to, and destroy Chart.js instances reflecting comparative provider costs.
- **Data Export**: Construct blob-based CSV exports of current calculations triggered via UI.

## Open Questions
- Is there a specific Chart.js version preferred (e.g., v3 vs v4)? Unless otherwise specified, we will default to the latest stable release via CDN.
- Do we have any preset preferences regarding color palettes (e.g., dark mode vs clean corporate white)?

## Verification Plan

### Automated Tests
*Codebase is configured as a purely client-side MVP without node/build layers per constraints — verification relies heavily on localized browser manual testing outlined below.*

### Manual Verification
- We will serve `index.html` locally using an equivalent localized static server.
- Cycle through correct values to verify accurate sequential chat flow.
- Feed invalid/out-of-bounds parameters resolving the specific error prompt ("От X до Y" or "Please enter a value").
- Validate final Chart.js rendering post-chat completion.
- Append over 10 inputs to ensure LocalStorage cleanly purges the oldest history record.
- Trigger and view the exported CSV values.
