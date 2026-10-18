
# WorkMate AI — Enterprise Productivity & Knowledge Platform

> **"From Information to Action."**  
> WorkMate AI is an AI-powered enterprise productivity platform designed to synthesize organizational knowledge, automate workflows, and improve operational decision-making using Google Gemini, Firebase Authentication, Cloud Firestore, and Cloud Run as the primary Google Cloud architecture.

---

## 1. Platform Architecture & Phases

### Primary Google Cloud Architecture

- **Firebase Authentication:** Manages user sign-in and identity.
- **Cloud Firestore:** Stores user profiles, conversations, tasks, workflow metadata, and contextual application data.
- **Google Gemini on Vertex AI:** Powers generative AI, summarization, question answering, document understanding, and reasoning.
- **Cloud Run:** Hosts and scales containerized backend services and API endpoints.
- **Vertex AI Search:** Supports semantic enterprise search and knowledge retrieval when configured.
- **Google Document AI:** Supports structured document extraction when configured.
- **File Storage:** Uses the configured object-storage provider for uploaded documents and generated reports; Amazon S3 and local storage remain optional alternatives.

> **Implementation note:** Verify that Firebase Authentication, Firestore, Gemini on Vertex AI, and Cloud Run are configured and working before describing them as implemented features.

### Phase 1: Operational Core & Multi-Tenant Foundation

