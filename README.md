# Aspida — The MCP Pentester

Aspida is an AI-assisted security scanner for Model Context Protocol (MCP) servers. It helps identify suspicious tool metadata, potential tool poisoning, schema weaknesses, capability risks, and other security concerns through static analysis, authorized dynamic testing, and explainable risk scoring.

## Overview

As MCP adoption grows, tools exposed to AI assistants introduce new security considerations. Aspida provides a structured way to inspect MCP configurations and evaluate potential risks before integrating tools into an application.

The scanner combines deterministic security rules with optional machine-learning and language-model assistance to help developers investigate suspicious findings.

## Features

- **Static Security Analysis:** Inspects MCP tool definitions, metadata, input schemas, and declared capabilities.
- **Tool Poisoning Detection:** Identifies suspicious descriptions and potentially deceptive tool metadata.
- **Unicode Analysis:** Detects potentially misleading Unicode characters and related metadata anomalies.
- **Tool Name Analysis:** Examines tool names for suspicious patterns and naming risks.
- **Schema Validation:** Evaluates input schemas for potential weaknesses and inconsistencies.
- **Capability Analysis:** Inspects declared capabilities for potential security concerns.
- **Cross-Tool Collision Detection:** Identifies potential conflicts and suspicious overlaps between tools.
- **Authorized Dynamic Testing:** Supports bounded security probes against explicitly authorized live MCP servers.
- **Explainable Risk Scoring:** Combines findings into a deterministic risk score with transparent severity bands.
- **Advisory Machine Learning:** Uses a TF-IDF and class-balanced logistic regression pipeline to provide supplementary security signals.
- **Optional LLM Analysis:** Adds semantic analysis to assist with investigating potentially suspicious metadata.
- **Security Reports:** Supports JSON and PDF report generation.
- **Web Dashboard:** Provides a React-based interface for inspecting scan results and findings.

## How It Works

Aspida follows a structured security analysis workflow:

1. **Input:** Provide an MCP configuration file or JSON configuration, or supply a live server URL where supported.
2. **Static Analysis:** Evaluate tool descriptions, names, schemas, capabilities, Unicode patterns, and cross-tool relationships.
3. **Dynamic Testing:** When explicitly authorized and supported, run bounded probes against a live server.
4. **Finding Aggregation:** Combine applicable findings from the analysis pipeline.
5. **Risk Scoring:** Calculate a deterministic risk score using the configured scoring rules.
6. **Supplementary Analysis:** Use optional ML or LLM components to provide additional context without independently changing the deterministic score.
7. **Reporting:** Review findings through the dashboard and export available reports.

## Risk Scoring

Aspida uses the deterministic RISK-1 scoring system to produce a score from 0 to 100.

| Score | Severity |
|---|---|
| 0–24 | Low |
| 25–49 | Moderate |
| 50–74 | High |
| 75–100 | Critical |

The score is intended to support security triage. It should not be interpreted as a guarantee that a server is safe or vulnerable.

Machine-learning and optional LLM outputs are advisory signals; they do not independently determine the deterministic risk score.

## Architecture

The application is organized into a frontend, backend, and analysis pipeline.

### Frontend

- React
- Security dashboard
- Scan result visualization
- Finding inspection and report access

### Backend

- Python
- FastAPI
- SQLAlchemy with asynchronous database access
- SQLite
- API key authentication
- Rate limiting
- Scan orchestration and report generation

### Security Analysis

- Deterministic static-analysis rules
- Bounded dynamic security probes
- TF-IDF feature extraction
- Class-balanced logistic regression
- Optional LLM-assisted semantic analysis
- JSON and PDF reporting

## Security Design

Aspida incorporates safeguards intended to reduce risks associated with scanning and handling untrusted inputs.

- API key authentication
- Rate limiting
- SSRF protections, including DNS pinning
- Authorization checks for dynamic testing
- Bounded probes and safety limits
- Deterministic scoring for reproducible risk assessment

Dynamic testing should only be performed against systems for which explicit authorization has been obtained.

## Evaluation Snapshot

The reported evaluation included backend and frontend tests, end-to-end scenarios, and security-detection benchmarks.

