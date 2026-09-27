# AI Governance Lab

## Bank pilot and open contribution roadmap

The [bank pilot plan](docs/bank-pilot-plan.md) assigns proposed work to the infrastructure and ABM fellows, defines validation gates, and prepares for a possible FINOS contribution. The [contributor architecture](docs/contributor-architecture.md) provides a working versioned workflow interface plus a target public/private architecture. Try `python -m examples.contributed_workflow` or `python -m governance_sim.workflows infrastructure --output outputs/infrastructure-run.json`. FINOS acceptance and bank deployment are future review decisions.

A reproducible research prototype for **AI-assisted loan application review**: six agent roles, three injected governance risks, four controls, deployment context, Monte Carlo comparison and an interactive Streamlit dashboard.

The **FINOS catalogue** tab also includes all **23 risks supplied in the reference attachment**, with searchable descriptions, an honest implementation coverage matrix and a persistent local evidence register. Catalogue coverage is not full simulation coverage: there are **3 simplified mechanisms, 2 partial proxies and 18 risks not simulated**. See [FINOS coverage and evidence tracking](docs/finos-coverage.md).

**All simulation inputs and outputs are synthetic.** This is an executable set of hypotheses, not an empirical bank model, compliance assessment or lending system. The simulation uses no LLM service, customer data or API keys. An optional **OpenAI analyst** can interpret aggregate results using your organization's API project; it makes a paid API request only when explicitly requested in the dashboard.

For **HomeWorld Educational Resources**, follow [OpenAI setup](docs/openai-setup.md). The account's organization, project and available credits must be verified in OpenAI Platform; they are not inferred from your Codex sign-in.

For the next infrastructure-focused phase, see the [12-week team work plan for Yang, Jiaman and Ning Wang](docs/team-work-plan.md).

## November 4 OSFF demo

Optional local AI explanations are available for every demo chart and category using Ollama. See [local AI setup and usage](docs/local-ai-demo.md). Choose a model in the demo sidebar and click an Explain button beside the visualization.

Open `http://127.0.0.1:8501/?demo=osff` after starting the dashboard. A guided infrastructure demonstration compares baseline, governed, overloaded, better-staffed and failover scenarios. Saved results, optional live local reruns and request-by-request traces use synthetic data and require no OpenAI calls.

See the [presenter guide and deadline plan](docs/osff-presenter-guide.md). The [self-contained offline backup](docs/osff-offline-demo.html) opens directly in a browser without a server. Regenerate both saved artifacts with `python scripts/build_osff_demo.py` after changing the infrastructure engine.

## Quick start

