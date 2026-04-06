# 🛡️ AnomalyGuard — Technology Stack Overview

> **Project:** Video Anomaly Detection System  
> **Architecture:** Full-stack JavaScript (React Frontend + Node.js Backend)

---

## 📐 Overall Architecture

```
┌──────────────────────────────────────────────────┐
│              React Frontend (Port 3000)           │
│  Dashboard │ Upload │ Charts │ Real-time Updates  │
└─────────────────────┬────────────────────────────┘
                      │ HTTP (REST API) + WebSocket
┌─────────────────────▼────────────────────────────┐
│             Node.js / Express Backend (Port 8000) │
│  Routes │ Controllers │ Middleware │ Services     │
│  ┌────────────────────────────────────────────┐   │
│  │  AI Service (OpenRouter / Azure AI)        │   │
│  │  Video Processor (FFmpeg + Sharp)          │   │
│  └────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────┘
```

---

## 🖥️ FRONTEND TECHNOLOGIES

### 1. React 18
- **Category:** UI Framework
- **Function:** The core frontend library used to build the entire user interface. React 18 introduces concurrent rendering, enabling smooth and non-blocking UI updates. All pages (Dashboard, Upload, Results) are React components that manage their own state and lifecycle.
- **Why used:** Component-based architecture makes it easy to build reusable UI pieces like stat cards, charts, and the upload form.

---

### 2. React Router DOM (v6)
- **Category:** Client-Side Routing
- **Function:** Manages navigation between pages without full page reloads. It maps URL paths like `/dashboard`, `/upload`, `/results` to specific React page components. Uses modern `BrowserRouter` and `Routes`/`Route` APIs.
- **Why used:** Provides a smooth Single Page Application (SPA) experience, allowing users to navigate between sections instantly.

---

### 3. Axios (v1.4)
- **Category:** HTTP Client
- **Function:** Handles all API communication between the frontend and the backend. Used in `services/api.js`, `videoService.js`, and `dashboardService.js` to send video uploads, fetch analysis results, and poll dashboard stats. Supports request/response interceptors, automatic JSON parsing, and error handling.
- **Why used:** More powerful than the native `fetch` API — provides cleaner syntax, automatic transforms, and better error management.

---

### 4. Recharts (v2.7)
- **Category:** Data Visualization / Charting Library
- **Function:** Renders interactive charts to display anomaly trends, detection statistics, and activity over time. Used in the `AnomalyChart` component. Supports line charts, bar charts, pie charts, and area charts — all responsive and animated.
- **Why used:** Built specifically for React, so charts are proper React components that integrate seamlessly with state and props.

---

### 5. React Dropzone (v14)
- **Category:** File Upload Utility
- **Function:** Powers the drag-and-drop video upload interface on the Upload page. Handles file selection, drag events, file type validation (only video files), and provides callbacks when a file is dropped. Abstracts complex browser file APIs into a simple React hook (`useDropzone`).
- **Why used:** Makes the video upload UI highly intuitive — users can drag videos directly from their file manager into the browser.

---

### 6. Lucide React (v0.263)
- **Category:** Icon Library
- **Function:** Provides a clean, consistent set of SVG icons used throughout the UI — navigation icons in the sidebar, action icons in header buttons, status indicators, and alert icons. Each icon is a proper React component, making it easy to style with CSS.
- **Why used:** Modern, lightweight alternative to Font Awesome. Icons are tree-shakable, meaning only icons actually used are included in the final bundle.

---

### 7. React Scripts (Create React App)
- **Category:** Build Toolchain
- **Function:** The underlying build system that powers the development server, hot-reloading, bundling (via Webpack), transpilation (via Babel), and production builds. Running `npm start` launches a dev server at `localhost:3000` with automatic reloading on file save.
- **Why used:** Zero-configuration setup — all complex Webpack/Babel configuration is pre-configured and hidden, allowing developers to focus on writing code.

---

