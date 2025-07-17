# Workflow-Engine-using-API-

Workflow Engine
A minimal, flexible state-machine (workflow) API built with .NET 8 and ASP.NET Core Minimal API. This engine allows developers and teams to define, instantiate, and manage custom workflows purely through API calls, making process automation experiments and integrations easy.

🚀 Features
Define workflows as collections of named states and permitted actions

Start new workflow instances based on any workflow definition

Execute actions on workflow instances with all transitions validated

Inspect workflow definitions, all states & actions, and the step-by-step instance history

In-memory storage (no database dependency) — super fast for testing and prototyping

📚 Table of Contents
Quick Start

API Endpoints

API Example Usage

Validation Rules

Project Structure

Assumptions & Limitations

For Interviewers / Reviewers

Ideas for Future Work

🔧 Quick Start
Prerequisites
.NET 8 SDK (install here)

[Optional] Homebrew for installation on Mac

Steps
Clone the repository

text
git clone https://github.com/yourusername/workflow-engine-demo.git
cd workflow-engine-demo/WorkflowEngine
Build and run the API

text
dotnet build
dotnet run
By default, the API is available at:

text
https://localhost:7000/swagger
Use the auto-generated Swagger UI for quick API exploration and testing.

📦 API Endpoints
Method	Endpoint	Description
POST	/workflows	Create a new workflow definition
GET	/workflows	List all workflow definitions
GET	/workflows/{definitionId}	Get a workflow definition
POST	/instances	Start a new workflow instance
GET	/instances	List all workflow instances
GET	/instances/{instanceId}	Get a workflow instance & history
POST	/instances/{instanceId}/actions	Execute an action on an instance
📝 API Example Usage
1. Create a Workflow Definition
Request

text
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
Request

text
POST /instances
Content-Type: application/json
json
{
  "definitionId": "approval"
}
3. Execute an Action
Request

text
POST /instances/{instanceId}/actions
Content-Type: application/json
json
{
  "actionId": "submit"
}
4. Inspect Workflow Instance
Request

text
GET /instances/{instanceId}
✅ Validation Rules
Workflow Definition

Each state and action must have unique id.

Workflow must have exactly one state with isInitial = true.

Each action:

Must reference existing state IDs in fromStates and toState.

Workflow Instance

Actions can only be taken if:

The action is enabled,

The action exists in the workflow definition,

Instance’s current state is among the action’s fromStates,

The target state exists and is enabled,

The instance is not already in a final state.

Errors are returned with helpful messages on all invalid operations.

🏗 Project Structure
text
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
├── Program.cs            # All API endpoint wiring
└── README.md
Models: Data objects for states, actions, definitions, and instances

Services: Business logic and validation

Persistence: Simple in-memory store (no external DB)

Program.cs: Adds endpoints using ASP.NET Core Minimal API pattern
