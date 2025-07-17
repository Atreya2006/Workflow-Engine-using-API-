# ⚙️ Workflow Engine using API

A minimal, extensible **state-machine (workflow) engine** built with **.NET 8** and **ASP.NET Core Minimal API**.  
It allows you to define, instantiate, and operate custom workflows through RESTful API calls — ideal for prototyping automation logic, integrations, and interview-ready backend design.

---

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet)
![Platform](https://img.shields.io/badge/platform-ASP.NET%20Core-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-beta-orange)

---

##  Features

-  Define workflows as collections of **named states** and **permitted actions**
-  Start new workflow instances based on defined templates
-  Execute actions that **transition workflow states**
-  Inspect workflow definitions and instance history
-  In-memory storage (no external database) for lightning-fast prototyping
-  Auto-generated **Swagger** UI for easy API exploration

---

##  Table of Contents

- [ Architecture & Project Structure](#-architecture--project-structure)
- [ Quick Start](#-quick-start)
- [ API Endpoints](#-api-endpoints)
- [ Example API Usage](#-example-api-usage)
- [ Validation Rules](#-validation-rules)
- [ Assumptions & Limitations](#-assumptions--limitations)
- [ For Interviewers / Reviewers](#-for-interviewers--reviewers)

---

##  Architecture & Project Structure

```
WorkflowEngine/
├── Models/
│   ├── State.cs
│   ├── Action.cs
│   ├── WorkflowDefinition.cs
│   ├── WorkflowInstance.cs
│   └── ApiResponse.cs
├── Services/
│   ├── WorkflowDefinitionService.cs
│   └── WorkflowInstanceService.cs
├── Persistence/
│   └── InMemoryStore.cs
├── Program.cs                # Minimal API endpoint wiring
└── README.md
```

- **Models** — Entities representing states, actions, workflows, instances, and responses.
- **Services** — Core business logic for defining and executing workflows.
- **Persistence** — Simple in-memory store for definitions and instance tracking.
- **Program.cs** — Contains all the API routing using .NET 8 Minimal API style.

---

##  Quick Start

###  Clone the Repository

```bash
git clone https://github.com/yourusername/workflow-engine-demo.git
cd workflow-engine-demo/WorkflowEngine
```

###  Build and Run the API

```bash
dotnet build
dotnet run
```

###  Access the Swagger UI

Visit:  
[https://localhost:7000/swagger](https://localhost:7000/swagger)  
Use the auto-generated Swagger interface to explore and test the API.

---

## 📡 API Endpoints

| Method | Endpoint                          | Description                           |
|--------|-----------------------------------|---------------------------------------|
| POST   | `/workflows`                      | Create a new workflow definition      |
| GET    | `/workflows`                      | List all workflow definitions         |
| GET    | `/workflows/{definitionId}`       | Get a workflow definition by ID       |
| POST   | `/instances`                      | Start a new workflow instance         |
| GET    | `/instances`                      | List all workflow instances           |
| GET    | `/instances/{instanceId}`         | Get a workflow instance & its history |
| POST   | `/instances/{instanceId}/actions` | Execute an action on an instance      |

---

##  Example API Usage

### 1. Create a Workflow Definition

```http
POST /workflows
Content-Type: application/json
```

```json
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
```

---

### 2. Start a Workflow Instance

```http
POST /instances
Content-Type: application/json
```

```json
{
  "definitionId": "approval"
}
```

---

### 3. Execute an Action

```http
POST /instances/{instanceId}/actions
Content-Type: application/json
```

```json
{
  "actionId": "submit"
}
```

---

### 4. Get Workflow Instance & History

```http
GET /instances/{instanceId}
```

---

##  Validation Rules

### Workflow Definitions

1. Must have **exactly one** state with `"isInitial": true`.
2. Each `id` (for states and actions) must be **unique**.
3. Actions must reference **valid and enabled** states for transitions.

### Workflow Instances

Actions can be executed only if:

- The action is **enabled**
- The instance is currently in one of the action’s `fromStates`
- The **target state exists** and is **enabled**
- The instance is **not already in a final state**

All invalid operations return **descriptive error messages**.

---

##  Assumptions & Limitations

1. Only **one initial state** allowed per workflow.
2. All data is stored **in memory** — restarting the app clears all definitions and instances.
3. No **authentication/authorization** — kept simple for demo & testing purposes.
4. Not suitable for **production use** — meant for **learning**, **testing**, and **interviews**.

---

##  For Interviewers / Reviewers

This project demonstrates:

- Clean code using ASP.NET Core Minimal API.
- Domain modeling for finite state machines.
- Service-oriented architecture with separation of concerns.
- Fully working validation and transitions.
- No third-party libraries — pure .NET 8 SDK.

---


##  Contributing

Pull requests are welcome! 
To contribute:

```bash
# Fork this repo
# Create a feature branch
# Make your changes
# Submit a PR 
```

---