### 8. CSS Custom Properties (Vanilla CSS)
- **Category:** Styling System
- **Function:** The project uses plain Vanilla CSS with custom properties (`:root` variables) for theming. Colors like `--primary-color`, `--success-color`, `--error-color` are defined once and reused everywhere. Each component has its own `.css` file, and `App.css` holds global styles.
- **Why used:** No CSS framework dependency, maximum flexibility, and easy global theming by changing variables in one place.

---

### 9. Web Vitals
- **Category:** Performance Monitoring
- **Function:** Measures and reports Core Web Vitals — key performance metrics like Largest Contentful Paint (LCP), First Input Delay (FID), and Cumulative Layout Shift (CLS). Reports are accessible via the `reportWebVitals()` call in `index.js`.
- **Why used:** Helps track real-world performance and ensure the app meets Google's performance standards.

---

### 10. Testing Library (Jest + React Testing Library)
- **Category:** Testing Framework
- **Function:** `@testing-library/react` provides utilities to render React components in a test environment and interact with them like a real user would (clicking, typing). `@testing-library/jest-dom` adds extra matchers for DOM assertions. `jest` is the underlying test runner.
- **Why used:** Encourages testing UI behavior from a user's perspective rather than testing implementation details.

---

## ⚙️ BACKEND TECHNOLOGIES

### 1. Node.js
- **Category:** Runtime Environment
- **Function:** The JavaScript runtime that powers the entire backend. Executes server-side JavaScript outside the browser. Uses ES Modules (`"type": "module"` in package.json), enabling `import`/`export` syntax in all backend files.
- **Why used:** Same language as the frontend (JavaScript), and its non-blocking I/O model is perfect for handling concurrent video processing requests and WebSocket connections.

---

### 2. Express.js (v4.18)
- **Category:** Web Framework
- **Function:** The HTTP framework that powers the REST API. Defines all routes (e.g., `POST /api/videos/upload`, `GET /api/dashboard/stats`), connects middleware, and handles request/response cycles. The `server.js` is the entry point that initializes Express and attaches all routers.
- **Why used:** Minimal, flexible, and the most widely used Node.js web framework. Provides a clean pattern for organizing routes, controllers, and middleware.

---

### 3. OpenAI SDK (v4.28)
- **Category:** AI Integration
- **Function:** The official OpenAI JavaScript SDK, used in `aiService.js` to communicate with AI language/vision models via the **OpenRouter** API (which is OpenAI-compatible). Sends extracted video frame data (as base64 images) to AI models for anomaly analysis and receives structured JSON responses describing detected events.
- **Why used:** OpenRouter provides access to multiple AI providers (GPT-4, Claude, Gemini, etc.) through a single OpenAI-compatible API endpoint.

---

### 4. Azure AI Inference SDK (`@azure-rest/ai-inference`)
- **Category:** AI Integration (Alternative Provider)
- **Function:** Microsoft's Azure AI SDK, providing an alternative way to call AI models hosted on Azure or GitHub Models. Used alongside the OpenAI SDK, giving the backend flexibility to route AI requests to either OpenRouter or Azure AI depending on configuration.
- **Why used:** Provides access to GitHub Models and Azure-hosted AI as a fallback or premium alternative.

---

### 5. FFmpeg (`ffmpeg-static`, `ffprobe-static`, `fluent-ffmpeg`)
- **Category:** Video Processing Engine
- **Function:** The most powerful open-source video processing tool. In this project it:
  - **Extracts frames** — pulls individual image frames from uploaded videos at set intervals
  - **Reads metadata** — duration, resolution, frame rate, codec information
  - **Generates thumbnails** — creates preview images shown in the UI
  - **Re-encodes video** — processes and annotates output videos with detection overlays
  - `ffmpeg-static` bundles the FFmpeg binary, `ffprobe-static` bundles the probe utility, and `fluent-ffmpeg` provides a clean JavaScript API to control both.
- **Why used:** Industry-standard tool for all video processing tasks. Static binaries mean no separate system installation is required.