- **Application Shell & Enterprise Design System:** Responsive light-theme UI using professional SaaS design principles (#FAFBFC, #FFFFFF, #172B4D, #4F46E5, #0D9488).
- **Authentication:** Firebase Authentication is the primary identity provider in the Google Cloud architecture. Server-side authorization protects user and workspace data.
- **Application Data:** Cloud Firestore is the primary database for user profiles, conversations, tasks, workflows, and contextual records.
- **Gemini AI Service Layer:** Server-side Gemini on Vertex AI integration for AI-powered workflows, with timeout and error handling.
- **File Storage Abstraction:** Stores large file binaries separately from application database documents.
- **Optional MongoDB Atlas Compatibility:** The original MongoDB Atlas/Mongoose data layer remains an optional alternative for compatible deployments.

### Phase 2: Document Intelligence & Grounded RAG

- **Multi-Format Ingestion:** Supports PDF, DOCX, TXT, and CSV uploads up to 25MB, with text extraction and workspace isolation.
- **AI Document Onboarding Agent:** Uses Gemini to suggest document classification, department ownership, chunking strategy, sensitivity labels, and compliance rules.
- **Human-in-the-Loop Approval:** Requires user or administrator review before indexing; AI must not grant or escalate access permissions.
- **Grounded RAG Knowledge Chat:** Retrieves relevant information with source citations and page references, acknowledging when evidence is unavailable.
- **Hybrid Vector Retrieval:** Vertex AI Search is the preferred Google Cloud option when configured. MongoDB Atlas Vector Search and workspace-scoped similarity/keyword search remain optional alternatives.
- **Document Intelligence Hub:** Provides document search, status metrics, chunk inspection, and secure downloads.

### Phase 3: AI Data Quality & Preparation Agent

- **Spreadsheet Ingestion:** Supports CSV and XLSX/XLS files up to 25MB with schema inference and column type detection.
- **Data Quality Profiling:** Detects duplicates, missing values, inconsistent dates, category variations, and numeric outliers using IQR; calculates a Data Quality Health Score.
- **Natural Language Cleaning Agent:** Uses Gemini to translate cleaning instructions into structured, explainable operations.
- **Safe Transformations:** Uses a strict allowlist of operations such as deduplication, whitespace trimming, date standardization, renaming columns, filling missing values, filtering rows, and handling outliers.
- **Human Approval:** Allows users to review, approve, reject, or modify transformation steps.
- **Immutable Raw Storage:** Preserves original datasets and generates cleaned CSV/XLSX exports with before-and-after audit logs.

### Phase 4: Multimodal AI Processing

- **Multimodal Inputs:** Supports processing pipelines for text, documents, images, audio, and video where supported by the selected Gemini model and configured tools.
- **Document Extraction:** Google Document AI can provide structured extraction when enabled.
- **Audio/Video Processing:** Uses configured transcription and media-processing services; real-time transcription and speaker diarization should only be claimed when implemented and tested.

### Phase 5: Micro-App Studio

- **Task-Specific Tools:** Creates lightweight, task-focused tools for recurring business requirements.
- **Controlled Execution:** Uses validated inputs, defined permissions, and execution boundaries for generated tools.

### Phase 6: Meeting Intelligence & Workflow Execution

- **Meeting Summaries:** Converts meeting transcripts or notes into summaries, decisions, and action items.
- **Approved Workflows:** Supports user-reviewed tasks, follow-ups, and workflow automation.
- **External Integrations:** Email, calendar, Slack, and Microsoft Teams integrations remain optional until configured.

### Phase 7: Productivity Analytics & Project Risk Insights

- **Productivity Analytics:** Summarizes usage and workflow outcomes where reliable data is available.
- **Project Risk Insights:** Highlights potential delays and risks based on available project data.
- **ROI Reporting:** Supports future measurement of time saved and workflow efficiency when measurement data is available.

---

## 2. Technology Stack

### Primary Stack

- **Frontend:** Next.js 16, React 19, TypeScript 5
- **Styling:** Tailwind CSS v4
- **Icons:** Lucide React
- **Authentication:** Firebase Authentication
- **Primary Database:** Cloud Firestore
- **Generative AI:** Google Gemini on Vertex AI
- **Enterprise Search:** Vertex AI Search, when configured
- **Document Processing:** Google Document AI, when configured
- **Backend Hosting:** Google Cloud Run
- **File Storage:** Configured object storage; Amazon S3 remains an optional provider
- **Validation:** Zod
- **Testing:** Node.js test runner with `tsx`

### Optional / Legacy-Compatible Technologies

The following technologies are retained as alternatives for compatible deployments and are not the primary services in the Google Cloud architecture above.

- MongoDB Atlas with Mongoose 8
- Credentials authentication with `bcryptjs` and signed JWT cookies via `jose`
- Local file storage using `LocalStorageProvider` and `./uploads`
- Amazon S3 as an alternative object-storage provider
- MongoDB Atlas Vector Search and cosine similarity/keyword fallback
- Docker-based local MongoDB

---

## 3. Environment Variables Configuration

Copy `.env.example` to create your local `.env`:

```bash
cp .env.example .env
```

### Primary Google Cloud Configuration

Use the exact variable names expected by the current application code.

| Variable / Setting | Description | Example |
|---|---|---|
| `GOOGLE_CLOUD_PROJECT` | Google Cloud project ID | `your-project-id` |
| `GOOGLE_CLOUD_LOCATION` | Vertex AI region | `us-central1` |
| `GEMINI_MODEL` | Supported Gemini model identifier | `gemini-2.5-flash` |
| `FIREBASE_PROJECT_ID` | Firebase project ID | `your-project-id` |
| `FIRESTORE_DATABASE_ID` | Firestore database ID, if configured | `(default)` |
| `APP_URL` | Application base URL | `http://localhost:3000` |
| `PORT` | Container port for Cloud Run | `8080` |

Configure Google Cloud credentials using Application Default Credentials locally and an appropriate service identity in Cloud Run. Never commit service-account private keys, access tokens, or environment secrets.

### Optional / Legacy MongoDB and Local Storage Variables

Keep these only if the corresponding optional code paths remain enabled.

| Variable | Description | Example |
|---|---|---|
| `MONGODB_URI` | MongoDB Atlas connection string | `mongodb+srv://<user>:<pwd>@<cluster>.mongodb.net/?retryWrites=true&w=majority` |
| `MONGODB_DB` | Database name | `workmate_ai` |
| `AUTH_SECRET` | JWT signing secret, at least 32 characters | Set a unique local secret |
| `AUTH_COOKIE_NAME` | Session cookie identifier | `workmate_auth_token` |
| `gemini_API_KEY` | Legacy provider key variable | Only if expected by the current code |
| `gemini_MODEL` | Legacy model configuration | Original setup value |
| `STORAGE_DRIVER` | Optional storage driver | `local` |
| `STORAGE_LOCAL_DIR` | Local storage directory | `./uploads` |

**Security Note:** Never commit `.env` files or credentials. Keep `.env` ignored by Git and maintain a sanitized `.env.example`.

---

## 4. Google Cloud & Firebase Setup

### Firebase Authentication

1. Open the [Firebase Console](https://console.firebase.google.com/) and create or select the project.
2. Enable the sign-in providers supported by the application, such as email/password.
3. Configure Firebase client settings using the environment variables expected by the code.
4. Configure server-side token verification and authorization for protected API routes.
5. Test registration, sign-in, sign-out, session expiry, and unauthorized access.

### Cloud Firestore

1. In the Firebase Console, create or enable a Cloud Firestore database.
2. Select an appropriate region and configure security rules.
3. Scope user records to the authenticated user and workspace.
4. Enforce authorization on the server; do not rely solely on client-side checks.
5. Test read/write permissions and cross-user access restrictions.

### Gemini on Vertex AI

1. Select the correct Google Cloud project and enable the Vertex AI API.
2. Choose a supported Gemini model and region.
3. Configure Application Default Credentials for local development.
4. Attach a least-privilege service account to Cloud Run with the required Vertex AI permissions.
5. Test prompt handling, timeouts, errors, and grounded responses.

### Cloud Run Deployment

1. Ensure the application has a production build and working start command.
2. Containerize the application if required by the selected deployment method.
3. Deploy to Cloud Run and configure the required environment variables.
4. Assign a runtime service account with only the necessary permissions.
5. Test health checks, authentication, Firestore access, Gemini calls, uploads, and downloads.

### Optional MongoDB Atlas Setup

MongoDB Atlas remains available as an alternative for deployments using the original MongoDB-backed implementation.

1. Create a cluster on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
2. Under **Security > Database Access**, create a database user.
3. Under **Security > Network Access**, allow the required IP addresses.
4. Copy the driver connection string.
5. Set `MONGODB_URI` and `MONGODB_DB` only if the optional MongoDB implementation is enabled.

### Optional Local Docker MongoDB

If the legacy MongoDB path is still used and Docker is installed:

```bash
docker compose up -d
```

Set in `.env` if needed:

```env
MONGODB_URI="mongodb://localhost:27017"
MONGODB_DB="workmate_ai"
```

---

## 5. Development & Running the Application

### 1. Install Dependencies

```bash
npm install
```

### 2. Configure Environment

Create `.env` from `.env.example` and add the Firebase, Firestore, Gemini/Vertex AI, and other values required by the current code. Never add secrets to source control.

### 3. Start the Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

### 4. Run Automated Tests

```bash
npm test
```

Run the available unit and integration tests. Coverage depends on the current test suite and may include authentication, session security, AI provider errors, workspace boundaries, and dashboard metrics.

### 5. Run TypeScript Check

```bash
npx tsc --noEmit
```

### 6. Run Linter

```bash
npm run lint
```

### 7. Build for Production

```bash
npm run build
npm run start
```

---

## 6. API Endpoints Reference

> The following endpoints are preserved from the original README. Verify them against the current code after migrating to Firebase Authentication and Firestore; endpoint names and persistence behavior may differ.

### Authentication

- `POST /api/auth/register` — Original registration endpoint for creating a user and workspace.
- `POST /api/auth/login` — Original credential verification and JWT session creation.
- `POST /api/auth/logout` — Clears the original authentication cookie.
- `GET /api/auth/me` — Returns the authenticated user and workspace session details in the original implementation.

### AI Knowledge

- `POST /api/ai/chat` — Authenticated chat endpoint that loads context, calls the configured AI provider, saves conversation history, and records audit logs.
- `GET /api/ai/conversations` — Retrieves conversations for the active user and workspace.
- `GET /api/ai/conversations/[id]` — Retrieves a specific conversation and its message history.
- `DELETE /api/ai/conversations/[id]` — Deletes a conversation and its messages.

### Document Intelligence & RAG

- `POST /api/documents/upload` — Uploads PDF, DOCX, TXT, and CSV files with validation, storage isolation, text extraction, and AI onboarding.
- `GET /api/documents` — Lists workspace documents, status filters, and metrics.
- `GET /api/documents/[id]` — Returns metadata, page counts, chunk statistics, and text preview.
- `DELETE /api/documents/[id]` — Deletes the stored file and associated records according to the active storage/database provider.
- `POST /api/documents/[id]/onboard` — Re-runs AI onboarding analysis.
- `POST /api/documents/[id]/onboard/approve` — Approves onboarding configuration and starts indexing where configured.
- `GET /api/documents/[id]/chunks` — Retrieves indexed chunk previews.
- `GET /api/documents/[id]/download` — Provides secure workspace-isolated downloads.
- `POST /api/ai/chat` — Supports general chat and RAG modes with citations and document filtering where configured.

---

## 7. Optional MongoDB Atlas Vector Search Setup

The preferred Google Cloud search option is **Vertex AI Search**, when configured. This section is retained for deployments using the original MongoDB Atlas implementation.

Without an Atlas Vector Search index, the original implementation can use workspace-scoped cosine similarity and keyword matching.

To enable MongoDB Atlas Vector Search:

1. Open your MongoDB Atlas cluster and navigate to **Atlas Search / Vector Search**.
2. Select **Create Index > Atlas Vector Search (JSON Editor)**.
3. Select database `workmate_ai` and collection `DocumentChunks`.
4. Name the index `vector_index`, or match `MONGODB_VECTOR_INDEX` in `.env`.
5. Use this index definition:

```json
{
  "fields": [
    {
      "type": "vector",
      "path": "embedding",
      "numDimensions": 384,
      "similarity": "cosine"
    },
    {
      "type": "filter",
      "path": "workspaceId"
    },
    {
      "type": "filter",
      "path": "documentId"
    }
  ]
}
```

---

## 8. Scope Boundaries & Roadmap

- **Phase 1 — Core Foundation:** Application architecture, authentication, database integration, AI service integration, knowledge chat, dashboard, settings, and responsive application shell.
- **Phase 2 — Document Intelligence & Grounded RAG:** Multi-format ingestion, human-reviewed onboarding, document chunking, indexing, and source-cited knowledge chat.
- **Phase 3 — Data Quality & Preparation:** Spreadsheet profiling, cleaning pipelines, and interactive transformation approval.
- **Phase 4 — Multimodal Processing:** Document and image understanding, plus configured audio/video processing.
- **Phase 5 — Micro-App Studio:** On-demand prompt-to-tool creation with controlled execution.
- **Phase 6 — Meeting Intelligence & Workflow Automation:** Meeting summaries, tasks, approved workflows, and follow-ups.
- **Phase 7 — Productivity Analytics & Project Risk Insights:** Productivity reporting, risk indicators, and ROI measurement where supported by available data.

---

## 9. Security & Responsible AI

- **Least-Privilege Access:** Grant each service only the permissions it requires.
- **Authentication and Authorization:** Validate identity and enforce user/workspace boundaries server-side.
- **Data Protection:** Keep credentials and tokens out of source control; configure secure transport and storage.
- **Grounded Responses:** Use retrieved sources for knowledge answers and indicate when supporting evidence is unavailable.
- **Human Review:** Require approval for sensitive onboarding decisions and impactful workflow actions.
- **Safe Automation:** Validate inputs and restrict tool execution to explicitly approved operations.
- **Privacy-Aware Processing:** Minimize external transmission of confidential data and avoid sending raw sensitive rows when summaries are sufficient.

---

## 10. Google Services Used in the Primary Architecture

- **Firebase Authentication:** User identity and sign-in.
- **Cloud Firestore:** Application records and contextual data.
- **Google Gemini on Vertex AI:** Generative AI, summarization, reasoning, and supported multimodal understanding.
- **Cloud Run:** Backend hosting and scaling.
- **Vertex AI Search:** Semantic enterprise search and knowledge retrieval, when configured.
- **Document AI:** Structured document extraction, when configured.

Describe these services as active integrations only after they are configured and verified in the code and deployed environment.

---

## 11. Original Optional Components Preserved

The following components remain documented for backwards compatibility and can be used if the corresponding code paths are still required:

- MongoDB Atlas with Mongoose 8 and original collections (`Users`, `Workspaces`, `WorkspaceMemberships`, `Documents`, `DocumentChunks`, `AIConversations`, `AIMessages`, `ActivityLogs`, `Tasks`, `Workflows`).
- Credentials authentication with `bcryptjs`, signed JWT session cookies with `jose`, and original authentication endpoints.
- Local file storage via `LocalStorageProvider` and `./uploads`.
- Amazon S3 as an alternative object-storage provider.
- MongoDB Atlas Vector Search and cosine similarity/keyword fallback.
- Local Docker MongoDB for development.
- Original environment variables and API endpoint references where supported by the current code.
