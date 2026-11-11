# Project Context & Agent Guidelines (`AGENTS.md`)

> **Instructions for AI Agents:** Read this entire file before exploring or modifying code. Every subagent (`ui-designer`, `developer`, `code-reviewer`, `security-auditor`, `tester`) must strictly follow these conventions.

---

## 1. Project Overview
- **Project Name:** [e.g., ShopMate App / Order Service]
- **Project Type:** [e.g., Mobile App / Backend API / Monorepo (Mobile + Backend)]
- **Core Purpose:** [1–2 sentence summary of what this repository does]

---

## 2. Tech Stack & Tooling
*(Fill in the applicable sections and delete unused ones)*

### Mobile / Frontend (if applicable)
- **Framework & Language:** [e.g., Flutter (Dart 3.x) / Next.js (TypeScript)]
- **State Management:** [e.g., BLoC / Riverpod / GetX / Zustand]
- **Routing:** [e.g., go_router / auto_route]
- **Dependency Injection:** [e.g., get_it + injectable / Provider]
- **Networking & Storage:** [e.g., Dio, flutter_secure_storage, Hive/Drift]
- **Package Manager & Commands:**
  - Install: `[e.g., flutter pub get]`
  - Lint / Analyze: `[e.g., dart analyze]`
  - Test: `[e.g., flutter test]`

### Backend (if applicable)
- **Framework & Language:** [e.g., Python 3.12 + FastAPI / Node.js + NestJS]
- **Package / Environment Manager:** [e.g., uv (always use `uv add` and `uv run`)]
- **Database & ORM:** [e.g., PostgreSQL + SQLAlchemy (async) + Alembic]
- **Authentication:** [e.g., JWT (Access + Refresh tokens) via OAuth2]
- **Package Manager & Commands:**
  - Install: `[e.g., uv sync]`
  - Lint / Format: `[e.g., uv run ruff check .]`
  - Type Check: `[e.g., uv run mypy .]`
  - Test: `[e.g., uv run pytest]`

---

## 3. Folder Structure & Architecture
- **Architectural Pattern:** [e.g., Clean Architecture (Feature-first) / Layered MVC]
- **Key Directories:**
  - `[path/to/features]` — Feature modules (presentation, domain, data layers)
  - `[path/to/core]` — Shared utilities, network client, theme, error handling
  - `[path/to/tests]` — Unit, widget/component, and integration tests

---

## 4. Coding Standards & Non-Negotiables
- **Principles:** Strictly enforce **DRY** (Don't Repeat Yourself) and **SoC** (Separation of Concerns). Keep UI components free of business logic and direct network calls.
- **Immutability & Models:** [e.g., Use `Equatable` or `Freezed` for states/events; use Pydantic v2 schemas for API request/response validation.]
- **Error Handling:** [e.g., Never swallow exceptions silently; map data-layer errors to typed domain failures / standard HTTP error responses.]
- **Resource Lifecycle:** Always dispose controllers, streams, subscriptions, timers, and database sessions properly.
- **Forbidden Practices:**
  - Do not introduce new third-party packages or competing state management libraries without explicit approval.
  - Never hardcode API keys, secrets, tokens, or environment URLs in source files.

---

## 5. UI / Design System Guidelines (For `ui-designer`)
- **Design Source:** [e.g., Stitch MCP Project ID / Figma Design Tokens]
- **Theme & Styling:** [e.g., Material 3 using centralized `AppTheme`, `AppColors`, and `AppTypography`—avoid inline hardcoded hex colors or text styles.]
- **Responsiveness:** Ensure layouts adapt cleanly across screen sizes and handle loading, empty, and error states.

---

## 6. Shared API Contract (For Multi-Project / Full-Stack Tasks)
*(Keep this section updated when mobile/web and backend agents work in parallel)*

- **Base URL:** `[e.g., /api/v1]`
- **Auth Header:** `Authorization: Bearer <access_token>`
- **Standard Response Envelope:**
  ```json
  {
    "success": true,
    "data": {},
    "error": null
  }
  ```
- **Active Endpoints / Feature Contracts:**
  - `[METHOD] /endpoint` — Request payload & Response schema summary

---

## 7. Testing Requirements (For `tester`)
- **Minimum Coverage:** Every new feature or bug fix must include unit tests for business logic/services and widget/endpoint tests for UI/API routes.
- **Mocking Strategy:** [e.g., `mocktail` / `bloc_test` for Flutter; `pytest-asyncio` + `httpx.AsyncClient` for Python]
- **Definition of Done:** All analyzer/linter checks pass with zero warnings, security audit is clean, and the entire test suite exits with code `0`.