Python 3.10+ is required. From this folder on Windows:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e ".[dashboard]"
.\.venv\Scripts\python.exe -m streamlit run dashboard/app.py
```

On macOS/Linux, substitute `python3` for `py` and `.venv/bin/python` for the Windows virtual-environment executable. Open the local URL printed by Streamlit (normally http://localhost:8501).

The standard-library engine can run without installing any dependencies:

```powershell
py -m governance_sim.cli --repetitions 100 --applications 200 --stress --output outputs/demo
py -m unittest discover -s tests -v
```

The command compares four presets at normal and triple workload. It writes configuration snapshots, seeds, per-replication metrics, summary intervals, and one complete case/event trace per scenario. `--config scenarios/high_control.json` loads a base deployment; the comparison runner applies all four governance presets to that deployment. Use `simulate(Config.load(...))` in Python to run exactly one supplied configuration.

## Explore the dashboard

The Lab includes optional local AI explanations for its workflow, settings, charts, experiment results, case traces, catalogue coverage and methodology. Select an Ollama model in **Local AI explanations** in the sidebar, then click **Explain** beside a panel. See [setup and usage](docs/local-ai-demo.md). These explanations use saved synthetic evidence and never change simulation results.

1. Set deployment capacity, model assumptions, risk probabilities and custom controls in the sidebar.
2. Run the experiment. The default is 100 independent replications of 200 applications per scenario.
3. Compare harm, undetected incidents, processing time, human queues, escalations and cost. There is deliberately no composite governance score.
4. Use the case explorer to follow a specific injected risk through agents, controls and its outcome.
5. Download results and configurations from Method & exports. Changing inputs marks existing results as stale until rerun.

Replications range from 1 to 10,000. Large products of scenarios × replications × applications can take substantial time. Start with the default; use the CLI for long experiments. Event logs are retained only for individual traces, not every Monte Carlo replication.

## Project map

| Location | Purpose |
| --- | --- |
| `governance_sim/config.py` | Typed deployment, risk and control configuration, scenario presets |
| `governance_sim/agents.py` | Six agent roles and shared human capacity |
| `governance_sim/engine.py` | Case scheduling, risk/control mechanisms, event ledger and metrics |
| `governance_sim/experiments.py` | Paired repeated experiments and uncertainty summaries |
| `governance_sim/cli.py` | Batch execution and portable exports |
| `dashboard/app.py` | Configuration, scenario comparison and visual case explorer |
| `dashboard/catalogue_view.py` | FINOS risk browsing, coverage matrix and assessment forms |
| `governance_sim/data/finos_risks.json` | All 23 supplied risk entries and source provenance |
| `governance_sim/assessments.py` | Validated local assessment persistence and import/export |
| `scenarios/` | Editable JSON configurations (no YAML dependency) |
| `docs/` | Architecture, methodology, assumptions and demo instructions |
| `tests/` | Behavioral verification and optional dashboard smoke test |
| `notebooks/demo.ipynb` | Small programmatic experiment |

## What is modeled

- Customer → Intake AI → Credit AI → Compliance AI → Loan Officer → decision/audit, supported by a System Administrator.
- Data drift degrades recommendation accuracy. Authorization attempts can cause prohibited access. Goal misalignment biases recommendations toward approval.
- Least privilege and data access controls reduce exposure; optional human approval can suspend decisions; observability increases post-incident detection and shortens recovery.
- Human reviewer slots have availability times. Workload, attention, experience and confidence-weighted trust affect review effectiveness.
- Regional redundancy reduces simulated outage duration. This is a simplified per-case availability model.
- Same-seed comparisons use the same fixed random draw slots per application.

See [methodology](docs/methodology.md) and [limitations](docs/assumptions.md) before interpreting results. See [demo script](docs/demo.md) for a fellowship presentation.

## Development

Run `python -m unittest discover -s tests -v`. The dashboard test runs when optional dependencies are installed. CI tests Python 3.10 and 3.13. Contributions should include mechanism-level tests and document every changed behavioral assumption; see [CONTRIBUTING.md](CONTRIBUTING.md).

Released under the MIT license. The risk themes are FINOS-style conceptual inspiration; no official FINOS taxonomy mapping or endorsement is claimed.



# User Guidance
Your project currently has **two working experiences: a guided infrastructure demo and a configurable lending research workbench**. Both use synthetic data. A production tool connected to real organizational systems is a future development stage.

**1. Start and use the demo**

From PowerShell in the project folder:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e ".[dashboard]"
.\.venv\Scripts\python.exe -m streamlit run dashboard/app.py
```

If `.venv` already exists, skip the first command.

