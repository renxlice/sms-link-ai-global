# SMS-Link AI Global
### Humanitarian Registration via Intelligent SMS Automation

<p align="center">
  <img src="https://img.shields.io/badge/UiPath-Studio%202024%2F2026-orange?style=for-the-badge&logo=uipath" />
  <img src="https://img.shields.io/badge/Architecture-Human--in--the--Loop-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge" />
</p>

---

## Project Overview

**SMS-Link AI Global** is an automated registration platform designed to accelerate humanitarian aid distribution. The solution captures incoming text message registration data, validates payload structures, and utilizes an intelligent **Human-in-the-Loop** architecture to dynamically handle data discrepancies — without process interruption.

---

## The Problem

Every year, millions of refugees and displaced persons attempt to register for critical humanitarian aid programs — including food, shelter, medical care, and legal protection. However, a significant **digital divide** exists:

- The vast majority of displaced populations **lack smartphones or stable internet access**, relying instead on basic mobile devices capable only of sending SMS.
- Aid organizations operate on digital systems that **require structured, validated data**.
- The gap between a raw incoming SMS and a structured database record frequently causes registrations to fail due to mistyped identity numbers or rigid ingestion systems that cannot handle incomplete data formats gracefully.

> **Vulnerable populations are often rejected by the very systems designed to assist them.**

---

## The Solution

**SMS-Link AI Global** bridges the gap between basic SMS inputs and fully validated humanitarian registration databases with **zero manual data re-entry** and a **resilient human-in-the-loop fallback mechanism**.

The workflow operates through the following stages:

| Stage | Description |
|:---|:---|
| **Data Ingestion** | Automatically reads and parses inbound registration text messages containing registrant data (Name, National ID, Category) |
| **Format Validation** | Executes structural integrity checks to verify that the National ID format adheres to the strict 16-digit standard |
| **Instant Ingestion** | Valid registrations are automatically committed to the registry ledger and marked as `Success` without human intervention |
| **Human-in-the-Loop Escalation** | Invalid IDs trigger an exception task in UiPath Action Center; the process suspends execution, waits for an agent to correct the data via a structured form, and resumes immediately upon submission |
| **Audit Trails** | Every execution and status change is logged into a structured database registry for comprehensive tracking and compliance |
| **Idempotency Guardrails** | Built-in validation structures ensure duplicate entries are filtered out and not reprocessed across multiple system runs |

---

## Business Impact

| Metric | Before SMS-Link AI Global | After SMS-Link AI Global |
|:---|:---|:---|
| **Registration Channel** | Internet-only portal | SMS (compatible with any mobile device) |
| **Data Validation** | Manual and error-prone | Automated and instant integrity validation |
| **Invalid ID Handling** | Automatic rejection | Escalation to human agent with real-time resolution |
| **Processing Speed** | Hours to days | Minutes |
| **Duplicate Registrations** | Common operational issue | Prevented by design through idempotent controls |
| **Audit Trail** | Inconsistent or manual | Complete structured logs on every run |

### Who Benefits?

- **Refugees and Displaced Persons** — Enabled to register securely from any location with a basic cellular signal.
- **Aid Organizations** — Eliminates manual administrative entry, decreases operational error rates, and scales processing capacity.
- **Human Agents** — Relieved from routine validations; focus shifts exclusively to edge cases requiring genuine human judgment.

---

## Agent Architecture

This solution utilizes a **hybrid architecture** combining coding agents and human orchestration:

```
┌─────────────────────────────────────────────────────────────┐
│                     SMS-Link AI Global                      │
│                                                             │
│  Inbound SMS                                                │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    Valid ID        ┌──────────────────┐    │
│  │   Parser &  │ ─────────────────► │  Auto-Commit to  │    │
│  │  Validator  │                    │  SMS_Gateway.xlsx │   │
│  └─────────────┘                    └──────────────────┘    │
│       │                                                     │
│       │  Invalid ID                                         │
│       ▼                                                     │
│  ┌─────────────┐    Suspend         ┌──────────────────┐    │
│  │  Exception  │ ─────────────────► │  UiPath Action   │    │
│  │  Handler    │                    │  Center Task     │    │
│  └─────────────┘                    └────────┬─────────┘    │
│       ▲                                      │              │
│       │           Human resolves             │              │
│       └──────────────────────────────────────┘              │
│                  Resume & Commit                            │
└─────────────────────────────────────────────────────────────┘
```

- **Core Workflow** — Parsing layers and database synchronization are executed via coding agents built using UiPath Studio workflow logic.
- **Exception Handling** — Orchestrated through low-code interactive forms within UiPath Action Center, allowing a seamless blend of automated task execution and human oversight.

---

## UiPath Components Used