---

### 6. Sharp (v0.32)
- **Category:** Image Processing
- **Function:** High-performance Node.js image processing library. Used in `videoProcessor.js` to resize, crop, compress, and encode extracted video frames before sending them to AI models. Converts frames to base64 PNG/JPEG format for API transmission.
- **Why used:** Extremely fast (built on the libvips engine), much faster than alternatives like Jimp. Ideal for processing large batches of video frames.

---

### 7. Multer (v1.4)
- **Category:** File Upload Middleware
- **Function:** Express middleware that handles `multipart/form-data` — the encoding type used for file uploads from the browser. Intercepts uploaded video files, saves them to the `uploads/` directory, and attaches file metadata to the request object for controllers to use.
- **Why used:** The standard, well-tested solution for handling file uploads in Express applications.

---

### 8. WebSocket (`ws` library, v8.14)
- **Category:** Real-Time Communication
- **Function:** Enables a persistent, bidirectional connection between the browser and server. As video analysis progresses on the backend, the server pushes real-time status updates (processing percentage, anomaly count, current frame being analyzed) directly to the frontend — with no client polling needed. The frontend connects via `ws://localhost:8000/ws/analysis/{jobId}`.
- **Why used:** Much more efficient than HTTP polling. Server pushes data only when there is something new, reducing latency and server load.

---

### 9. Helmet (v7.1)
- **Category:** Security Middleware
- **Function:** Automatically sets HTTP security response headers to protect against common web vulnerabilities:
  - `X-Frame-Options` — prevents clickjacking attacks
  - `X-XSS-Protection` — activates browser XSS filter
  - `Content-Security-Policy` — controls which resources can be loaded
  - `Strict-Transport-Security` — enforces HTTPS connections
- **Why used:** One-line security hardening — adds a strong security baseline to every HTTP response.

---

### 10. Express Rate Limit (v7.1)
- **Category:** Security / API Protection
- **Function:** Limits how many requests a single IP address can make within a given time window. Prevents abuse of the video upload endpoint and AI analysis endpoints. Returns `429 Too Many Requests` when limits are exceeded. Configured in `middleware/security.js`.
- **Why used:** Prevents DoS/brute-force attacks and controls AI API costs by limiting runaway request floods.

---

### 11. Joi (v17.11)
- **Category:** Request Validation
- **Function:** Schema-based request validation library. Used in `middleware/validation.js` to validate incoming API payloads and query parameters before they reach controllers. Defines rules for video upload parameters (file size, type, detection options) and returns descriptive error messages for invalid inputs.
- **Why used:** Far cleaner than manual `if/else` validation; schemas are reusable, composable, and self-documenting.

---

### 12. CORS (`cors` package, v2.8)
- **Category:** Security / HTTP Headers
- **Function:** Configures Cross-Origin Resource Sharing headers on the backend. Allows the React frontend (running at `localhost:3000`) to make API calls to the backend (running at `localhost:8000`) — which would otherwise be blocked by the browser's same-origin policy.
- **Why used:** Essential for any setup where frontend and backend run on different ports or domains.

---

### 13. dotenv (v16.3)
- **Category:** Configuration Management
- **Function:** Loads environment variables from `.env` files into `process.env` at startup. The backend `.env` contains sensitive values like the OpenRouter API key, Azure credentials, port numbers, and allowed CORS origins. These are never hardcoded in source files.
- **Why used:** Keeps secrets out of source code — critical for security and for supporting multiple environments (dev, staging, production).

---

### 14. UUID (v9)
- **Category:** Unique ID Generation
- **Function:** Generates universally unique identifiers (UUIDs v4) for each video analysis job. When a video is uploaded, a UUID is created as the `jobId`. This ID is used to track the job through processing, file storage, WebSocket channels, and all API responses.
- **Why used:** Guaranteed uniqueness without needing a database auto-increment — safe for concurrent, distributed environments.

