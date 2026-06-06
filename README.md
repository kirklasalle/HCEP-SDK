# HCEP SDK — Human Communication Eye Protocol SDK

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Unity%20%7C%20Unreal%20%7C%20Python%20%7C%20.NET-lightgrey.svg)]()

Welcome to the official **HCEP SDK** repository. This toolkit provides multi-platform developer integrations, agentic tools, and virtual avatar control scripts to connect to the **Human Communication Eye Protocol (HCEP)** desktop runtime server.

HCEP processes live visual and audio streams in real-time, calculates pupil/gaze parallax corrections, and decodes the unspoken language of eye contact into five cognitive-emotional modes: **Logic, Affect, Spirit, Heart,** and **Think**.

---

## Repository Structure

```
hcep-sdk/
├── docs/                      # Technical specification & API documents
│   ├── index.html             # Interactive True Gaze™ Parallax Simulator
│   ├── PRD.md                 # Product Requirements Document
│   ├── DEVELOPER_GUIDE.md     # Engineering & Architectural Reference
│   └── USER_GUIDE.md          # End-User Installation & Dashboard Setup
├── sdk/
│   ├── python/                # Python LLM / Agent integrations
│   │   ├── hcep_langchain.py  # LangChain BaseTool implementation
│   │   └── hcep_llamaindex.py # LlamaIndex FunctionTool integration
│   ├── csharp/                # C# Semantic Kernel plugins
│   │   └── HcepSemanticKernelPlugin.cs
│   ├── unity/                 # Unity Gaze Controller component
│   │   └── HcepGazeController.cs
│   └── unreal/                # Unreal Engine C++ Component
│       ├── HcepGazeController.h
│       └── HcepGazeController.cpp
└── LICENSE                    # Open-source MIT License
```

---

## Quick Start & Integration Guides

### 1. Model Context Protocol (MCP) Integration
HCEP acts as a native Model Context Protocol (MCP) server. You can register it with Claude Desktop or any MCP client by pointing to its `/mcp` POST endpoint:

* **Endpoint:** `POST http://localhost:5000/mcp`
* **JSON-RPC Tools Discovery:**
  ```json
  {
    "jsonrpc": "2.0",
    "method": "tools/list",
    "id": 1
  }
  ```

### 2. Python LangChain Agent
Add HCEP as a live perception tool for your LangChain agents:
```python
from sdk.python.hcep_langchain import HcepStateTool
from langchain.agents import initialize_agent, AgentType

tool = HcepStateTool(base_url="http://localhost:5000")
agent = initialize_agent(
    tools=[tool],
    llm=chat_model,
    agent=AgentType.STRUCTURED_CHAT_ZERO_SHOT_REACT_DESCRIPTION
)

# Agent queries HCEP gaze/cognitive modes on-demand
agent.run("Tell me if the user is paying attention or internally processing a thought.")
```

### 3. Unreal Engine Avatar Control (C++)
Attach the `UHcepGazeController` component to your Unreal Character or Pawn to drive face rig skeletal transforms via HCEP WebSocket stream:

```cpp
// AMyAvatarCharacter.cpp
void AMyAvatarCharacter::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // Fetch smoothly interpolated gaze target coordinates
    FVector EyeLookDirection = GazeControllerComponent->TargetGazeDirection;
    FRotator HeadPoseRotation = GazeControllerComponent->TargetHeadRotation;

    // Apply rotations directly to your character's skeleton
    LeftEyeMesh->SetRelativeRotation(EyeLookDirection.Rotation());
    RightEyeMesh->SetRelativeRotation(EyeLookDirection.Rotation());
}
```

---

## Documentation & Interactive Demos

To test the spatial gaze geometry locally without launching the app, open the **True Gaze™ Parallax Simulator**:
1. Open the [docs/index.html](docs/index.html) file in any standard web browser.
2. Drag your mouse to simulate face movements.
3. Toggle between **Raw Coordinates** and **Parallax Correction On** to see the gaze error rate drop down to sub-degree thresholds.

For detailed guidelines, read the [User Guide](docs/USER_GUIDE.md) and [Developer Guide](docs/DEVELOPER_GUIDE.md).

---

## Licensing & Disclaimers

*   **HCEP SDK (this repository):** All integration wrappers, Unity components, Unreal controllers, and Python scripts are licensed under the open-source [MIT License](LICENSE).
*   **HCEP Core Engine (compiled runtime):** The main HCEP background application, facial feature classification neural weights, and spatial solver DLLs are proprietary software owned by Kirk LaSalle and are governed by the commercial/proprietary **HCEP End User License Agreement** included in the binary download.

*Copyright © 2026 Kirk LaSalle. All rights reserved.*
