# 🗂️ BreatheCode (4Geeks Academy) API — SKILL_LOG

> Documentation for the BreatheCode API connection (4Geeks Academy)
> Base URL: `https://breathecode.herokuapp.com/v1`

---

## 📦 Milestone 1: Authentication & Token

### 🔍 Token Retrieval

**First attempt (failed):**
From the 4Geeks interface, we went to **Application → Local Storage → accessToken** in the browser's DevTools.
The token displayed there turned out to be invalid/expired when tested against the API (it responded with `"Invalid or Inactive Token"`).

**Second attempt (successful):**
We used **DevTools → Network tab** to capture a real request made by the web application to the BreatheCode API. We looked for a request containing `breathecode.herokuapp.com`, and there in **Request Headers** we found the `Authorization: Token <...>` header. That token was valid.

> ⚠️ **Lesson learned:** The token visible in `Application → Local Storage → accessToken` may be expired. The one actually used by the application in active sessions can be different. The most reliable way to capture it is from the **Network** tab, looking at the `Authorization` header of an outgoing request to the API.

**Token stored** ✅ (2026-08-30)
- Format: `Authorization: Token <hash>`
- Stored securely at `~/.openclaw/workspace/.breathecode/config.sh` (permissions 600, not shown in logs)

**Endpoint tested:**
```bash
GET /v1/admissions/user/me
→ 200 OK. Returns full user profile with cohorts, roles, permissions.
```

**Authentication methods tested:**

| Format | Result |
|---------|----------|
| `Authorization: Token <hash>` | ✅ Works (Django Rest Framework) |
| `Authorization: Bearer <hash>` | ❌ 401 - Authentication credentials not provided |
| `Authorization: JWT <hash>` | ❌ 401 - Authentication credentials not provided |
| `Authorization: <hash>` (raw) | ❌ 401 - Authentication credentials not provided |
| Query param `?token=<hash>` | ❌ 401 - Authentication credentials not provided |

**Conclusion:** The API uses Django Rest Framework with `Token`-based authentication.

---

## 📦 Milestone 2: Discovered Endpoints

### ✅ Working Endpoints (tested)

| Endpoint | Description | Response |
|---|---|---|
| `GET /v1/admissions/user/me` | Current user + cohorts + progress | ✅ Full data |
| `GET /v1/admissions/academy/cohort/me` | User's cohorts | ⚠️ Requires `Academy` header |
| `GET /v1/assignment/user/me/task` | All tasks/assignments | ✅ Full data |
| `GET /v1/admissions/public/syllabus` | Public syllabus | ❓ Pending test |

### 🔍 Endpoints to Explore

- `GET /v1/admissions/academy/cohort/me` (with Academy header)
- `GET /v1/assignment/user/me/task?cohort=<slug>` (filtered by cohort)

---

## 📦 Milestone 3: Key Data Structure

### User & Cohorts (`/v1/admissions/user/me`)

> **Note:** User data lives at the root level, not inside a nested `user` object.

```json
{
  "id": 21741,
  "email": "mariacidtl@gmail.com",
  "first_name": "María",
  "last_name": "Del Espino Del Cid Toledo",
  "username": "mariacidtl@gmail.com",
  "github": { "username": "mariacidtl" },
  "profile": { ... },
  "cohorts": [
    {
      "cohort": { "id": 1727, "slug": "spain-aie-pt-4", "name": "spain-aie-pt-4" },
      "role": "STUDENT",
      "educational_status": "ACTIVE",
      "finantial_status": "UP_TO_DATE",
      "completion": {
        "strategy": { "type": "LEGACY_PROJECTS" },
        "overall": { "total": 7, "completed": 0, "percent": 0.0 },
        "required": {
          "PROJECT": {
            "total": 7, "completed": 0, "percent": 0.0,
            "is_met": false,
            "missing": ["slug1", "slug2", ...]
          }
        },
        "pending_required_slugs": { "PROJECT": [...] },
        "pending_required_count": 7
      }
    }
  ]
}
```

### Tasks (`/v1/assignment/user/me/task`)

```json
{
  "id": 12345,
  "title": "Task name",
  "task_status": "PENDING" | "DONE",
  "task_type": "PROJECT" | "EXERCISE" | "LESSON" | "QUIZ",
  "revision_status": "PENDING" | "APPROVED",
  "associated_slug": "task-slug",
  "github_url": "https://github.com/...",
  "live_url": null,
  "cohort": { "id": 1727, "slug": "spain-aie-pt-4" },
  "opened_at": "2026-08-24T16:55:42Z",
  "delivered_at": "2026-08-26T18:14:53Z"
}
```

---

## 📦 Milestone 4: Created Skills

### Skill 1: `breathecode-auth`

| Field | Value |
|---|---|
| **Purpose** | Verify token validity and display user profile |
| **Endpoint** | `GET /v1/admissions/user/me` |
| **Prompt** | *"Verifica que mi token de 4Geeks sigue siendo válido"* |

