# Performance Engineering Interview — Master Guide (3 Months)

**Purpose:** One consolidated study and interview-prep plan for all roles discussed in your job search.  
**Duration:** 12 weeks (3 months) — ~12–15 hours/week recommended.  
**Companion doc:** `PERFORMANCE-ENGINEERING-PRACTICE-WORKBOOK.md` (exercises, mocks, rubrics).

---

## Table of Contents

1. [Role Catalog & Interview Focus](#1-role-catalog--interview-focus)
2. [Skills Matrix (What to Emphasize Per Role)](#2-skills-matrix)
3. [Unified Technical Foundations](#3-unified-technical-foundations)
4. [12-Week Calendar Overview](#4-12-week-calendar-overview)
5. [Month 1 — Foundations & Tooling](#5-month-1--foundations--tooling)
6. [Month 2 — Role Tracks & Deep Dives](#6-month-2--role-tracks--deep-dives)
7. [Month 3 — Mocks, Stories & Interview Sprint](#7-month-3--mocks-stories--interview-sprint)
8. [STAR Story Bank Template](#8-star-story-bank-template)
9. [System Design Prompts](#9-system-design-prompts)
10. [Behavioral & Leadership Question Bank](#10-behavioral--leadership-question-bank)
11. [Day-Before & Day-Of Checklist](#11-day-before--day-of-checklist)
12. [Resources & Reading List](#12-resources--reading-list)

---

## 1. Role Catalog & Interview Focus

| ID | Role Archetype | Level | Primary Interview Style |
|----|----------------|-------|-------------------------|
| R1 | **Airbnb-style** — Perf strategy, profiling platform, fleet SLOs | Staff+ IC (12+ yr) | System design + org influence + business metrics |
| R2 | **Hardware platform** — Benchmark porting, silicon readiness | Senior IC | Experiments, roofline, HW/SW boundary debug |
| R3 | **Senior platform perf** — Prod Python/GPU, observability, regressions | Senior IC (7+ yr) | Prod RCA, layered stack, SLO definition |
| R4 | **BambooHR** — Lead Perf Eng + SDET, unified QE | Eng Manager | Leadership + dual technical credibility |
| R5 | **NVIDIA ML infra** — Researcher efficiency, NCCL, fleet GPU util | IC (~5+ yr infra) | NSight, distributed training, fleet patterns |
| R6 | **AV perf engineer** — C++/Python, heterogeneous, tooling roadmap | IC lead (~3+ yr) | C++ profiling, real-time, cross-team initiatives |
| R7 | **Enterprise perf QA** — NeoLoad/JMeter, J2EE, multi-tier reports | Senior IC / lead | Load test planning, executive reports |
| R8 | **NeoLoad perf tester** — Modern stack, CI/CD, Datadog/Dynatrace | Senior IC | NeoLoad depth, SLA→tests, APM correlation |
| R8b | *(Same family as R7; modern tooling emphasis)* | | |
| R9 | **NVIDIA HPC customer** — Fortran/MPI/OpenACC/CUDA, compilers | Senior IC (8+ yr) | HPC porting, roofline, customer communication |

### Role decision tree (which track to emphasize)

```
GPU + NCCL + ML researchers + fleet?           → R5
Fortran/MPI/OpenACC + HPC customers?           → R9
C++ + AV + safety-adjacent tooling?            → R6
NeoLoad + SLA + CI gates + APM?                → R7 / R8
Lead team (Perf + SDET) + Playwright?          → R4
Staff strategy + company-wide profiling?       → R1
Benchmark new hardware with arch teams?        → R2
Prod Python/GPU + observability?               → R3
```

**Rule:** Never interview for R9 the day after prepping only NeoLoad (R8). Block **2 prep days per role family** before each loop.

---

## 2. Skills Matrix

| Skill | R1 | R2 | R3 | R4 | R5 | R6 | R7/8 | R9 |
|-------|:--:|:--:|:--:|:--:|:--:|:--:|:----:|:--:|
| SLO/SLA/SLI & error budgets | ●●● | ● | ●●● | ●●● | ●● | ●● | ●●● | ● |
| System design (platform) | ●●● | ●● | ●● | ●● | ●● | ●● | ● | ● |
| Leadership / influence | ●●● | ● | ●● | ●●● | ●● | ●● | ●● | ●● |
| Load/stress/soak testing | ●● | ●● | ●● | ●●● | ● | ● | ●●● | ● |
| NeoLoad / JMeter | — | — | — | ●● | — | — | ●●● | — |
| Test automation (Playwright) | — | — | — | ●●● | — | — | ● | — |
| Production RCA | ●●● | ●● | ●●● | ●● | ●●● | ●●● | ●● | ●● |
| CPU/kernel/JVM/runtime | ●●● | ●●● | ●●● | ●● | ●● | ●●● | ●● | ●●● |
| GPU / CUDA | ●● | ●●● | ●●● | — | ●●● | ●●● | — | ●●● |
| NCCL / distributed ML training | ● | ● | ●● | — | ●●● | ● | — | ● |
| MPI / OpenMP / OpenACC / Fortran | — | ●● | — | — | — | — | — | ●●● |
| Observability (metrics/traces/profiles) | ●●● | ●● | ●●● | ●●● | ●●● | ●●● | ●●● | ●● |
| CI/CD quality gates | ●● | ● | ●● | ●●● | ●● | ●● | ●●● | ● |
| Hardware benchmarking | ●● | ●●● | ● | — | ●● | ●● | — | ●● |
| C++ optimization | ●● | ●● | ●● | — | ● | ●●● | — | ●●● |
| Python perf | ●● | ● | ●●● | ●● | ●●● | ●● | ●● | ● |
| Business / cost narrative | ●●● | ● | ●● | ●● | ●●● | ● | ●● | ● |

Legend: ●●● = must master for loop | ●● = strong | ● = awareness

---

## 3. Unified Technical Foundations

Study these once in Month 1; reuse vocabulary in every interview.

### 3.1 Latency & statistics

- **Percentiles:** p50, p95, p99, p999 — why averages lie.
- **Coordinated omission** and why load generators can lie.
- **Little’s Law:** L = λW (concurrency, arrival rate, wait time).
- **Utilization vs saturation:** resource busy % vs queueing delay.
- **Strong vs weak scaling** (HPC and distributed systems).

### 3.2 SLO framework

```
SLI (measure) → SLO (internal target) → SLA (contract)
Error budget = allowed unreliability before freeze/features tradeoff
```

**Example SLIs:** API availability, p99 latency, throughput RPS, GPU util efficiency.

### 3.3 Investigation playbook (memorize)

1. Scope — what degraded, when, which cohort?
2. Correlate — deploy, config, traffic, dependency, hardware pool.
3. Measure — RED/USE metrics, traces, profiles.
4. Hypothesize — one bottleneck at a time.
5. Experiment — canary, replay load, isolate tier.
6. Fix + guardrail — CI gate, alert, runbook, baseline update.

### 3.4 Bottleneck layers (always walk top to bottom)

Application → runtime (GC/GIL) → middleware → DB → cache → network → OS → hardware.

### 3.5 Load test types

| Type | Goal |
|------|------|
| Load | SLA at expected peak + headroom |
| Stress | Find breaking point |
| Soak/endurance | Leaks, drift over hours |
| Spike | Sudden burst + recovery |
| Regression | Same script vs baseline build |

### 3.6 Profiling tool families

| Layer | Tools |
|-------|--------|
| CPU (Linux) | perf, flame graphs, eBPF (bpftrace) |
| JVM | async-profiler, JFR |
| Python | py-spy, cProfile, scalene |
| GPU | Nsight Systems, Nsight Compute, nvprof legacy |
| Distributed | OpenTelemetry, Jaeger, Datadog APM |
| ML training | NCCL logs, PyTorch profiler, memory snapshot |
| HPC | TAU, IPM, Darshan, MPI timelines |
| Continuous | Parca, Pyroscope, Datadog CP |

### 3.7 Roofline model (GPU & CPU)

```
Attainable GFLOP/s = min(peak compute, bandwidth × arithmetic intensity)
```

Know how to classify memory-bound vs compute-bound kernels.

---

## 4. 12-Week Calendar Overview

| Week | Theme | Hours (guide) |
|------|--------|---------------|
| 1 | Metrics, SLOs, investigation playbook | 12–15 |
| 2 | Profiling fundamentals (CPU + traces) | 12–15 |
| 3 | Load testing methodology + NeoLoad concepts | 12–15 |
| 4 | Observability + CI/CD gates | 12–15 |
| 5 | GPU + CUDA + Nsight intro | 15–18 |
| 6 | Distributed systems perf (tail latency, caches) | 12–15 |
| 7 | **Track A:** ML (NCCL, training) OR **Track B:** HPC (MPI/OpenACC) | 15–18 |
| 8 | **Track C:** C++/AV OR **Track D:** Manager/leadership | 15–18 |
| 9 | System design workshops (2 designs) | 15–18 |
| 10 | STAR stories + behavioral intensive | 12–15 |
| 11 | Full mock interviews (3–4 roles) | 15–20 |
| 12 | Weak-area sprint + interview logistics | 12–15 |

---

## 5. Month 1 — Foundations & Tooling

### Week 1: Metrics, SLOs, Playbook

**Read (4–5 hrs)**
- Google SRE Book: Chapters on SLIs/SLOs, monitoring.
- Brendan Gregg: USE method blog post; RED method summary.

**Do (6–8 hrs)**
- Workbook: W1 exercises (SLO design, investigation drill).
- Write your investigation playbook on one page (from memory).
- Draft 3 SLIs + SLOs for a fictional e-commerce API.

**Deliverable:** 1-page SLO doc + investigation checklist printed.

### Week 2: CPU Profiling & Tracing

**Read (3–4 hrs)**
- Flame graph article (Brendan Gregg).
- OpenTelemetry tracing concepts.

**Do (8–10 hrs)**
- Run `perf` or py-spy on a sample program; export flame graph.
- Trace a 3-service docker-compose with OTel or Jaeger demo.
- Workbook: W2 exercises.

**Deliverable:** Screenshot + 5-bullet analysis of one flame graph.

### Week 3: Load Testing & NeoLoad

**Read (3 hrs)**
- NeoLoad documentation: populations, scenarios, correlation.
- Gatling/JMeter comparison notes (interview talking points).

**Do (8–10 hrs)**
- If NeoLoad license: build login → browse → checkout scenario.
- If no license: design on paper + JMeter equivalent for practice.
- Workbook: W3 load test plan for HR SaaS (BambooHR-style).

**Deliverable:** Load test plan template filled for one product.

### Week 4: Observability & CI/CD

**Read (3 hrs)**
- Datadog/Dynatrace: distributed tracing, service map docs (one vendor deep).

**Do (8–10 hrs)**
- Sketch CI pipeline with perf stages (PR smoke, nightly, pre-release).
- Design regression policy: when to block release.
- Workbook: W4 exercises.

**Deliverable:** CI/CD perf integration diagram + gate thresholds table.

### Month 1 checkpoint

- [ ] Can explain SLI/SLO/error budget in 3 minutes
- [ ] Can run investigation playbook without notes
- [ ] Can name 4 load test types and when to use each
- [ ] Have 1 flame graph analysis example ready to discuss

---

## 6. Month 2 — Role Tracks & Deep Dives

Pick **primary track** (most interviews) + **secondary track** (backup roles).

### Week 5: GPU & Nsight (R5, R6, R3, R9 partially)

**Topics**
- CUDA execution model, occupancy, memory coalescing.
- Nsight Systems timeline reading.
- Nsight Compute roofline & memory throughput.

**Do**
- Profile a small PyTorch training step or matrix multiply kernel.
- Workbook: W5 GPU debug checklist drill.

**Deliverable:** "GPU util 40%" debug narrative (written, 1 page).

### Week 6: Distributed Systems Perf (R1, R3, R8)

**Topics**
- Tail latency, fan-out, caching, DB pools, backpressure.
- Capacity planning, headroom, autoscaling pitfalls.

**Do**
- Whiteboard: p99 regression after deploy (full RCA).
- System design: profiling platform (45 min timed) — R1.

**Deliverable:** One completed system design doc (see Section 9).

### Week 7: Split track

#### Track A — ML Infra (R5)
- NCCL: AllReduce, topology, debugging hangs.
- DDP/FSDP overview, dataloader bottlenecks.
- Fleet utilization metrics and actions.

#### Track B — HPC (R9)
- MPI scaling, OpenMP threading, OpenACC data regions.
- Stencil/roofline whiteboard.
- Fortran/C array layout and halo exchange.

**Do:** Workbook W7A or W7B (not both in same week if time-boxed; skim the other).

### Week 8: Split track

#### Track C — C++ / AV (R6)
- C++ allocators, cache, lock contention.
- Perf CI for C++ monorepo design.
- Safety-adjacent language (determinism, WCET awareness).

#### Track D — Management (R4)
- Team roadmap, hiring, perf+automation unified strategy.
- Playwright vs Selenium, test pyramid, flake policy.
- 5 leadership STAR stories finalized.

#### Track E — Enterprise load test (R7/R8) — if primary
- NeoLoad correlation deep dive.
- J2EE/multi-tier monitoring lab on paper.
- Sample executive perf report (Workbook template).

### Month 2 checkpoint

- [ ] One full system design written and rehearsed aloud
- [ ] Primary track: 10 technical questions answered aloud (record yourself)
- [ ] Secondary track: 5 questions minimum
- [ ] GPU or HPC debug checklist memorized (role-dependent)

---

## 7. Month 3 — Mocks, Stories & Interview Sprint

### Week 9: System design intensive

| Day | Activity |
|-----|----------|
| Mon | Design: Company-wide continuous profiling (R1) — 45 min + 15 review |
| Tue | Design: Perf observability for AV software (R6) |
| Wed | Design: CI perf gates for microservices (R8) |
| Thu | Design: Fleet GPU utilization program (R5) |
| Fri | Design: HPC porting workflow for customer (R9) — narrative, not just boxes |

Use Workbook rubrics to self-score each design.

### Week 10: Behavioral & STAR

- Finalize **8 STAR stories** (see Section 8).
- Record 3 answers on phone; listen for clarity, metrics, "I" vs "we."
- Practice: influence without authority, conflict, failure, mentorship.

### Week 11: Full mock interviews

Schedule **4 mocks** (peer, mentor, or self-timed):

| Mock | Role focus | Duration |
|------|------------|----------|
| 1 | Your #1 target role — full loop simulation | 2–3 hrs |
| 2 | Technical deep dive only | 1.5 hrs |
| 3 | Behavioral + leadership | 1 hr |
| 4 | Different role family (prevent cross-contamination) | 2 hrs |

After each mock: gap list → 2 days remediation.

### Week 12: Weak-area sprint

- Re-take lowest-scoring Workbook sections.
- Company research (2 hrs per company: blog, stack, recent incidents/news).
- Logistics: questions to ask, elevator pitch per role, salary band notes.

### Month 3 checkpoint

- [ ] 8 STAR stories with metrics, under 3 min each
- [ ] 2 system designs rehearsed aloud under 45 min
- [ ] 4 mocks completed with written feedback
- [ ] Role-specific elevator pitch (30 sec) memorized per active application

---

## 8. STAR Story Bank Template

Prepare **8 stories** covering these themes. Map each story to multiple roles.

| # | Theme | Roles using it |
|---|--------|------------------|
| S1 | High-impact prod regression → fix → guardrail | R1, R3, R5, R6, R8 |
| S2 | Built tooling/platform others adopted | R1, R5, R6, R8 |
| S3 | Influenced teams without authority | R1, R4, R5, R6, R9 |
| S4 | Load test / SLA program with business outcome | R4, R7, R8 |
| S5 | GPU or HPC optimization with measured speedup | R2, R5, R6, R9 |
| S6 | Build vs buy or vendor evaluation | R1, R4, R8 |
| S7 | Mentorship / growing an engineer | R1, R4, R6 |
| S8 | Failure, pushback, or wrong initial diagnosis | All |

### STAR template per story

```
Title:
Role(s):
Situation (2–3 sentences):
Task — your specific accountability:
Actions (3–5 bullets, "I" not "we"):
Results — numbers: latency %, cost $, GPU-hours, adoption %, incidents:
Learning (1 sentence, optional):
```

### Metrics cheat sheet (use real numbers from your career)

- Latency: p95 improved X% (before → after ms)
- Throughput: RPS or jobs/hour +Y%
- Cost: $/month infra saved, GPU-hours saved
- Reliability: incidents down N, SLO compliance %
- Adoption: N teams, M services on platform
- Quality: flake rate down, escape defects down
- People: team grew X→Y, promotion, retention

---

## 9. System Design Prompts

For each prompt: requirements → constraints → high-level diagram → data model → APIs → scaling → failure modes → rollout → metrics.

### R1 — Continuous profiling platform
- Requirements: all backend teams, CPU+memory, deploy correlation, low overhead.
- Deep dive: sampling, storage cost, PII in stacks, multi-tenant.

### R4 — Unified QE for multi-product SaaS
- Perf + automation + observability under one roadmap.
- CI stages, quality metrics, team structure.

### R5 — Researcher efficiency tooling
- Profile training jobs, recommend fixes, track fleet waste.
- Integration with scheduler, notebooks, PyTorch.

### R6 — AV perf CI & monitoring
- C++ binaries, heterogeneous targets, regression on perception latency.

### R8 — Perf testing in CI/CD
- Baselines, NeoLoad controller scaling, false positive handling.

### R9 — Customer HPC port engagement
- Workflow from baseline CPU → GPU port → validation → scaling study.

---

## 10. Behavioral & Leadership Question Bank

### Influence & strategy
1. How do you prioritize perf work across 100+ services?
2. Product wants features; you want gates — what do you do?
3. Long-term platform vs short-term hotfix — example.
4. First 90 days in [role]?

### Management (R4)
5. How do you unify perf engineers and SDETs on one roadmap?
6. Underperformer on your team — steps taken.
7. Hiring: what do you look for in a perf vs automation hire?
8. How do you measure your team's success?

### Customer / cross-functional (R9, R5, R7)
9. Customer/researcher disagrees with your recommendation.
10. Executive summary of a complex technical finding.

### Failure & learning
11. Tell me about a perf initiative that failed.
12. When you misidentified the bottleneck — what happened?

---

## 11. Day-Before & Day-Of Checklist

### Day before
- [ ] Re-read company blog / engineering posts (30 min)
- [ ] Rehearse 30-sec elevator pitch for **this** role only
- [ ] Review 3 STAR stories mapped to job description bullets
- [ ] Prepare 5 questions for interviewers (written)
- [ ] Test video/audio; quiet space; water, pen, paper
- [ ] Sleep 7+ hours

### Day of
- [ ] No new topics — light review of cheat sheet only
- [ ] Bring: investigation playbook, one metrics story, questions list
- [ ] Answer structure: clarify → structure → depth → tradeoffs → summary
- [ ] If stuck: think aloud, state assumptions, ask interviewer

### Post-interview (within 24 hrs)
- [ ] Write debrief: questions asked, gaps, stories that landed
- [ ] Thank-you note if appropriate (brief, specific)

---

## 12. Resources & Reading List

### Books
- *Systems Performance* — Brendan Gregg
- *Site Reliability Engineering* — Google
- *The Art of Capacity Planning* — Arlitt (legacy but useful concepts)

### Online
- NVIDIA Nsight documentation & GTC talks (GPU, HPC, ML)
- MLPerf rules and result summaries (benchmark literacy)
- OpenTelemetry docs
- NeoLoad learning center

### Practice environments
- Local: Docker compose microservices + load generator
- GPU: Google Colab or local NVIDIA GPU for Nsight labs
- HPC: OpenMPI + sample NAS Parallel Benchmarks (if pursuing R9)

---

## Quick Reference: Elevator Pitches by Role Family

**R1 Staff platform:** "I define performance strategy tied to SLOs and business outcomes, and build profiling platforms teams actually adopt."

**R4 Manager QE:** "I lead perf and automation engineers with one roadmap: load testing, observability, and CI gates that protect releases."

**R5 ML infra:** "I make researchers and clusters more efficient via end-to-end training/inference analysis, NCCL debugging, and fleet utilization programs."

**R7/R8 Load test:** "I turn SLAs into NeoLoad scenarios with solid correlation and data, analyze full-stack bottlenecks with APM, and integrate regression testing into CI."

**R9 HPC:** "I accelerate MPI/OpenMP scientific codes with OpenACC/CUDA, validate correctness, and feed compiler teams reproducible performance gaps."

---

*Last updated: May 2026. Pair with `PERFORMANCE-ENGINEERING-PRACTICE-WORKBOOK.md` for daily exercises.*