| Component | Role |
|:---|:---|
| **UiPath Studio** | Core sequential workflow design and validation development |
| **UiPath Action Center & Form Tasks** | Human-in-the-loop task generation, interface assignment, and data verification |
| **UiPath Persistence Activities** | `WaitForFormTaskAndResume` — suspends and resumes the robot lifecycle |
| **UiPath Orchestrator / Maestro** | Centralized process deployment, environment folder syncing, and live transaction logging |
| **UiPath Excel Activities** | Data synchronization, spreadsheet updates, and ledger maintenance with header preservation |

---

## Prerequisites

Before running the solution, ensure you have the following environment configuration:

- **UiPath Studio** (2024.x / 2026.x or newer) with Windows/Cross-Platform compatibility
- **UiPath Automation Cloud Account** with a provisioned Orchestrator Tenant
- **UiPath Action Center** service enabled in your specific Orchestrator Tenant
- **Required Package Dependencies** (install via *Manage Packages* in Studio):

```
UiPath.Persistence.Activities
UiPath.Excel.Activities
UiPath.Form.Activities
```

---

## Setup and Installation

### 1. Clone and Open the Project

```bash
git clone https://github.com/YOUR_USERNAME/sms-link-global.git
```

Open **UiPath Studio**, select **Open Local Project**, and open the `project.json` file inside the cloned repository directory.

---

### 2. UiPath Orchestrator Connection Setup

Because this project utilizes Orchestrator's persistence engine and Action Center, you must ensure your local Studio instance is properly provisioned:

1. Log in to your **UiPath Automation Cloud** account via web browser.
2. Open **UiPath Assistant** on your desktop, click the settings icon, sign in, and ensure the status bar turns green (Connected / Licensed).
3. In your Automation Cloud web portal, ensure the **Action Center** service is enabled inside your specific Tenant settings.
4. Open the project in UiPath Studio and verify the bottom-right status bar shows a successful connection to your active **Cloud Orchestrator Modern Folder**.

---

### 3. Local Environment Directory Creation

The robot automatically manages its own local workspace directories. On its first run, it will generate the following folders and files in your system root to process incoming payloads:

```
C:\SMS_Link_AI_Global_Orchestration\
├── SMS_Inbound\
│   └── sms_inbound.txt   <- Demo payload (auto-generated)
└── SMS_Processed\
```

**Default mock payload content in `sms_inbound.txt`:**
```
REG#Mamat Alkatiri#12345#Social
```

---

### 4. Setup the Master Ledger (Excel Gateway)

Before running the workflow, you must manually provide the data transaction ledger template:

1. Create a blank Excel file named **`SMS_Gateway.xlsx`**
2. Create a sheet inside named **`SMS_Sheet`**
3. Place the file at the following path:

```
C:\SMS_Link_AI_Global_Orchestration\SMS_Gateway.xlsx
```

> **Note:** Keep the file closed during robot execution to prevent file-lock exception errors.

---

### 5. Execution and Live Testing

You can test and run this long-running workflow using two different methods:

#### Option A: Running Locally via UiPath Studio (For Development and Testing)

1. Click **Run** or **Debug** directly inside UiPath Studio.
2. The robot will parse the `sms_inbound.txt` file, detect the invalid 5-digit ID (`12345`), and mark the status as `Awaiting Human Action`.
3. The workflow will upload the task to your Cloud Action Center and pause itself, letting your Studio execution enter a `Suspended` state.
4. Go to your **UiPath Action Center** web portal, open **My Tasks**, assign the task to yourself, correct the ID to a 16-digit number, and click **Submit**.
5. Your local Studio instance will instantly wake up and finish writing the successful transaction back to the Excel ledger.

#### Option B: Deploying and Running Directly from UiPath Orchestrator (For Production)

1. In UiPath Studio, click the **Publish** button at the top ribbon to package the project into a `.nupkg` file and upload it directly to your Orchestrator Tenant packages.
2. Log in to your **UiPath Automation Cloud** web portal, navigate to Orchestrator, go to your designated Modern Folder, and create a new **Process** using this package.
3. Ensure you have an **Unattended Robot** or a connected Attendant User Session provisioned to that folder to provision a runtime machine.
4. Click **Start a Job** from the Orchestrator Automations dashboard to trigger the process remotely.
5. When the invalid ID triggers the validation failure, the job status inside Orchestrator will automatically change to `Suspended`, freeing up your machine's runtime license completely.
6. Once the operator resolves the form task in the Action Center portal, Orchestrator will automatically allocate an available robot machine to pick up the job, resume the execution state, and finalize the ledger without any manual system restarts.

---

## Project Structure

```
sms-link-global/
├── project.json
├── Main.xaml                           <- Entry point workflow
├── FormIDNUMBERVerification.form.json  <- Action Center form definition
├── README.md
└── LICENSE
```

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