**Test result:**

```json
{
  "authenticated": true,
  "user": {
    "id": 21741,
    "email": "mariacidtl@gmail.com",
    "first_name": "María",
    "last_name": "Del Espino Del Cid Toledo",
    "username": "mariacidtl@gmail.com",
    "github": { "username": "mariacidtl" }
  },
  "cohort_count": 29,
  "roles": ["student"]
}
```

---

### Skill 2: `breathecode-projects`

| Field | Value |
|---|---|
| **Purpose** | List all projects with status (pending, submitted, graded) |
| **Endpoint** | `GET /v1/assignment/user/me/task` |
| **Prompt** | *"Muéstrame todos mis proyectos de 4Geeks con su estado"* |

**Test result:**

```json
{
  "total_projects": 32,
  "pending": 12,
  "done": 20,
  "recent_pending": [
    {"title": "Todo List CLI with Python", "cohort": "spain-aie-pt-4"},
    {"title": "Centralized Incident Manager", "cohort": "error-handling-debugging-and-testing"},
    {"title": "Error Handling", "cohort": "error-handling-debugging-and-testing"},
    {"title": "Backend Architecture Proposal", "cohort": "spain-backend-development-with-coding-agents"},
    {"title": "Securing the API: Authentication and Route Restric", "cohort": "autentication-in-web-applications"}
  ],
  "recent_done": [
    {"title": "Talk to the Machine — Chat Interface", "delivered_at": "2026-08-24"},
    {"title": "Milestone 3 — Talent Pipeline Tracker", "delivered_at": "2026-08-26"},
    {"title": "My Agent, My Way: Teaching Your Personal Assistant", "delivered_at": "2026-08-28"}
  ]
}
```

---

### Skill 3: `breathecode-pending`

| Field | Value |
|---|---|
| **Purpose** | Get pending work grouped by cohort |
| **Endpoint** | `GET /v1/admissions/user/me` + `GET /v1/assignment/user/me/task` |
| **Prompt** | *"Qué trabajos tengo pendientes en 4Geeks?"* |

**Test result:**

```json
{
  "total_pending_cohorts": 16,
  "total_missing_projects": 32,
  "pending_by_cohort": [
    {
      "cohort": "spain-aie-pt-4",
      "stage": "STARTED",
      "progress": "0/7 (0.0%)",
      "missing_count": 7,
      "missing": [
        "ai-eng-milestone-web-fundamentals",
        "exercise-terminal-challenge",
        "first-collaborative-project-tailwind-css",
        "html-css-artist-landing-seo-access",
        "simple-dashboard-tailwind-css",
        "todo-list-cli-python",
        "typescript-cinema-seat-manager"
      ]
    },
    {
      "cohort": "Frontend development with Coding Agents",
      "stage": "INACTIVE",
      "progress": "3/4 (75.0%)",
      "missing_count": 1,
      "missing": ["chat-interface-real-ai-api"]
    },
    {
      "cohort": "Coding Fundamentals with Typescript",
      "stage": "INACTIVE",
      "progress": "3/4 (75.0%)",
      "missing_count": 1,
      "missing": ["ai-eng-milestone-coding-fundamentals"]
    },
    {
      "cohort": "Backend development with Coding Agents",
      "stage": "INACTIVE",
      "progress": "0/3 (0.0%)",
      "missing_count": 3,
      "missing": ["ai-basic-inventory-agent-loop", "ai-eng-company-incidents-analysis", "voice-to-do-list-api"]
    },
    {
      "cohort": "Advanced personal assistants with Openclaw",
      "stage": "INACTIVE",
      "progress": "0/3 (0.0%)",
      "missing_count": 3,
      "missing": ["openclaw-integration", "openclaw-memory", "openclaw-skills"]
    }
  ]
}
```

> *Note: Only the most relevant pending cohorts are shown above. 11 additional cohorts with pending projects exist.*

---

### Skill 4: `breathecode-progress`

| Field | Value |
|---|---|
| **Purpose** | Overall course progress summary across all cohorts |
| **Endpoint** | `GET /v1/admissions/user/me` |
| **Prompt** | *"Cómo va mi progreso general en 4Geeks?"* |

**Test result:**

```json
{
  "student": "María Del Espino Del Cid Toledo",
  "email": "mariacidtl@gmail.com",
  "total_cohorts": 29,
  "legacy_project_cohorts": 19,
  "completed_cohorts": 4,
  "total_required_projects": 47,
  "completed_projects": 16,
  "completion_percent": 34.0,
  "pending_projects": 31
}
```

---

### Skill 5: `breathecode-module-summary`

| Field | Value |
|---|---|
| **Purpose** | Generate a structured summary of any course module and append it to a shared Google Doc |
| **Endpoint** | `GET /v1/assignment/user/me/task` + Zapier MCP (Google Docs) |
| **Google Doc ID** | `1IxP6RTHF0Zz9yMnNQwOb51r43_BtGQuy8T6lCDilyIc` |
| **Prompt** | *"Hazme un resumen del módulo 'Advanced personal assistants with Openclaw'"* |

