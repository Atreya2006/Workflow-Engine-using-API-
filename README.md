# Workflow-Engine-using-API-

#  Workflow Engine using API

A minimal, flexible state-machine (workflow) API built with **.NET 8** and **ASP.NET Core Minimal API**. This engine enables developers and teams to define, instantiate, and manage custom workflows entirely through API calls — ideal for rapid prototyping, testing automation logic, and integration experiments.

---

##  Features

- Define workflows as collections of named states and permitted actions.
- Start new workflow instances based on any workflow definition.
- Execute actions on workflow instances with all transitions validated.
- Inspect workflow definitions, states, actions, and instance history.
- In-memory storage (no database dependency) — super fast for testing and prototyping.

---

##  Table of Contents

- [ Quick Start](#-quick-start)
- [ API Endpoints](#-api-endpoints)
- [ API Example Usage](#-api-example-usage)
- [ Validation Rules](#-validation-rules)
- [ Project Structure](#-project-structure)
- [ Assumptions & Limitations](#-assumptions--limitations)
- [ For Interviewers / Reviewers](#-for-interviewers--reviewers)
- [ Ideas for Future Work](#-ideas-for-future-work)

---

## 🔧 Quick Start

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/en-us/download)
- [Optional] Homebrew (for installation on Mac)

### Steps

1. **Clone the repository:**

```bash
git clone https://github.com/yourusername/workflow-engine-demo.git
cd workflow-engine-demo/WorkflowEngine

2.**Build and run the API:**

dotnet build
dotnet run

3.**Access the Swagger UI (API Explorer):**

(https://localhost:7000/swagger)

API Endpoints

| Method | Endpoint                          | Description                           |
| ------ | --------------------------------- | ------------------------------------- |
| POST   | `/workflows`                      | Create a new workflow definition      |
| GET    | `/workflows`                      | List all workflow definitions         |
| GET    | `/workflows/{definitionId}`       | Get a workflow definition by ID       |
| POST   | `/instances`                      | Start a new workflow instance         |
| GET    | `/instances`                      | List all workflow instances           |
| GET    | `/instances/{instanceId}`         | Get a workflow instance & its history |
| POST   | `/instances/{instanceId}/actions` | Execute an action on an instance      |

Example API Usage

1. Create a Workflow Definition

h

POST /workflows
Content-Type: application/json

json

{
  "id": "approval",
  "name": "Approval Workflow",
  "states": [
    { "id": "draft", "name": "Draft", "isInitial": true, "isFinal": false, "enabled": true },
    { "id": "review", "name": "Review", "isInitial": false, "isFinal": false, "enabled": true },
    { "id": "approved", "name": "Approved", "isInitial": false, "isFinal": true, "enabled": true }
  ],
  "actions": [
    { "id": "submit", "name": "Submit", "enabled": true, "fromStates": ["draft"], "toState": "review" },
    { "id": "approve", "name": "Approve", "enabled": true, "fromStates": ["review"], "toState": "approved" }
  ]
}

2. Start a Workflow Instance

http

POST /instances
Content-Type: application/json

json

{
  "definitionId": "approval"
}

3. Execute an Action

h

POST /instances/{instanceId}/actions
Content-Type: application/json

json

{
  "actionId": "submit"
}

4. Get Workflow Instance & History

http

GET /instances/{instanceId}

**Validation Rules**

Workflow Definitions

1)Must have exactly one state with "isInitial": true.

2)Each id (state/action) must be unique.

3)Action transitions must point to valid and enabled states.

Workflow Instances

Actions can be executed only if:

1)The action is enabled.

2)The instance is in one of the action's fromStates.

3)The target state exists and is enabled.

4)The instance is not already in a final state.

5)Invalid operations return clear, descriptive errors.

Assumptions & Limitations

1)Only one initial state allowed per workflow.

2)All workflow data is stored in memory (no persistence after restart).

3)No authentication/authorization — this is by design for simplicity.

4)Not meant for production usage — ideal for learning, testing, and interviews.





