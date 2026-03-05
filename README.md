# 🤖 RepoSwarm

<p align="center">
  <img src="assets/banner-nodes.png" alt="RepoSwarm - Multi-Repo Architecture Discovery" width="100%">
</p>

<p align="center">
  <strong>AI-powered multi-repo architecture discovery platform</strong>
</p>

<p align="center">
  <a href="https://github.com/reposwarm/reposwarm/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=for-the-badge" alt="License"></a>
  <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/Python-3.12+-3776AB.svg?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.12+"></a>
  <a href="https://www.youtube.com/watch?v=rOMf9xvpgtc"><img src="https://img.shields.io/badge/Demo-YouTube-red.svg?style=for-the-badge&logo=youtube" alt="YouTube Demo"></a>
</p>

<p align="center">
  <a href="#quick-start">Quick Start</a> •
  <a href="#what-is-reposwarm">What Is It</a> •
  <a href="#how-it-works">How It Works</a> •
  <a href="#ecosystem">Ecosystem</a> •
  <a href="#contributing">Contributing</a>
</p>

> **📦 Previously `loki-bedlam/repo-swarm`. Moved to the [RepoSwarm organization](https://github.com/reposwarm). Old URLs redirect automatically.**

---

## Quick Start

Install the CLI and bootstrap everything in one shot:

```bash
curl -fsSL https://raw.githubusercontent.com/reposwarm/reposwarm-cli/main/install.sh | sh
```

Then:

```bash
reposwarm new --local             # Bootstrap everything: Temporal, API, Worker, UI
reposwarm doctor                  # Full health check
reposwarm repos add my-app --url https://github.com/org/my-app
reposwarm investigate my-app      # Run your first investigation
reposwarm dashboard               # Watch it work
```

That's it. The CLI handles setup, configuration, investigation, diagnostics, and results — all from a single binary.

👉 **Full CLI docs:** [**reposwarm-cli**](https://github.com/reposwarm/reposwarm-cli)

---

## What is RepoSwarm?

RepoSwarm automatically analyzes your entire codebase portfolio and generates standardized architecture documentation. Point it at your GitHub repos (or CodeCommit, GitLab, Azure DevOps, Bitbucket) and get back clean, structured `.arch.md` files — perfect as AI agent context, onboarding docs, or architecture reviews.

<p align="center">
  <img src="assets/banner-swarm.png" alt="RepoSwarm Agents" width="80%">
</p>

### ✨ Key Features

- 🔍 **AI-Powered Analysis** — Uses Claude to deeply understand codebases
- 📝 **Standardized Output** — Generates consistent `.arch.md` architecture files
- 🔄 **Incremental Updates** — Only re-analyzes repos with new commits
- 💾 **Smart Caching** — DynamoDB or file-based caching avoids redundant work
- 🎯 **Type-Aware Prompts** — Specialized analysis for backend, frontend, mobile, infra, and libraries
- 📦 **Results Hub** — All architecture docs committed to a centralized repository
- 🔌 **Multi-Provider** — Anthropic API, Amazon Bedrock, or LiteLLM proxy
- 🌐 **Multi-Git** — GitHub, GitLab, CodeCommit, Azure DevOps, Bitbucket

### 📋 See It In Action

Check out [RepoSwarm's self-analysis](https://github.com/royosherove/repo-swarm-sample-results-hub/blob/main/repo-swarm.arch.md) — RepoSwarm investigating its own codebase!

🎬 **Architecture Overview (click to play)**

[![▶ Watch on YouTube](https://img.youtube.com/vi/rOMf9xvpgtc/hqdefault.jpg)](https://www.youtube.com/watch?v=rOMf9xvpgtc)

---

## How It Works

```mermaid
graph TB
    A[Your Repositories] -->|New commits detected| B[RepoSwarm Engine]
    B -->|Temporal Workflow| C[Clone & Analyze]
    C -->|Claude AI Analysis| D[Generate .arch.md]
    D -->|Cache in DynamoDB| E[Store Results]
    E -->|Auto-commit| F[Results Hub Repository]
    F -->|Query with AI| G[Reports & Insights]
    
    style A fill:#e1f5fe,color:#000
    style B fill:#fff3e0,color:#000
    style F fill:#f3e5f5,color:#000
    style G fill:#e8f5e8,color:#000
```

**Pipeline:** Cache check → Clone → Type detection → Structure analysis → Prompt selection → AI analysis → Store results → Cleanup

---

## Common Workflows

### Run an Investigation

```bash
reposwarm repos add my-app --url https://github.com/org/my-app
reposwarm investigate my-app
reposwarm wf progress
reposwarm results read my-app
```

### Investigate All Repos in Parallel

```bash
reposwarm investigate --all --parallel 3
reposwarm dashboard
```

### Diagnose Issues

```bash
reposwarm doctor                       # Full health check
reposwarm errors                       # Stalls + failures
reposwarm wf retry <workflow-id>       # Re-run a failed investigation
```

### Search Across All Architecture Docs

```bash
reposwarm results search "authentication"
reposwarm results diff repo-a repo-b
reposwarm results export --all -d ./docs
```

👉 **Full command reference:** [**reposwarm-cli README**](https://github.com/reposwarm/reposwarm-cli)

---

## Configuration

### LLM Provider

```bash
reposwarm config provider setup        # Interactive (Anthropic, Bedrock, LiteLLM)
```

### Git Provider

```bash
reposwarm config git setup             # Interactive (GitHub, GitLab, CodeCommit, Azure, Bitbucket)
```

### Adding Repositories

```bash
reposwarm repos add my-backend --url https://github.com/org/my-backend
reposwarm repos add my-frontend --url https://github.com/org/my-frontend
reposwarm repos discover               # Auto-discover from CodeCommit
```

Or edit `prompts/repos.json` directly:

```json
{
  "repositories": {
    "my-backend": {
      "url": "https://github.com/org/my-backend",
      "type": "backend",
      "description": "Main API service"
    }
  }
}
```

### Analysis Prompt Types

| Type | Focus | Prompts |
|------|-------|---------|
| 🔧 **Backend** | APIs, databases, services | [`prompts/backend/`](prompts/backend/) |
| 🎨 **Frontend** | Components, routing, state | [`prompts/frontend/`](prompts/frontend/) |
| 📱 **Mobile** | UI, device features, offline | [`prompts/mobile/`](prompts/mobile/) |
| 📚 **Libraries** | API surface, internals | [`prompts/libraries/`](prompts/libraries/) |
| ☁️ **Infrastructure** | Resources, deployments | [`prompts/infra-as-code/`](prompts/infra-as-code/) |
| 🔗 **Shared** | Security, auth, monitoring | [`prompts/shared/`](prompts/shared/) |

---

## Ecosystem

```mermaid
graph TB
    User([👤 User / AI Agent])
    
    CLI["⌨️ reposwarm-cli<br/><i>Go binary — setup, investigate,<br/>diagnose, results</i>"]
    API["🔌 reposwarm-api<br/><i>REST API server — repos,<br/>workflows, prompts</i>"]
    UI["📊 reposwarm-ui<br/><i>Next.js dashboard —<br/>browse investigations</i>"]
    Core["🤖 reposwarm<br/><i>Core engine — Temporal<br/>workflows + Claude analysis</i>"]
    
    Temporal[("⏱ Temporal<br/>Orchestration")]
    Git[("🌐 Git Providers<br/>GitHub · GitLab · CodeCommit<br/>Bitbucket · Azure DevOps")]
    LLM[("🧠 LLM Provider<br/>Anthropic · Bedrock · LiteLLM")]
    Hub[("📋 Results Hub<br/>.arch.md files")]
    
    User -->|commands| CLI
    User -->|browser| UI
    CLI -->|HTTP| API
    UI -->|HTTP| API
    API -->|workflows| Temporal
    Temporal -->|tasks| Core
    Core -->|clone repos| Git
    Core -->|analyze code| LLM
    Core -->|commit results| Hub
    CLI -.->|"manages<br/>(start/stop/upgrade)"| API
    CLI -.->|manages| UI
    CLI -.->|manages| Core
    
    style CLI fill:#2d3748,color:#fff,stroke:#4299e1
    style API fill:#2d3748,color:#fff,stroke:#48bb78
    style UI fill:#2d3748,color:#fff,stroke:#ed8936
    style Core fill:#2d3748,color:#fff,stroke:#9f7aea
    style Temporal fill:#1a202c,color:#fff,stroke:#718096
    style Git fill:#1a202c,color:#fff,stroke:#718096
    style LLM fill:#1a202c,color:#fff,stroke:#718096
    style Hub fill:#1a202c,color:#fff,stroke:#718096
```

| Project | Description | Install |
|---------|-------------|---------|
| ⌨️ [**reposwarm-cli**](https://github.com/reposwarm/reposwarm-cli) | CLI — setup, investigate, diagnose, results | `curl -fsSL .../install.sh \| sh` |
| 🔌 [**reposwarm-api**](https://github.com/reposwarm/reposwarm-api) | REST API server for repos, workflows, prompts | Managed by CLI |
| 📊 [**reposwarm-ui**](https://github.com/reposwarm/reposwarm-ui) | Next.js dashboard for browsing investigations | Managed by CLI |
| 🤖 **reposwarm** (this repo) | Core engine — Temporal workflows + analysis | Managed by CLI |
| 📋 [**sample-results-hub**](https://github.com/royosherove/repo-swarm-sample-results-hub) | Example output — generated `.arch.md` files | — |

---

## Project Structure

```
reposwarm/
├── prompts/                 # AI analysis prompts by repo type
│   ├── backend/            # API, database, service prompts
│   ├── frontend/           # UI, component, routing prompts
│   ├── mobile/             # Mobile app specific prompts
│   ├── libraries/          # Library/API prompts
│   ├── infra-as-code/      # Infrastructure prompts
│   ├── shared/             # Cross-cutting concerns
│   └── repos.json          # Repository configuration
├── src/
│   ├── investigator/       # Core analysis engine
│   │   └── core/          # Main analysis logic
│   ├── workflows/          # Temporal workflow definitions
│   ├── activities/         # Temporal activity implementations
│   ├── models/             # Data models and schemas
│   └── utils/              # Storage adapters and utilities
├── tests/                  # Unit and integration tests
└── temp/                   # Generated .arch.md files (local dev)
```

---

## Credits

RepoSwarm was born out of a hackathon at [Verbit](https://verbit.ai/), built by:
- [Moshe](https://github.com/mosher)
- [Idan](https://github.com/Idandos)
- [Roy](https://github.com/royosherove)

---

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes and add tests
4. Submit a pull request

---

## License

This project is licensed under the [Apache License 2.0](LICENSE).
