# SMS-Link AI: Humanitarian Registration via Intelligent SMS Automation

## Project Overview
SMS-Link AI is an automated registration platform designed to accelerate humanitarian aid distribution. The solution captures incoming text message registration data, validates payload structures, and utilizes an intelligent Human-in-the-Loop architecture to dynamically handle data discrepancies without process interruption.

## The Problem
Every year, millions of refugees and displaced persons attempt to register for critical humanitarian aid programs including food, shelter, medical care, and legal protection[cite: 2]. However, a significant digital divide exists: the vast majority of displaced populations lack smartphones or stable internet access, relying instead on basic mobile devices capable only of sending SMS[cite: 2]. 

Conversely, aid organizations operate on digital systems that require structured, validated data[cite: 2]. The gap between a raw incoming SMS and a structured database record frequently causes registrations to fail due to mistyped identity numbers or rigid ingestion systems that cannot handle incomplete data formats gracefully[cite: 2]. Consequently, vulnerable populations are often rejected by the very systems designed to assist them[cite: 2].

## The Solution
SMS-Link AI bridges the gap between basic SMS inputs and fully validated humanitarian registration databases with zero manual data re-entry and a resilient human-in-the-loop fallback mechanism[cite: 2].

The workflow operates through the following stages:
1. **Data Ingestion:** The system automatically reads and parses inbound registration text messages containing registrant data such as Name, National ID, and Category[cite: 2].
2. **Format Validation:** The process executes structural integrity checks to verify that the National ID format adheres to the 16-digit standard[cite: 2].
3. **Instant Ingestion:** Valid registrations are automatically committed to the registry ledger and marked as Success without requiring human intervention[cite: 2].
4. **Human-in-the-Loop Escalation:** If an invalid ID format is detected, the workflow automatically generates an exception task within the UiPath Action Center[cite: 2]. The process suspends execution to conserve runtime resources, waits for an agent to update the credentials via a structured form, and resumes immediately upon submission without restarting the global workflow[cite: 2].
5. **Audit Trails:** Every execution and status change is logged into a structured database registry for comprehensive tracking and compliance[cite: 2].
6. **Idempotency Guardrails:** Built-in validation structures ensure duplicate entries are filtered out and not reprocessed across multiple system runs[cite: 2].

## Business Impact

| Metric | Before SMS-Link AI | After SMS-Link AI |
| :--- | :--- | :--- |
| **Registration Channel** | Internet-only portal[cite: 2] | SMS (compatible with any mobile device)[cite: 2] |
| **Data Validation** | Manual and error-prone[cite: 2] | Automated and instant integrity validation[cite: 2] |
| **Invalid ID Handling** | Automatic rejection[cite: 2] | Escalation to human agent with real-time resolution[cite: 2] |
| **Processing Speed** | Hours to days[cite: 2] | Minutes[cite: 2] |
| **Duplicate Registrations** | Common operational issue[cite: 2] | Prevented by design through idempotent controls[cite: 2] |
| **Audit Trail** | Inconsistent or manual[cite: 2] | Complete structured logs on every run[cite: 2] |

* **Refugees and Displaced Persons:** Enabled to register securely from any location with a basic cellular signal[cite: 2].
* **Aid Organizations:** Eliminates manual administrative entry, decreases operational error rates, and scales processing capacity[cite: 2].
* **Human Agents:** Relieved from routine validations, allowing operational focus to shift exclusively to edge cases requiring genuine human judgment[cite: 2].

## Agent Architecture
This solution utilizes a **hybrid architecture combining coding agents and human orchestration**. The core workflow, parsing layers, and database synchronization are executed via coding agents built using standard UiPath Studio workflow logic[cite: 2]. Exception handling and dynamic data corrections are orchestrated through low-code interactive forms within the UiPath Action Center, allowing a seamless blend of automated task execution and human oversight[cite: 2].

## UiPath Components Used
* **UiPath Studio:** For core sequential workflow design and validation development[cite: 2].
* **UiPath Action Center & Form Tasks:** For human-in-the-loop task generation, interface assignment, and data verification[cite: 2].
* **UiPath Persistence Activities:** Specifically utilizing `WaitForFormTaskAndResume` to suspend and resume the robot lifecycle[cite: 2].
* **UiPath Orchestrator / Maestro:** For centralized process deployment, environment asset management, and transaction logging[cite: 2].
* **UiPath Excel Activities:** For data synchronization, spreadsheet updates, and ledger maintenance with header preservation[cite: 2].

## Prerequisites
* UiPath Studio 2024.x or newer (Windows / Cross-Platform compatibility).
* `UiPath.Persistence.Activities` package enabled within the project dependencies.
* Access to a UiPath Automation Cloud tenant with Action Center services provisioned.

## Setup and Installation Instructions
1. **Clone the Repository:**
   ```bash
   git clone [https://github.com/YOUR_USERNAME/sms-link-global.git](https://github.com/YOUR_USERNAME/sms-link-global.git)
