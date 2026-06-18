# 🤖 MyBankAgent

A self-hosted, privacy-first personal finance platform featuring an autonomous AI financial analyst. The system architecture completely isolates your financial data within your home network, seamlessly syncing records via a direct open-banking API while delegating specialized analytical processing to a local AI engine.

---

## 🏗️ Local-Network Infrastructure Architecture

Unlike traditional finance applications that process private ledgers on commercial cloud providers, **MyBankAgent** implements a hybrid, decoupled topology across your secure local area network (LAN):

1. **Always-On Storage & Middleware (Synology NAS via Docker):** 
   Runs the dual Express applications (Frontend UI & Backend REST API) along with the persistent MySQL relational database. It fetches bank transactions 24/7, keeping records constantly updated.
2. **On-Demand Processing Engine (Dedicated Workstation):** 
   Hosts the local AI core utilizing **Ollama (Qwen2.5-Coder:7b)** leveraging dedicated GPU acceleration.
3. **Graceful Degradation:** 
   When the workstation is powered down, the platform automatically triggers a fallback loop. Users retain full data audit control, interactive tracking graphs, and previous chat logs cached within MySQL, while new prompt options degrade gracefully to an "offline" state.
4. **Zero-Trust Boundary Deployment:** 
   Remote requests map exclusively through a hardware-level **OpenVPN** tunnel on your router. No inbound router ports are exposed to the public internet.

---

## 🎯 Core User Stories (MVP)

### 📊 Epic: Privacy-First AI Financial Management

#### User Story 1: Sovereign Data & Local Architecture
* **As a** privacy-conscious individual,
* **I want to** host a personal finance platform entirely on my local hardware with direct bank API synchronization and a decoupled local AI,
* **So that** my sensitive financial records and analytics never touch third-party cloud services or leave my home network.

#### User Story 2: Consolidated Multi-Account Expense Tracking
* **As a** user with multiple bank accounts,
* **I want to** view a consolidated dashboard of my categorized expenses across all accounts grouped by month and year,
* **So that** I can easily cross-reference seasonal spending spikes and compare my current financial year against historical periods.

#### User Story 3: Debt and Interest Analytics
* **As a** user managing loans or credit lines,
* **I want to** track my aggregate debt levels and total interest fees paid month-over-month from my earliest available data point,
* **So that** I can visualize my debt reduction progress and accurately audit the true cost of my liabilities.

#### User Story 4: Offline-Capable AI Advisory & Debt Simulation
* **As a** user seeking financial optimization,
* **I want to** consult a local AI analyst for cost-cutting ideas and debt payoff simulations (e.g., extra principal payments), while preserving all historical chats and generated tips in my database,
* **So that** I can continuously review my financial strategies even when my primary workstation running the AI engine is powered down.

#### User Story 5: Rule-Based Automatic Categorization
* **As a** user tracking monthly recurring expenses,
* **I want to** create deterministic rules that automatically categorize incoming transactions based on text matching in the description,
* **So that** predictable recurring line items (such as gym memberships or subscriptions) are accurately classified without requiring repetitive manual overrides every month.

---

## 🛠️ Monolithic Repository Directory Structure

```text
my-bank-agent/
│
├── .devcontainer/
│   └── devcontainer.json   # Provisions isolated node, network, and database containers
├── docker-compose.yml     # Local orchestration for development MySQL images
│
├── back-end/
│   ├── bruno/             # E2E automated API integration tests
│   ├── config/            # Database driver pools (Sequelize initialization)
│   ├── middleware/        # JSON payload validation and JWT authorization handlers
│   ├── models/            # Relational database models (User, Transaction, ChatLog, Rule)
│   ├── controllers/       # Decoupled transaction layer managing req/res payloads
│   ├── routes/            # Core routing declarations 
│   ├── adapters/          # Universal translation adapters consuming vanilla native fetch
│   ├── services/          # Business logic encapsulation and pure SQL queries
│   ├── tests/             # Jest unit and cryptographic integration suite
│   ├── app.js             # Main server entrypoint
│   └── package.json
│
└── front-end/
    ├── public/
    │   ├── css/           # Bootswatch professional themes (Lux/Darkly)
    │   └── js/            # Client-side processing rendering Chart.js visualizations
    ├── routes/            # Frontend routing mapping view components
    ├── utils/             # Reusable generic native fetch wrappers connecting to the API
    ├── views/             # EJS Template engines structured by domain modules
    │   ├── partials/      # Global atomic HTML segments (head, navbar, scripts)
    │   ├── dashboard.ejs
    │   └── ai-analyst.ejs
    ├── app.js
    └── package.json
```

---

## 🧰 Technology Stack

### Backend Stack
* **Runtime Environment:** Node.js (LTS v20+)
* **Application Framework:** Express.js (REST API Architecture)
* **Database Management Layer:** Sequelize (ORM configured to process robust transaction models and native, parametric raw SQL strings)
* **Database Engine:** MySQL 8.0 (ACID compliant ledger tracking)
* **API Ingestion Strategy:** Pure native `fetch` API wrappers (strictly **no Axios** dependencies to isolate the pipeline against third-party supply chain attack vectors)

### Frontend Stack
* **View Template Engine:** Embedded JavaScript (EJS templates built inside atomic modular blocks)
* **User Interface Framework:** Bootstrap 5 spiced with custom **Bootswatch** rendering (Lux/Darkly) and Bootstrap Icons
* **Data Visualization Layer:** **Chart.js** (Client-side Canvas graphing initialized via native JSON array mapping)

### Testing & Tooling
* **Development Isolation:** VS Code Dev Containers (automated containerized workspace initialization)
* **Automated Unit Testing Framework:** Jest (Unit validations, mock services, and cryptography suites)
* **E2E Integration Validation:** Bruno REST client collections

---

## 🧪 Development Methodology & Design Patterns

### 🚨 Incremental Test-Driven Development (TDD)
The project is built incrementally following a rigorous local verification workflow. Database integrations, service layers, and cryptographic components are continuously monitored using **Jest** before adding route end-points or client markup views. Status check regressions are enforced through project parameters.

### 🔌 The Adapter Pattern
To maintain a flexible, platform-agnostic baseline, the repository implements a strict **Adapter Pattern** for external bank layers. The application logic never parses multi-vendor JSON response signatures directly. 


All raw transactions feed into uniform, normalized internal camelCase contracts via provider-specific modules. Setting `ACTIVE_BANK=MOCK` inside the `.env` context triggers a deterministic test-data generator, ensuring the full Fullstack ecosystem executes perfectly in sandbox isolation without active bank credential hooks.

### 🔐 Native Isolation Cryptography
User registration profiles are cryptographically secured using Node.js' native `crypto` module. Authentication layers rely on asynchronous `crypto.scrypt()` computation blocks for hashing, paired with `crypto.timingSafeEqual()` matching buffers to completely immunize the backend login handlers against targeted timing attacks.

---

## 🚀 Future Roadmap (v2)

While the initial MVP focuses heavily on data privacy, transaction tracking, debt audit, and AI advisory, the following capabilities are prioritized for the next major release:

* **📊 Dynamic Budgeting Engine:** Establish custom spending limits per category, track monthly consumption thresholds in real-time, and view progress graphs.
* **⚠️ Proactive AI Alerts:** Enable the AI analyst to generate automated warning notifications before specific spending thresholds are breached.
* **⚡ Cache Optimization:** Integrate Redis memory stores to handle persistent session syncing and cache expensive analytical datasets.

---

## 📄 License
This project is licensed under the terms of the [MIT License](LICENSE).
