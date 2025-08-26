# Reality Check: Benchmarking Real Agents vs LLM Wrappers 🚦

[![Releases](https://img.shields.io/badge/Releases-download-blue?logo=github)](https://github.com/Luxes11yy/ai-agents-reality-check/releases) [![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/) [![License](https://img.shields.io/badge/License-Apache%202.0-blue)](LICENSE)

![Agent vs LLM banner](https://images.unsplash.com/photo-1555066931-4365d14bab8c?ixlib=rb-1.2.1&auto=format&fit=crop&w=1650&q=80)

Table of contents
- Overview
- Key goals
- Core benchmarks
- Design and methodology
- Metrics and statistical validation
- How to run (download and execute)
- Example outputs
- Reproducibility and artifacts
- Repository layout
- Contributing
- License and contact

Overview
This repository hosts a mathematical benchmark that exposes the performance gap between real agents and LLM-based agent wrappers. It evaluates agent systems across multiple dimensions: stress testing, network resilience, ensemble coordination, and failure analysis. The tests isolate architectural overhead from real-system behavior and supply statistical validation and reproducible workflows.

Key goals
- Measure end-to-end agent performance under realistic load.
- Separate architectural theater (wrappers, proxies, orchestration layers) from core agent behavior.
- Validate results with statistical tests and confidence intervals.
- Provide simple, reproducible scripts and datasets for repeatable experiments.
- Surface failure modes and recovery costs across designs.

Core benchmarks
- Stress Test: scaled concurrency, message bursts, and queue backpressure.
- Network Resilience: packet loss, latency spikes, and partition tolerance.
- Ensemble Coordination: task distribution, leader election, and consensus latency.
- Failure Analysis: fault injection, degraded-mode behavior, and mean time to recovery (MTTR).
- Baseline Comparison: pure agent runtime vs. LLM wrapper plus orchestration.

Design and methodology
- Controlled variables: task size, network conditions, input entropy, agent memory limits.
- Factorial design: vary architecture, ensemble size, network profile, and failure rate.
- Synthetic workloads: deterministic workloads with seeded randomness to allow repeat runs.
- Realistic scenarios: simulated API calls, external tool invocations, and multi-hop planning.
- Isolation: run agent cores in minimal environments to measure pure execution costs. Add wrappers to expose orchestration overhead.

Benchmark flow
1. Provision environment with controlled resources.
2. Start agents and wrappers according to the experiment matrix.
3. Drive workload at target QPS and concurrency.
4. Inject faults per schedule (latency, packet loss, node crash).
5. Collect traces, logs, and metrics.
6. Run post-hoc statistical analysis and generate visualizations.

Metrics and statistical validation
- Latency: median, p50, p90, p99, and distribution plots.
- Throughput: sustained requests per second under stable and stressed loads.
- Availability: successful task completion rate and timeouts.
- Recovery: MTTR and success after recovery window.
- Coordination overhead: extra time and messages required for consensus or task handoff.
- Resource cost: CPU, memory, and network per completed task.

Statistical approach
- Bootstrap for confidence intervals on non-normal metrics.
- Permutation tests to compare architectures without distributional assumptions.
- False discovery control using Benjamini-Hochberg for multi-metric comparisons.
- Effect sizes reported alongside p-values to show practical relevance.
- Repeat runs and seed control to reduce run-to-run variance.

How to run (download and execute)
Download the release bundle from the Releases page and execute the runner included in the archive:
- Visit the releases page: https://github.com/Luxes11yy/ai-agents-reality-check/releases
- Download the release asset named ai-agents-reality-check-<version>.tar.gz or .zip.
- Extract and run the benchmark runner:

  ```
  tar xzf ai-agents-reality-check-1.0.0.tar.gz
  cd ai-agents-reality-check-1.0.0
  ./run_benchmark.sh --profile default --runs 10
  ```

The release contains:
- run_benchmark.sh — orchestrates experiments and collects artifacts.
- env.example — sample environment variables.
- configs/ — experiment matrices and network profiles.
- tools/ — helper scripts for fault injection and metric collection.

If the release link does not respond, check the Releases section on the repository homepage and download the latest bundle there.

Example experiment matrix
- Architectures: core-agent, llm-wrapper-single, llm-wrapper-ensemble
- Ensemble sizes: 1, 3, 5
- Network profiles: ideal, jittered, lossy
- Fault rates: 0%, 1%, 5%
- Runs per cell: 10 seeds

Example commands
- Run a single scenario locally:
  ```
  ./run_benchmark.sh --arch llm-wrapper-ensemble --ensemble-size 3 --network lossy --runs 5
  ```
- Run full grid (use a machine with at least 8 vCPUs):
  ```
  ./run_benchmark.sh --profile grid --runs 10
  ```

Outputs and artifacts
- results/<timestamp>/metrics.csv — per-run metrics and tags.
- results/<timestamp>/traces/ — raw traces and logs.
- results/<timestamp>/plots/ — latency histograms, throughput over time, and ensemble coordination heatmaps.
- analysis/report.html — automated report with tables, charts, and statistical summaries.

Reproducibility and reproducible research
- All experiments log seeds, environment variables, and package versions.
- The benchmark uses deterministic workload generators when given a seed.
- The analysis pipeline reproduces figures from raw CSVs using the same scripts.
- The release archive includes a Dockerfile and a Conda environment file for pinned dependencies.
- Use the included run_repro.sh to re-create experiments from raw artifacts.

Statistical validation examples
- Bootstrap: compute 95% CI on median latency by resampling runs.
- Permutation: compare two architectures by shuffling labels and computing p-value for difference in medians.
- Multiple comparisons: apply Benjamini-Hochberg across metric family (latency, throughput, availability).
- Report: show CI, p-value, and Cohen’s d for each comparison.

Visualizations
- Latency violin plots per architecture and network profile.
- Throughput time-series with shaded CI bands.
- Ensemble message counts and handoff graphs.
- Recovery waterfall showing time to steady-state after fault injection.

Best practices
- Measure both wall-clock and CPU time to capture orchestration costs.
- Capture raw logs and structured metrics (JSON) for trace correlation.
- Avoid single-run claims; use multiple seeds and report variance.
- Present both relative and absolute results so readers can judge trade-offs.

Repository layout
- /benchmarks — benchmark definitions and drivers
- /configs — experiment matrices and profiles
- /tools — fault injection, network emulator, metric collectors
- /analysis — analysis scripts and Jupyter notebooks
- /results — generated artifacts (ignored in git)
- run_benchmark.sh — main runner
- run_repro.sh — replay and reproduce experiments
- Dockerfile — container to run experiments
- environment.yml — pinned Python deps
- LICENSE — Apache 2.0 license

Contributing
- Open issues to propose new benchmarks, add network profiles, or report bugs.
- Fork, implement a change, and submit a pull request with tests and a small dataset.
- Use the issue templates and PR checklist.
- Add reproducible examples that show a new failure mode or defense.

Roadmap
- Add hardware-in-the-loop tests for edge devices.
- Add more orchestration tiers and multi-cloud scenarios.
- Integrate with Prometheus and Grafana for live dashboards.
- Add a standard CSV schema for third-party benchmarks to contribute data.

Community and citation
- Cite this repository by linking to the release version used. Use the release archive for reproducible references.
- If you publish results derived from this benchmark, include the experiment matrix and scripts used.

License and contact
- Licensed under Apache 2.0. See LICENSE for details.
- For questions or to report issues, open an issue on the repo or contact the maintainers via the Issues page.

Releases
- Download and execute the release bundle from the Releases page: https://github.com/Luxes11yy/ai-agents-reality-check/releases