**Test result:** Summary of "Advanced personal assistants with Openclaw" written to Google Doc:

| Section | Content |
|---|---|
| **Duration** | 20 hours (3 days) |
| **What you learn** | Advanced OpenClaw concepts, skills system, memory management, API integration |
| **Languages/Tools** | OpenClaw, Zapier MCP, Shell scripting, Python, Google Docs API |
| **Key concepts** | Skills, MCP servers, secrets management, agent memory, API tokens |
| **Deliverables** | 1 pending project, 1 completed project, 5 completed exercises |
| **Google Doc** | https://docs.google.com/document/d/1IxP6RTHF0Zz9yMnNQwOb51r43_BtGQuy8T6lCDilyIc/edit |

---

### Skill 6: `breathecode-next-projects`

| Field | Value |
|---|---|
| **Purpose** | Identify the 3 most important **truly pending** projects to deliver with a brief summary of each |
| **Endpoint** | `GET /v1/assignment/user/me/task` |
| **Prompt** | *"Cuáles son los siguientes 3 proyectos que tengo que entregar?"* |

**Deduplication logic:**
- Excludes projects already marked as **DONE** in any cohort
- Excludes duplicate entries in the general cohort `spain-aie-pt-4` that are already **DONE** in their respective micro-cohorts (submodules)
- Orders by curriculum sequence (module order from the course syllabus)

**Ordering:** Projects are sorted by curriculum position (exact module order from the course syllabus), not by creation date.

**Test result (2026-08-31):**

```
🎯 3 próximos proyectos a entregar (orden curricular)

1. My 4Geeks Assistant — Teaching OpenClaw to Track Your Progress
   📚 Advanced personal assistants with Openclaw
   📝 Conectar tu asistente OpenClaw con la API de 4Geeks Academy para que pueda seguir tu progreso académico automáticamente.
   🔧 OpenClaw, BreatheCode API (REST), Python, mcporter

2. Building context from an existing project - Financial dashboard
   📚 Working with AI coding agents → Context Engineering
   📝 Construir contexto a partir de un dashboard financiero para entender cómo los AI coding agents trabajan con código heredado.
   🔧 AI coding agents, prompt engineering, code analysis, git

3. Applying Spec Driven Development - Financial dashboard
   📚 Working with AI coding agents → Spec-Driven Development
   📝 Aplicar desarrollo guiado por especificaciones al dashboard financiero, escribiendo specs que guíen a los agentes AI.
   🔧 Spec-driven dev, AI agents, documentation, iterative prompting
```

**Next in queue (when top 3 are done):**

| Pos | Proyecto | Módulo |
|-----|----------|--------|
| 4 | Enhancing development with agent skills - Financial dashboard | Working with AI coding agents → Developing Rules & Skills |
| 5 | Backend Architecture Proposal | Backend development with Coding Agents |
| 6 | Securing the API: Authentication... | Authentication in web applications |
| 7 | Centralized Incident Manager | Error handling, debugging and testing |
| 8 | Error Handling | Error handling, debugging and testing |

**Cron job:** `a5701e04` — every Monday at 08:00 Europe/Madrid — announces to Telegram

---

## 📦 Milestone 5: Configuration

### Token Location

```
~/.openclaw/workspace/.breathecode/config.sh
```

### Helper Function for Scripts

```bash
source ~/.openclaw/workspace/.breathecode/config.sh

# Makes authenticated requests
breathecode_api GET "/admissions/user/me"

# Or with direct curl
curl -s -H "Authorization: Token $BREATHECODE_TOKEN" \
  "https://breathecode.herokuapp.com/v1/admissions/user/me"
```

---

## 📦 Overall Progress

### Global Summary (2026-08-30)

| Metric | Value |
|---|---|
| Cohorts with required projects | 19 |
| Completed cohorts | 4 |
| Total required projects | 47 |
| Completed | 16 |
| **Global completion %** | **34.0%** |

### Completed Cohorts ✅
- `spain-ai-engineering-introduction` (3/3 - 100%)
- `web-ui-fundamentals-with-tailwind` (3/3 - 100%)
- `command-line-git-and-github` (2/2 - 100%)
- `personal-assistants-with-openclaw` (2/2 - 100%)

### Active Cohort 🔵
- `spain-aie-pt-4` — STARTED — 7 pending projects

### Partial Progress Cohorts 🟡
| Cohort | Progress |
|---|---|
| `frontend-development-with-coding-agents` | 3/4 (75%) |
| `coding-fundamentals-with-typescript-` | 3/4 (75%) |

---

## 🔧 Technical Notes

- The BreatheCode API runs on **Django Rest Framework**
- Tokens can expire; renew from the browser in `Application → Local Storage → accessToken`
- The `user/me` response is very large (includes all cohorts, each with its progress)
- `assignment/user/me/task` returns tasks from **all** cohorts, no default filter