- **Backend:** 154 tests were reported in the suite; 153 were run, with one live-DNS test skipped.
- **Frontend:** 90 tests.
- **End-to-end scenarios:** 8 of 8 passed.
- **Benign-tool benchmark:** After tuning, 1 false positive was reported across 153 benign samples.
- **Published attack examples:** 7 of 7 were detected in the evaluated in-sample set.
- **Independent attack samples:** The experimental ML component detected 5 of 10 samples and remains advisory.

These results describe the evaluated test environment and datasets. They should not be interpreted as proof of comprehensive vulnerability detection or production security.

## Limitations

Aspida is a security-assessment aid, not a replacement for a complete security review.

- Static analysis cannot reliably identify malicious behavior hidden inside a tool's implementation when the available metadata appears benign.
- The attack evaluation dataset is limited.
- The ML component is experimental and may miss attacks or generate false positives.
- Dynamic testing has been validated on controlled fixtures; behavior against arbitrary live servers may differ.
- SQLite and an in-process scanning runner may not be suitable for multi-tenant, high-concurrency production deployments without further engineering.
- A low risk score does not establish that an MCP server is safe.

## Getting Started

The commands below are a starting template. Confirm the actual directory structure, dependency files, and entry points in your repository before running them.

### Prerequisites

- Python compatible with the backend dependencies
- Node.js and npm compatible with the frontend dependencies
- Git

### Clone the Repository

Replace the example URL with your actual GitHub repository URL.

```bash
git clone https://github.com/YOUR_USERNAME/Aspida.git
cd Aspida
```

### Backend Setup

Navigate to the backend directory used by your repository.

```bash
cd backend
python -m venv .venv
```

Activate the virtual environment.

**Windows PowerShell:**

```powershell
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt:**

```bat
.venv\Scripts\activate.bat
```

**Linux/macOS:**

```bash
source .venv/bin/activate
```

Install dependencies using the dependency file provided by your repository. For example, if it contains `requirements.txt`:

```bash
pip install -r requirements.txt
```

Start the FastAPI application using the actual application module configured in your repository. A common example is:

```bash
uvicorn app.main:app --reload
```

The module path above is only an example; verify it against your project before running the command.

### Frontend Setup

Open a separate terminal and navigate to the frontend directory.

```bash
cd frontend
npm install
npm run dev
```

The directory names and scripts may differ in your repository. Check `package.json` and the actual folder structure if these commands do not match your setup.

## Responsible Use

Aspida is intended for authorized security testing, defensive research, and improving the security of MCP integrations.

- Only scan systems you own or have permission to assess.
- Obtain explicit authorization before running dynamic tests.
- Use the scanner's configured safety limits.
- Validate important findings manually before making security decisions.
- Do not treat automated results as a guarantee of security.

## Roadmap

Potential future improvements include:

- Expanded and independently validated MCP attack datasets
- Improved detection of indirect and cross-tool prompt-injection risks
- More extensive testing against diverse real-world MCP implementations
- Stronger evaluation of false positives and false negatives
- Improved isolation and scalability for dynamic scans
- Additional integrations for security workflows
- More comprehensive reproducibility and benchmarking tools

## Team

Aspida was developed through the combined contributions of the following team members.

- **Shubhankar Bhagat — Project Lead:** System architecture, backend pipeline, scoring engine, and evaluation.
- **Soham Paranjpe:** Presentation, architecture explanation, and documentation support.
- **Urvish Kharate:** Problem framing, presentation, and documentation support.
- **Rudra Waghmare:** Presentation, results discussion, and testing support.

## Acknowledgements

Aspida builds on the broader open-source ecosystem for Python, FastAPI, React, machine learning, and security research.

Thanks to the researchers and developers who publish MCP security guidance, vulnerability examples, and defensive testing methodologies that help improve the security of AI tool integrations.

## References and Further Reading

- [Model Context Protocol](https://modelcontextprotocol.io/)
- [OWASP](https://owasp.org/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [React Documentation](https://react.dev/)
- [scikit-learn Documentation](https://scikit-learn.org/stable/)

## License

No license has been specified here. If you intend to distribute Aspida as open-source software, add a `LICENSE` file and update this section accordingly.
