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

This solution explicitly utilizes **both Low-code Agents and Coded Agents** within its lifecycle to achieve dynamic case management:

```
┌─────────────────────────────────────────────────────────────┐
│                     SMS-Link AI Global                      │
│                                                             │
│  Inbound SMS                                                │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐    Valid ID        ┌──────────────────┐    │
│  │   Parser &  │ ─────────────────► │  Auto-Commit to  │    │
│  │  Validator  │                    │  SMS_Gateway.xlsx│    │
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

- **Low-code Agents** — Leveraged for core high-level orchestration, long-running asynchronous persistence states (WaitForFormTaskAndResume), and conditional exception handling. It smoothly coordinates human handoffs by rendering dynamic validation interfaces within the UiPath Action Center web portal.
- **Coded Agents** — Applied through advanced backend logic expressions, multi-point Regular Expression (Regex) patterns to enforce strict structural parsing constraints, and native dataset filtering. Additionally, an external Claude CLI Agent is integrated into the pre-deployment lifecycle to execute automated static code reviews and structural optimization on the main workflow files.

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

> **Payload Format — Single vs. Multi-line:**
> The robot reads `sms_inbound.txt` **line by line**. Each line represents one independent registration record. The delimiter between fields within a single record is `#`.
>
> **Single registration:**
> ```
> REG#Mamat Alkatiri#12345#Social
> ```
>
> **Batch registrations (multiple records in one file):**
> ```
> REG#Mamat Alkatiri#12345#Social
> REG#Siti Rahayu#1234567890123456#Medical
> REG#John Doe#9876543210987654#Food
> ```
>
> The robot processes each line sequentially. Invalid IDs on any line trigger an individual Action Center task per record — other valid records in the same file are processed immediately without waiting for human resolution.

---

### 4. Setup the Master Ledger (Excel Gateway)

Before running the workflow, you must manually provide the data transaction ledger template:

1. Create a blank Excel file named **`SMS_Gateway.xlsx`**
2. Create a sheet inside named **`SMS_Sheet`**
3. Place the file at the following path:

```
C:\SMS_Link_AI_Global_Orchestration\SMS_Gateway.xlsx
```

> **Required Headers:** Before running the robot, ensure row 1 of `SMS_Sheet` contains the following column headers exactly as written:
>
> | A1 | B1 | C1 | D1 |
> |:---|:---|:---|:---|
> | `Nama` | `National ID` | `Category` | `Status` |
>
> If these headers are absent or misspelled, UiPath's Append Range activity will throw a header mismatch error. The robot does not auto-create headers.

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

> **Production SMS Ingestion Architecture:**
> In a live deployment, the `sms_inbound.txt` file is not populated manually. Instead, an upstream integration layer receives inbound SMS messages and writes them to the input folder automatically. Common integration patterns include:
>
> - **Twilio Webhook** — A Twilio phone number forwards incoming SMS payloads to a lightweight middleware (e.g., an Azure Function or AWS Lambda), which appends each message as a new line to `sms_inbound.txt` on the production machine.
> - **Email-to-SMS Gateway** — Carriers such as Telkomsel (Indonesia) and GSMA-compatible providers support email-to-SMS bridging. Inbound SMS are delivered as emails; a UiPath Email Activities workflow monitors the inbox and writes parsed bodies to the input folder.
> - **Direct API Integration** — For aid organizations with existing CRM or field-collection platforms (e.g., KoboToolbox, ODK), a webhook posts to a REST endpoint that writes to the input folder on a scheduled polling interval.
>
> The core UiPath automation is intentionally decoupled from the ingestion channel — any method that reliably writes a `REG#Name#ID#Category` line to `sms_inbound.txt` is compatible with this solution.

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