---

### 15. Node-Cron (v3.0)
- **Category:** Task Scheduling
- **Function:** Runs scheduled background tasks at defined cron intervals. Used for cleanup jobs — automatically deleting old processed video files and expired analysis data after a retention period, preventing disk space from filling up. Works like Linux cron jobs but inside the Node.js process.
- **Why used:** Keeps the `uploads/` and `processed/` directories clean automatically without manual admin intervention.

---

### 16. Node-Fetch (v3.3)
- **Category:** HTTP Client (Server-Side)
- **Function:** Provides the standard browser `fetch()` API for use inside Node.js. Used in the backend to make direct HTTP calls to external APIs where the OpenAI SDK is not used — for example, raw API requests or downloading resources.
- **Why used:** Familiar fetch syntax, consistent with the browser API, making code portable and readable.

---

### 17. Nodemon (Dev Dependency)
- **Category:** Development Tool
- **Function:** Watches backend source files for changes and automatically restarts the Node.js server whenever a file is saved. `npm run dev` uses nodemon instead of raw `node`. Eliminates the need to manually restart the server during development.
- **Why used:** Massively speeds up the development feedback loop.

---

### 18. Jest (Dev Dependency)
- **Category:** Testing Framework (Backend)
- **Function:** JavaScript testing framework for writing and running backend unit and integration tests. `npm test` runs the test suite. Can test individual service functions, controllers, and middleware in isolation.
- **Why used:** Industry-standard Node.js testing framework with excellent async/await support.

---

## 📊 Technology Summary Table

| Layer | Technology | Role |
|---|---|---|
| Frontend UI | React 18 | Component rendering & state management |
| Frontend Routing | React Router v6 | SPA page navigation |
| Frontend API | Axios | HTTP REST API calls |
| Frontend Charts | Recharts | Interactive data visualization |
| Frontend Icons | Lucide React | SVG icon components |
| Frontend Upload | React Dropzone | Drag-and-drop file upload |
| Build Toolchain | Create React App | Webpack + Babel bundling |
| Styling | Vanilla CSS + CSS Variables | Theming and component styles |
| Backend Server | Express.js | REST API framework |
| Backend Runtime | Node.js (ES Modules) | Server-side JS execution |
| Real-time Comms | WebSocket (`ws`) | Live analysis progress updates |
| AI Analysis | OpenAI SDK | Vision model inference via OpenRouter |
| AI Analysis (Alt) | Azure AI Inference SDK | Azure / GitHub Models integration |
| Video Processing | FFmpeg (fluent-ffmpeg) | Frame extraction, thumbnails, encoding |
| Image Processing | Sharp | Frame resizing and base64 conversion |
| File Upload | Multer | Multipart form / file handling |
| Security Headers | Helmet | HTTP security hardening |
| Rate Limiting | Express Rate Limit | API abuse prevention |
| Validation | Joi | Request schema validation |
| Cross-Origin | CORS | Allow frontend-backend communication |
| Configuration | dotenv | Env variable management |
| Job Tracking | UUID | Unique analysis job IDs |
| Scheduling | Node-Cron | Automated file cleanup tasks |
| Dev Server Reload | Nodemon | Auto-restart on file change |
| Testing | Jest | Backend unit/integration tests |

---

## 🔑 Key Data Flow

### Video Upload → AI Analysis → Real-time Results
```
1. User drags video into React Dropzone
2. Axios POSTs multipart/form to Express /api/videos/upload
3. Multer saves video to /uploads directory
4. UUID job ID is generated
5. FFmpeg extracts frames at set intervals
6. Sharp resizes frames and converts to base64
7. OpenAI SDK sends frames to AI model (OpenRouter/Azure)
8. AI returns structured JSON: anomaly type, confidence, timestamp
9. WebSocket pushes real-time progress % to browser
10. React Dashboard renders Recharts graphs with results
```

---

*Built for enhanced security surveillance and real-time video anomaly detection.*