Open [the guided demo](http://127.0.0.1:8501/?demo=osff). It loads saved simulation results immediately.

Use this presentation sequence:

| Step | What to show | Main question |
|---|---|---|
| Workflow | Request → authorization → approval → deployment → monitoring | Where can governance intervene? |
| Baseline versus governed | Enable permissions, approval and monitoring | What changes when controls are introduced? |
| Governed versus increased demand | Triple arrival rate | Can the approval team handle demand? |
| Additional staffing | Increase reviewers from two to five | Does staffing relieve the bottleneck? |
| Failover | Enable resilience with staffing unchanged | What does failover improve? |
| Request trace | Follow one request across scenarios | Which events explain the outcome? |

Use the saved results for the main presentation. Try **Run live comparison** with 10 replications during practice.

If the app is unavailable, open the [offline demo](F:/OneDrive-Major/OneDrive/Documents/ChatGPT/AI-GOVERNANCE-SIMULATION/docs/osff-offline-demo.html). It requires no server.

The [presenter guide](F:/OneDrive-Major/OneDrive/Documents/ChatGPT/AI-GOVERNANCE-SIMULATION/docs/osff-presenter-guide.md) contains the full 15-minute script and assumptions.

**2. Use the current research tool**

Open [the research workbench](http://127.0.0.1:8501/).

This currently models **loan application review**, whereas the guided demo models **infrastructure deployment**. They are separate simulation engines.

A useful working session is:

1. Define a question, such as “How does human approval affect harmful outcomes and waiting time under increased demand?”
2. Set deployment, staffing, model and risk assumptions in the sidebar.
3. Start with 100 replications and 200 applications.
4. Run the experiment and inspect **Scenario comparison**.
5. Examine harm, undetected incidents, queues, processing time and cost together.
6. Use **Case explorer** to understand individual outcomes.
7. Export the configuration, experiment and metrics from **Method & exports**.

Use **FINOS catalogue** to browse the 23 listed risks and maintain local evidence assessments. Catalogue inclusion does not mean that a risk is simulated.

The optional OpenAI analyst interprets aggregate results; it is unnecessary for running either simulation.

**3. Develop the demo**

Keep improvements focused on a clear question, visible comparison and explainable trace.

| Change | Main file |
|---|---|
| Infrastructure assumptions, scheduling and controls | `governance_sim/infrastructure.py` |
| Demo presentation and interaction | `dashboard/osff_demo.py` |
| Saved results and offline backup generation | `scripts/build_osff_demo.py` |
| Infrastructure behavioral verification | `tests/test_infrastructure.py` |
| Lending mechanisms | `governance_sim/engine.py` |
| Lending configuration | `governance_sim/config.py` |
| Research workbench interface | `dashboard/app.py` |

For each mechanism change:

1. Write down the expected behavior and assumptions.
2. Implement the change and a focused behavioral test.
3. Update the relevant documentation.
4. Run tests and regenerate the demo artifacts:

```powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
.\.venv\Scripts\python.exe scripts/build_osff_demo.py
```

For example, adding reviewers should affect the approval queue; it should not silently change attack probability. Such checks make the results explainable.

**4. Develop a real organizational tool**

I recommend making the next release a **validated decision-support simulator**. Connecting it to operational systems should follow separately.

| Stage | Development work | Completion criterion |
|---|---|---|
| Configurable infrastructure workbench | Add infrastructure parameter forms, custom scenarios and exports | A user can create and reproduce an infrastructure experiment without editing Python |
| Evidence and calibration | Record parameter units, sources, uncertainty and versions | Every input is identified as measured, estimated or synthetic |
| Validation | Compare traces and outputs with expert expectations and permitted staging measurements | Document where the model agrees, disagrees and remains uncertain |
| Team application | Add authentication, access roles, durable storage and run history | Users can securely save, share and reconstruct experiments |
| Operational pilot | Introduce bounded integrations with staging systems | Integration behavior, permissions and recovery are demonstrated before broader deployment |

In everyday organizational use, the cycle would be: **select a workflow → enter evidence-backed assumptions → compare options → inspect uncertainty and traces → review findings with operators → save the decision and supporting evidence**.

If “real tool” means actually approving or executing deployments, that requires an additional execution layer: real identity and permission enforcement, approval workflows, integration adapters, audit records and rollback. The current simulated controls do not perform those actions.

Your existing [team work plan](F:/OneDrive-Major/OneDrive/Documents/ChatGPT/AI-GOVERNANCE-SIMULATION/docs/team-work-plan.md) already assigns domain work to Yang, methods to Jiaman and coordination to Ning. The immediate priority is to name the implementation owner and extend the working infrastructure engine into a configurable workbench. Some roadmap text still describes infrastructure features as future work, although the demo already implements them.
