DEV\_DesignArchitectureAddendum\_10.2.3\_20260128  
**Title:** Frontend–Backend Test Wiring and Authentication Integration (Current State)

---

## **1\. Purpose and Scope**

This addendum documents the **current, working** setup for:

1. How the **frontend talks to the backend** in all environments (local, test, production).  
2. How **authentication** is implemented and wired between frontend and backend.  
3. How **frontend tests** (unit and E2E) are wired relative to the backend and auth.

The goal is to prevent repeated “reverse-engineering” of integration behavior and to serve as the single source of truth for test wiring and auth integration for the v10.2 architecture.

---

## **2\. Repositories and Environments**

### **2.1 Repositories**

* **Frontend:** `fortunetell-frontend`  
  * Next.js application.  
  * Contains unit tests (Vitest) and E2E tests (Playwright).  
* **Backend:** `fortunetell-backend`  
  * Fastify-based API server.  
  * Uses Prisma and PostgreSQL.  
  * Exposes all HTTP routes under a global `/api/v1` prefix.

Repositories are **fully separated**. There is **no monorepo** and **no filesystem coupling** (no `cd ../backend` / shared node\_modules / shared build).

### **2.2 Environments**

* **Local Development**  
  * Frontend: `http://localhost:3000`  
  * Backend: `http://localhost:5000/api/v1`  
  * E2E tests: run against `http://localhost:3000` (frontend) which in turn calls `http://localhost:5000/api/v1` (backend).  
* **Production**  
  * Frontend: Vercel (final domain planned: `https://app.fortunetell.app`)  
  * Backend: Render at  
    `https://fortunetell.onrender.com/api/v1`  
  * Frontend communicates directly with the Render backend over HTTPS using the configured API base URL.  
* **CI**  
  * Backend CI: runs **only backend tests**.  
  * Frontend CI (current state): runs **unit tests only**; E2E tests are **temporarily disabled** to avoid blocking on non-critical accessibility contrast issues.

---

## **3\. Runtime Frontend → Backend Integration**

### **3.1 Core Contract**

* The frontend treats the backend as a **pure HTTP service** at a configured base URL.  
* All backend routes exist under a **single API base path**: `/api/v1`.  
* No client-side proxying or Next.js API routes are used for application data; the browser calls the backend directly.

### **3.2 Frontend Environment Variable**

Single critical env var:

* **`NEXT_PUBLIC_API_BASE_URL`** (frontend)  
  * **Scope:** Public (exposed to browser via Next).  
  * **Required:** Yes.  
  * **Meaning:** The **full backend base URL, including `/api/v1`**.  
  * **Examples:**  
    * Local: `http://localhost:5000/api/v1`  
    * Production: `https://fortunetell.onrender.com/api/v1`

All API calls are constructed as:

\<value of NEXT\_PUBLIC\_API\_BASE\_URL\> \+ \<endpoint path\>

Examples:

* Auth login:  
  `https://fortunetell.onrender.com/api/v1/auth/login`  
* Forecast run:  
  `https://fortunetell.onrender.com/api/v1/projection/run`

### **3.3 API Client Behavior**

* Location: `src/api/client.ts`

Base URL computation:  
const API\_BASE\_URL \= process.env.NEXT\_PUBLIC\_API\_BASE\_URL ?? "http://localhost:5000";

*   
  * In **normal usage**, `NEXT_PUBLIC_API_BASE_URL` **is set** and includes `/api/v1`.  
  * The fallback `"http://localhost:5000"` **does not include `/api/v1`** and is effectively a safety net only. For all sanctioned environments, the env var must be set correctly and the fallback should never be relied on.

