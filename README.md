````markdown
# engineering-team-using-crewai

A demonstration project showing how to orchestrate a multi-agent “engineering team” using the [crewai](https://pypi.org/project/crewai/) framework. This repo defines a crew of specialized agents (engineering lead, backend engineer, frontend engineer, test engineer) and a set of tasks (design, code, frontend, test) which are executed sequentially to satisfy a simple account-management requirements specification.

---

## Table of Contents

- [Project Overview](#project-overview)  
- [Features](#features)  
- [Architecture & Design](#architecture--design)  
- [Getting Started](#getting-started)  
  - [Prerequisites](#prerequisites)  
  - [Installation](#installation)  
- [Configuration](#configuration)  
  - [`agents.yaml`](#agentsyaml)  
  - [`tasks.yaml`](#tasksyaml)  
- [Usage](#usage)  
- [Project Structure](#project-structure)  
- [Contributing](#contributing)  
- [License](#license)  
- [Contact](#contact)  

---

## Project Overview

This repository shows how to leverage the `crewai` library to coordinate multiple LLM-driven “agents” working together on software engineering tasks. In this example, the crew is given a set of requirements for an account management system (create accounts, deposit/withdraw funds, track share transactions, compute P&L, etc.) and each agent contributes their specialty to fulfill and test those requirements.

---

## Features

- **Multi-Agent Orchestration**  
  - Engineering Lead  
  - Backend Engineer (with safe code execution)  
  - Frontend Engineer  
  - Test Engineer (with safe code execution)  
- **Configurable Agents & Tasks**  
  - Agents and tasks driven by external YAML configs  
- **Sequential Process**  
  - Tasks executed in defined order (design → code → frontend → test)  
- **Safe Code Execution**  
  - Backend & Test agents run code in Docker sandbox  
- **Extensible**  
  - Easily add new agents, tasks, or change execution mode  

---

## Architecture & Design

1. **Crew Base Class**  
   - Defined in `engineering_team/crew.py` using the `@CrewBase` decorator.  
   - Specifies how agents and tasks are instantiated.  

2. **Agents**  
   - Each agent is a `crewai.Agent` configured via `config/agents.yaml`.  
   - Example parameters:  
     - `allow_code_execution`  
     - `code_execution_mode` (e.g., `"safe"`)  
     - `max_execution_time` & `max_retry_limit`  

3. **Tasks**  
   - Defined in `config/tasks.yaml`, each mapped to a `crewai.Task`.  
   - Examples: `design_task`, `code_task`, `frontend_task`, `test_task`.  

4. **Process Orchestration**  
   - The crew is created with `Process.sequential`, ensuring tasks run in order.  

---

## Getting Started

### Prerequisites

- Python ≥ 3.8  
- Docker (for safe code execution mode)  
- Virtual environment tool (optional, but recommended)  

### Installation

1. **Clone the repository**  
   ```bash
   git clone https://github.com/Rishi-Kora/engineering-team-using-crewai.git
   cd engineering-team-using-crewai
````

2. **Create & activate a virtual environment**

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

   > *Note:* `requirements.txt` should include at least:
   >
   > ```text
   > crewai
   > pysbd
   > ```

---

## Configuration

### `config/agents.yaml`

Defines each agent’s LLM, prompts, and execution settings. Example structure:

```yaml
engineering_lead:
  model: "gpt-4"
  temperature: 0.7
backend_engineer:
  model: "gpt-4"
  temperature: 0.3
  allow_code_execution: true
  code_execution_mode: "safe"
  max_execution_time: 500
  max_retry_limit: 3
frontend_engineer:
  model: "gpt-4"
  temperature: 0.7
test_engineer:
  model: "gpt-4"
  allow_code_execution: true
  code_execution_mode: "safe"
  max_execution_time: 500
  max_retry_limit: 3
```

### `config/tasks.yaml`

Maps tasks to requirement prompts or task-specific instructions:

```yaml
design_task:
  description: "Draft system design: classes, modules, and data flow."
  prompt_template: "Given requirements, produce a high-level architecture."
code_task:
  description: "Implement the core `Account` class and trading logic."
frontend_task:
  description: "Build interfaces or CLI for account operations."
test_task:
  description: "Write test cases to validate deposit, withdrawal, transactions, and P&L."
```

---

## Usage

Run the end-to-end crew process via the main script:

```bash
python main.py
```

* **Output**

  * All agent outputs (design docs, generated code, test results) will be written into the `output/` directory.
* **Custom Inputs**

  * You can override `requirements`, `module_name`, or `class_name` in `main.py` to point to your own spec.

---

## Project Structure

```
.
├── config/
│   ├── agents.yaml         # Agent configurations
│   └── tasks.yaml          # Task definitions
├── engineering_team/
│   └── crew.py             # CrewBase class defining agents & tasks
├── output/                 # Generated artifacts (designs, code, tests)
├── main.py                 # Entry point to kickoff the crew
├── requirements.txt        # Python dependencies
└── README.md               # This file
```

---

## Contributing

Contributions are welcome! Please follow these guidelines:

1. **Fork** the repo.
2. **Create a feature branch** (`git checkout -b feature/YourFeature`).
3. **Commit changes** (`git commit -m "Add your feature"`).
4. **Push** to your fork (`git push origin feature/YourFeature`).
5. **Open a Pull Request** describing your changes.

Please ensure all new code is covered by appropriate tests and follows PEP 8 style.

---

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

## Contact

* **Author**: Rishi Kora
* **GitHub**: [Rishi-Kora](https://github.com/Rishi-Kora)
* **Project URL**: [https://github.com/Rishi-Kora/engineering-team-using-crewai](https://github.com/Rishi-Kora/engineering-team-using-crewai)

```
```