Request construction:  
const baseUrl \= opts.baseUrl ?? API\_BASE\_URL;  
const res \= await fetch(\`${baseUrl}${path}\`, { ... });

* 

Example: forecast:  
apiClient.projection.run(body)  
// path \= "/projection/run"  
// final URL \= (NEXT\_PUBLIC\_API\_BASE\_URL) \+ "/projection/run"

* 

---

## **4\. Authentication Model (Runtime)**

### **4.1 Frontend Auth State**

* Implementation: Custom React context (not next-auth / not OAuth).  
* Storage:  
  * Token: `localStorage["fortuneTell.authToken"]`  
  * User profile: `localStorage["fortuneTell.authUser"]`  
* Transport:

All authenticated requests send:  
Authorization: Bearer \<JWT\>

*   
  * No auth cookies are used; this is **pure bearer-token auth**.

### **4.2 Backend JWT Behavior**

* Secrets / Issuer (environment variables on backend):  
  * `SUPABASE_JWT_SECRET` – secret used to sign / verify tokens.  
  * `SUPABASE_JWT_ISSUER` – expected `iss` claim.  
* Algorithm: HS256 via `jsonwebtoken`.  
* Lifetime: **7 days** (`"7d"` in signing options).

Claims (canonical shape):  
{  
  "sub": "\<user-id\>",  
  "email": "\<user-email\>",  
  "iss": "\<issuer\>",  
  "iat": \<issued-at\>,  
  "exp": \<expires-at\>  
}

*   
* Validation:  
  * Header must be `Authorization: Bearer <token>` (regex-validated).  
  * Token must be:  
    * Valid JWT.  
    * Signed with `SUPABASE_JWT_SECRET`.  
    * Matching `SUPABASE_JWT_ISSUER` (if configured).  
  * Failure conditions return an **error envelope** (see §6.2) with types like:  
    * `AUTH_INVALID`  
    * `AUTH_EXPIRED`  
    * `AUTH_REQUIRED`

### **4.3 Auth Endpoints**

(All paths include `/api/v1` prefix.)

* `POST /auth/register`  
  * Body: `{ email, password }`  
  * Public (no token required).  
  * Creates user and returns success envelope.  
* `POST /auth/login`  
  * Body: `{ email, password }`  
  * Public (no token required).  
  * Returns `{ data: { token: "<JWT>" } }`.  
* `GET /auth/me`  
  * Requires `Authorization: Bearer <JWT>`.  
  * Returns user object.

### **4.4 Frontend Auth Flow (Normal)**

1. User submits credentials to `/auth/login`.  
2. Backend returns signed JWT.  
3. Frontend:  
   * Stores token in `localStorage`.  
   * Stores user profile.  
4. All subsequent API calls include `Authorization: Bearer <token>` header.  
5. On token expiry or invalidation:  
   * Backend returns `AUTH_EXPIRED` / `AUTH_INVALID`.  
   * Frontend is expected to clear auth state and prompt user to re-authenticate.

---

## **5\. Frontend Testing Architecture**

### **5.1 Unit Tests (Vitest)**

* Command: `npm run test:unit`  
* Scope:  
  * React components.  
  * Hooks (`useForecast`, `useAdScheduler`, settings controllers, etc.).  
  * Utility functions (transaction sorting, error translation).  
* Backend interaction:  
  * **No real HTTP calls.**  
  * Any API interactions are **mocked**.  
* Dependency on backend:  
  * Only at **type / contract** level: there is a separate contract test that can regenerate API types from the backend OpenAPI spec, but it is not part of the standard unit test run.

### **5.2 E2E Tests (Playwright)**

* Command: `npm run test:e2e` → `playwright test`  
* Projects:  
  * Chromium.  
  * WebKit.

Base URL:  
use: {  
  baseURL: 'http://localhost:3000',  
  ...  
}

* 

Frontend web server:  
const apiBaseUrl \=  
  process.env.NEXT\_PUBLIC\_API\_BASE\_URL ?? 'http://localhost:5000/api/v1';

webServer: \[  
  {  
    command: 'PATH="$HOME/.nvm/versions/node/v20.19.5/bin:$PATH" npm run dev',  
    url: 'http://localhost:3000',  
    reuseExistingServer: true,  
    timeout: 60 \* 1000,  
    env: {  
      ...process.env,  
      NEXT\_PUBLIC\_API\_BASE\_URL: apiBaseUrl  
    }  
  }  
\]

* 

#### **5.2.1 Backend expectation during E2E**

* Playwright **does not** start the backend.  
* During E2E runs:  
  * Backend must already be running at:  
    `http://localhost:5000/api/v1`  
* For successful E2E:  
  * Start backend in another terminal (or process manager).  
  * Then run `npm run test:e2e`.

This enforces the “backend as external service” model even in tests.

### **5.3 E2E State Seeding and Auth**

* Central helper: `tests/helpers/e2eSetup.ts`  
* Primary entry: `resetAndSeedAppState(page, options)`:  
  * Optionally freeze time (`frozenTime`).  
  * Seed:  
    * `localStorage` with:  
      * Auth token and user.  
      * Onboarding completion flags.  
      * Accessibility preferences.  
      * Active scope/account.  
    * IndexedDB with accounts and transactions via an injected script.  
  * `page.goto('/')` (home page).  
  * `assertAppReady(page, { expectAccount })`:  
    * Ensures:  
      * Not on 404\.  
      * Not stuck on login.  
      * Forecast has completed successfully.  
      * DockBar “Record Transaction” button is visible.  
* **Key diagnostic:**  
  If `"Home page forecast did not load within 30s"` appears, that means:  
  * Backend is reachable but forecast did not complete, typically because:  
    * Backend isn’t running at `localhost:5000/api/v1`, or  
    * Auth token is invalid and `/projection/run` rejects it, or  
    * A forecast runtime error occurred.  
* **Auth for E2E (current intent):**  
  * E2E no longer relies on a naive placeholder like `"test-token-e2e-12345"` that the backend will reject.  
  * Instead, E2E must:  
    * Seed a **valid JWT** (signed with the same `SUPABASE_JWT_SECRET` / `ISSUER`), or  
    * Use an automated login/registration step to obtain a real token before seeding localStorage.  
  * The core requirement: **the token used in E2E MUST be something the backend will accept under current JWT rules**, or tests will fail at the forecast phase.

### **5.4 Current E2E Status**

* Local (as of 2026-01-28):  
  * All **functional E2E tests** pass:  
    * Account ledger ordering (postedDate / createdAt / id).  
    * Posted-date semantics.  
    * Account-page transaction persistence (including WebKit flakiness fix on radio click).  
  * Remaining E2E failures are **accessibility-only**:  
    * Color-contrast violations (red vs white) detected by axe-core.  
* CI:  
  * E2E tests are **not executed** in frontend CI for now to avoid blocking on the known contrast issues.  
  * Plan is to re-enable once token system / semantic color work brings the app into WCAG-compliant contrast ranges.

---

## **6\. Backend Behavior Relevant to Frontend & Tests**

### **6.1 API Base Path and Health**

* Global prefix: **`/api/v1`**.  
* Health endpoint:  
  * Path: `/api/v1/health`  
  * Public (no auth).  
  * Used for:  
    * Render health checks.  
    * Quick diagnosis from local dev and test environments.

### **6.2 Envelope Contracts**

**Success:**  
{  
  "data": { ...payload... }  
}

* 

**Error:**  
{  
  "error": {  
    "type": "ERROR\_CONSTANT",  
    "message": "Human-readable explanation",  
    "details": { ...optional... }  
  }  
}

*   
* Typical error types surfaced to frontend/tests relevant to integration:  
  * `AUTH_INVALID`, `AUTH_EXPIRED`, `AUTH_REQUIRED`  
  * `PROJECTION_INVALID_REQUEST`, `PROJECTION_ENGINE_ERROR`  
  * Standard `NOT_FOUND`, `VALIDATION_FAILED`, etc.

Frontend and tests assume this envelope shape and treat any response with an `error` field as a failed call, even if HTTP status is `200`.

### **6.3 CORS**

* Current code:  
  * `origin: true` (allows all origins).  
  * `credentials: true`.  
* Implications:  
  * Integration is permissive and will not block calls from Vercel or local hostnames.  
  * This is acceptable for development but is **not sufficient** for long-term production security.  
* Recommended (future) production configuration:  
  * Restrict origins to:  
    * `https://app.fortunetell.app`  
    * any designated staging domains.  
  * Implement via:  
    * A `FRONTEND_ORIGIN`/`ALLOWED_ORIGINS` env var, and  
    * A corresponding change in `src/app.ts` CORS setup.

---

## **7\. CI Behavior (Current State)**

### **7.1 Backend CI**

* Runs in `fortunetell-backend` only.  
* Behavior:  
  * Uses **backend directory as root**.  
  * Runs backend install, build, and tests.  
  * No references to `frontend` repo or frontend tests.  
* Purpose:  
  * Validate backend API logic, migrations, and runtime independently.

### **7.2 Frontend CI**

* Runs in `fortunetell-frontend` only.  
* Current pipeline:  
  * `npm ci`  
  * `npm run test:unit`  
  * `npm run test:e2e` is **intentionally omitted** for now.  
* Rationale:  
  * E2E tests are sensitive to:  
    * Backend availability.  
    * Accessibility contrast rules.  
  * To avoid blocking CI on non-essential accessibility issues, E2E runs are currently **developer-triggered** on local machines only.

---

## **8\. Known Gaps and Planned Future Work**

This section is intentionally forward-looking to prevent future misinterpretation of the current state as “final.”

1. **Accessibility contrast**  
   * Current UI red/white combinations fail WCAG AA color-contrast.  
   * E2E axe tests correctly flag these.  
   * Plan: resolve during **token / semantic color system** work and then:  
     * Confirm all axe tests pass locally.  
     * Re-enable E2E in CI (in full or as a smoke subset).  
2. **E2E in CI**  
   * Long-term goal: run at least a **smoke subset** of E2E tests in CI, with:  
     * A predictable backend (dedicated CI backend instance or container).  
     * Stable test auth user / token story.  
   * Until then, E2E is **local-only** and must be run manually before high-risk changes.  
3. **CORS tightening**  
   * Production backend should:  
     * Only allow specific frontend origins.  
   * This requires:  
     * Introducing env-based origin configuration.  
     * Updating deployment runbooks and docs.  
4. **Auth in tests**  
   * E2E should standardize on **one** of:  
     * Real login during test setup (preferred for fidelity).  
     * A clearly documented, test-specific JWT minting utility shared with backend.  
   * Whatever approach is canonical must be documented here and kept in sync with backend auth changes.

---

This addendum describes the **current, working integration and test wiring** between `fortunetell-frontend` and the backend API as of **2026-01-28**, and should be treated as the reference document when modifying:

* Environment variables for API connectivity.  
* Authentication behavior.  
* E2E / CI pipelines that involve cross-service integration.