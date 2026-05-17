# Performance Engineering Interview — Practice Workbook

**Companion to:** `PERFORMANCE-ENGINEERING-INTERVIEW-MASTER-GUIDE.md`  
**How to use:** Complete exercises in order by week. Score yourself with rubrics. Repeat failed sections in Month 3.

**Legend**
- ⏱ = suggested time box
- ✍️ = written deliverable
- 🎤 = practice aloud (record optional)
- 💻 = hands-on on machine

---

## Table of Contents

1. [Weekly Exercise Index](#weekly-exercise-index)
2. [Week 1 — SLOs & Investigation](#week-1--slos--investigation)
3. [Week 2 — Profiling & Tracing](#week-2--profiling--tracing)
4. [Week 3 — Load Testing & NeoLoad](#week-3--load-testing--neoload)
5. [Week 4 — Observability & CI/CD](#week-4--observability--cicd)
6. [Week 5 — GPU & Nsight](#week-5--gpu--nsight)
7. [Week 6 — Distributed Systems](#week-6--distributed-systems)
8. [Week 7 — ML Track (R5)](#week-7--ml-track-r5)
9. [Week 7 — HPC Track (R9)](#week-7--hpc-track-r9)
10. [Week 8 — C++ AV Track (R6)](#week-8--c-av-track-r6)
11. [Week 8 — Management Track (R4)](#week-8--management-track-r4)
12. [Week 8 — Enterprise Load Test Track (R7/R8)](#week-8--enterprise-load-test-track-r7r8)
13. [System Design Practice Sets](#system-design-practice-sets)
14. [Mock Interview Scripts](#mock-interview-scripts)
15. [Flashcards & Drills](#flashcards--drills)
16. [Self-Scoring Rubrics](#self-scoring-rubrics)
17. [Personal Progress Log](#personal-progress-log)

---

## Weekly Exercise Index

| Week | Workbook section | Master Guide week |
|------|------------------|-------------------|
| 1 | W1 | Month 1 Week 1 |
| 2 | W2 | Month 1 Week 2 |
| 3 | W3 | Month 1 Week 3 |
| 4 | W4 | Month 1 Week 4 |
| 5 | W5 | Month 2 Week 5 |
| 6 | W6 | Month 2 Week 6 |
| 7 | W7A or W7B | Month 2 Week 7 |
| 8 | W8A/B/C | Month 2 Week 8 |
| 9–12 | Sections 13–14 | Month 3 |

---

## Week 1 — SLOs & Investigation

### Exercise 1.1 — Define SLOs (⏱ 45 min) ✍️

**Scenario:** Multi-tenant HR SaaS (BambooHR-style). API + web UI. Peak load during payroll week.

Define for each:
1. Three SLIs (with measurement method)
2. SLO targets (30-day window)
3. Error budget policy (what happens when burned)

| Service | SLI | SLO | Error budget action |
|---------|-----|-----|---------------------|
| Login API | | | |
| Employee search API | | | |
| Payroll submit API | | | |

**Model answer framework (self-check after):**
- SLIs measurable from client or server (availability, p99 latency, success rate).
- SLOs realistic (e.g. 99.9% availability = 43 min downtime/month).
- Error budget ties to release freeze or perf sprint.

---

### Exercise 1.2 — Investigation drill (⏱ 30 min) 🎤

**Scenario:** p99 latency for `POST /api/bookings` increased from 400ms to 1.2s over 2 hours. Error rate stable at 0.1%.

Walk through the 6-step playbook aloud in under 8 minutes. Then write your top 5 hypotheses ranked.

| Step | Your notes |
|------|------------|
| 1. Scope | |
| 2. Correlate | |
| 3. Measure | |
| 4. Hypothesize | |
| 5. Experiment | |
| 6. Fix + guardrail | |

---

### Exercise 1.3 — Metric interpretation (⏱ 20 min) ✍️

Given:

| Metric | Value | Normal |
|--------|-------|--------|
| CPU app tier | 25% | 60% |
| DB CPU | 95% | 50% |
| Connection pool | 98/100 used | 40% avg |
| p95 latency | 2s | 300ms |
| Cache hit rate | 99% | 99% |

**Questions:**
1. Most likely bottleneck tier?
2. Next 3 data points you would pull?
3. One short-term mitigation vs one structural fix?

---

### Exercise 1.4 — Flash drill (⏱ 10 min) 🎤

Define without notes: SLI, SLO, SLA, error budget, strong scaling, weak scaling, coordinated omission.

---

## Week 2 — Profiling & Tracing

### Exercise 2.1 — Flame graph analysis (⏱ 60 min) 💻 ✍️

**Task:** Profile any Python or Java program (or use sample below).

```python
# sample_hotpath.py — run under py-spy or cProfile
import hashlib, time
def work(n):
    s = 0
    for i in range(n):
        s += int(hashlib.md5(str(i).encode()).hexdigest(), 16)
    return s
for _ in range(50):
    work(200_000)
```

**Deliverable:**
1. Attach or sketch flame graph
2. Identify top 2 frames by inclusive time
3. Propose one optimization and expected impact

**Self-check rubric:** Named hot function, distinguished inclusive vs exclusive time, optimization tied to evidence.

---

### Exercise 2.2 — Distributed trace RCA (⏱ 45 min) ✍️

**Scenario trace ( fictional ):**
```
Total 980ms
  API gateway     12ms
  auth-service    45ms
  booking-service 910ms
    ├─ inventory-call  120ms
    ├─ pricing-call    650ms  ← errors: 0
    └─ DB write         80ms
```

**Questions:**
1. Critical path?
2. If pricing-call SLO is 200ms, where do you focus?
3. What span attributes would you add for deploy correlation?

---

### Exercise 2.3 — Tool matching (⏱ 15 min) ✍️

Match symptom → first tool:

| Symptom | Tool (perf / py-spy / JFR / eBPF / Datadog trace / Nsight) |
|---------|--------------------------------------------------------------|
| Java GC pauses in prod | |
| Python API slow, unknown function | |
| Kernel block I/O wait high | |
| GPU kernel low occupancy | |
| Cross-service tail latency | |

---

## Week 3 — Load Testing & NeoLoad

### Exercise 3.1 — Load test plan (⏱ 90 min) ✍️

**Scenario:** E-commerce checkout. Target: 500 RPS sustained, p95 < 800ms, errors < 0.1%.

Fill in:

| Section | Your plan |
|---------|-----------|
| Objectives (load/stress/soak which?) | |
| Workload model (VU, ramp, think time) | |
| Scenarios (steps) | |
| Test data strategy | |
| Correlation points (tokens, IDs) | |
| Monitors (tiers) | |
| Pass/fail criteria | |
| Risks / assumptions | |

---

### Exercise 3.2 — NeoLoad correlation (⏱ 45 min) ✍️

**Flow:** `GET /login` → extract `csrf_token` → `POST /login` (body uses token) → `GET /account` with session cookie.

Document:
1. Extractor type and source
2. Variable scope (global vs population)
3. Failure mode if correlation breaks (symptoms)
4. How you debug in NeoLoad

---

### Exercise 3.3 — Load vs stress vs soak (⏱ 20 min) 🎤

Explain each in 2 sentences and when you'd run each for the checkout scenario.

---

### Exercise 3.4 — Bottleneck table (⏱ 30 min) ✍️

| Observation | Likely cause | Next test to confirm |
|-------------|--------------|----------------------|
| Throughput flat, CPU 30% | | |
| Errors spike at same RPS every run | | |
| p99 grows over 4-hour soak | | |
| NeoLoad shows slow, DB CPU idle | | |

---

## Week 4 — Observability & CI/CD

### Exercise 4.1 — CI pipeline design (⏱ 60 min) ✍️

Draw stages:

```
PR → merge → staging → production
```

For each stage specify:
- Which perf tests run (smoke/load/soak)
- Max duration budget
- Pass/fail gates (example thresholds)
- Baseline storage approach

---

### Exercise 4.2 — APM trace exercise (⏱ 30 min) ✍️

**Scenario:** Dynatrace shows booking-service `pricing-call` 650ms but NeoLoad total transaction 980ms.

Reconcile the numbers. List 3 reasons for discrepancy between tools.

---

### Exercise 4.3 — Regression policy (⏱ 20 min) ✍️

Write a one-paragraph policy:
- p95 regression > 10% vs baseline → ?
- Error rate > 0.5% under standard load → ?
- Who approves override?

---

### Exercise 4.4 — Dashboard spec (⏱ 30 min) ✍️

Design 6 panels for a "Release 2.4.0 perf comparison" dashboard (metrics names only).

---

## Week 5 — GPU & Nsight

### Exercise 5.1 — GPU util debug checklist (⏱ 45 min) 🎤 ✍️

**Scenario:** Training job shows 35% GPU utilization average.

Write ordered checklist (min 12 items) covering:
- DataLoader
- CPU-GPU sync
- Batch size
- Kernel launch
- NCCL (if multi-GPU)
- I/O checkpointing

---

### Exercise 5.2 — Roofline whiteboard (⏱ 30 min) ✍️

**Kernel:** 1 TFLOP/s peak, 500 GB/s memory bandwidth. Performs 8 FLOP per 4 bytes loaded.

1. Arithmetic intensity = ?
2. Memory-bound or compute-bound?
3. Two optimization directions

---

### Exercise 5.3 — Nsight Systems narrative (⏱ 30 min) ✍️

Describe what you look for in a timeline (even without a live capture):
- Large CPU gaps before GPU work
- Back-to-back `cudaDeviceSynchronize`
- Low GPU rows with high CPU rows

---

### Exercise 5.4 — ML vs HPC vocabulary (⏱ 15 min) ✍️

Fill: When do you use NCCL vs MPI AllReduce?

| Context | Technology |
|---------|------------|
| Multi-GPU single node training | |
| Multi-node CFD simulation | |
| PyTorch DDP gradient sync | |
| Weather model on 1000 nodes | |

---

## Week 6 — Distributed Systems

### Exercise 6.1 — p99 regression RCA (⏱ 45 min) 🎤

**Scenario:** Deploy at 14:00. p99 up 40% by 14:30. p50 unchanged. One region affected.

Full spoken RCA (10 min max). Include rollback decision criteria.

---

### Exercise 6.2 — Caching design tradeoffs (⏱ 30 min) ✍️

Compare for read-heavy API:
1. CDN edge cache
2. Application Redis
3. DB query cache

Columns: consistency, invalidation complexity, tail latency impact.

---

### Exercise 6.3 — Capacity headroom (⏱ 30 min) ✍️

**Given:** Peak 10k RPS, current max tested 12k RPS at p95 SLA limit.

1. Headroom %?
2. Risks of running "hot"?
3. What load test proves payroll-week safety?

---

### Exercise 6.4 — System design timed (⏱ 45 min) ✍️ 🎤

**Prompt (R1):** Design continuous profiling for 500 microservices.

Timer 45 min. Structure:
1. Requirements (5 min)
2. High-level architecture (15 min)
3. Data storage & cost (10 min)
4. Adoption & rollout (10 min)
5. Failure modes (5 min)

Score with rubric in Section 16.

---

## Week 7 — ML Track (R5)

### Exercise 7A.1 — Training step breakdown (⏱ 30 min) ✍️

Label typical % time (rough) for a poorly optimized job vs well optimized:

| Phase | Poor job | Optimized job |
|-------|----------|---------------|
| DataLoader | | |
| Forward | | |
| Backward | | |
| AllReduce | | |
| Optimizer | | |

---

### Exercise 7A.2 — NCCL hang debug (⏱ 40 min) ✍️ 🎤

**Scenario:** 64-GPU job hangs at step 1000 during AllReduce.

Write debug steps 1–15. Include: logs, network, straggler, version mismatch.

---

### Exercise 7A.3 — Fleet utilization program (⏱ 45 min) ✍️

Design:
1. Top 5 metrics to track fleet-wide
2. Top 5 inefficiency patterns
3. Automated remediation (safe vs human approval)

---

### Exercise 7A.4 — Researcher collaboration (⏱ 20 min) 🎤

**Scenario:** Researcher says "model is slow." You have 30 min with them. Script your questions (min 10).

---

## Week 7 — HPC Track (R9)

### Exercise 7B.1 — Port strategy (⏱ 45 min) ✍️

**Scenario:** Legacy Fortran MPI code, 50k lines, hot loop is 3D stencil.

Compare paths:
1. OpenACC directives
2. CUDA rewrite of kernel only
3. Library substitution

Table: effort, risk, peak perf, maintainability (1–5 scale).

---

### Exercise 7B.2 — OpenACC data regions (⏱ 30 min) ✍️

Identify bug:
```fortran
!$acc data copyin(a(1:n)) copyout(b(1:n))
  call compute(a, b)
!$acc end data
! Host reads b(1:n) immediately without update
```

Explain fix and performance implication of `copy` vs `present`.

---

### Exercise 7B.3 — MPI scaling (⏱ 30 min) ✍️

**Given:** 80% efficiency at 32 ranks, 50% at 128 ranks.

List 5 causes of scaling loss. What plots do you generate?

---

### Exercise 7B.4 — Customer memo (⏱ 60 min) ✍️

Write 1-page customer-facing summary:
- Baseline CPU performance
- GPU port result (2.5x speedup)
- Correctness validation method
- Recommended next steps

---

## Week 8 — C++ AV Track (R6)

### Exercise 8A.1 — C++ hotspot patterns (⏱ 30 min) ✍️

Match pattern → symptom:
| Pattern | Symptom |
|---------|---------|
| False sharing | |
| Heap churn | |
| Lock contention | |
| Cache-unfriendly traversal | |

---

### Exercise 8A.2 — Perf CI for C++ (⏱ 45 min) ✍️

Design perf CI for perception pipeline:
- What runs per PR vs nightly?
- Benchmark binaries / golden traces?
- Regression threshold?

---

### Exercise 8A.3 — Latency vs throughput (⏱ 20 min) 🎤

Onboard AV inference: 10 cameras, 30 FPS each, 50ms budget per frame. Explain optimization priorities.

---

## Week 8 — Management Track (R4)

### Exercise 8B.1 — Unified QE roadmap (⏱ 60 min) ✍️

12-month roadmap with quarters:
- Q1: assess
- Q2: quick wins
- Q3: platform
- Q4: scale

Include perf, automation, observability pillars.

---

### Exercise 8B.2 — Leadership scenarios (⏱ 45 min) 🎤

Answer aloud (STAR):
1. SDET and perf engineer conflict on CI priority
2. Product skips quality gate for deadline
3. Flaky suite at 15% — your 30-day plan

---

### Exercise 8B.3 — Team metrics dashboard (⏱ 30 min) ✍️

Pick 8 metrics you'd report monthly to VP Engineering.

---

## Week 8 — Enterprise Load Test Track (R7/R8)

### Exercise 8C.1 — Executive report (⏱ 90 min) ✍️

Use template:

```markdown
# Performance Test Report — [Release X]

## Executive Summary
Pass/Fail: 
Business risk (1 paragraph):

## Test Scope
Environments, scenarios, duration, load model:

## Results Summary
[Table: scenario | SLA | actual | pass/fail]

## Key Findings
1.
2.
3.

## Recommendations (prioritized)
| Priority | Finding | Owner | Est. impact |

## Appendix
Methodology limitations:
```

Fill with realistic fictional numbers.

---

### Exercise 8C.2 — J2EE multi-tier (⏱ 30 min) ✍️

**Stack:** WebLogic + Oracle + browser UI.

Where do you place monitors for slow "Submit order" transaction?

---

### Exercise 8C.3 — NeoLoad vs JMeter (⏱ 15 min) 🎤

2-minute comparison; when you'd choose each.

---

## System Design Practice Sets

### Set A — Profiling platform (R1) ⏱ 45 min

**Requirements recap:** 500 services, <1% overhead, deploy tags, 90-day retention, self-service.

**Must mention:** sampling, symbolization, storage tiering, access control, CI integration.

---

### Set B — GPU fleet efficiency (R5) ⏱ 45 min

Scheduler integration, idle GPU detection, right-sizing jobs, chargeback metrics.

---

### Set C — Perf testing in CI (R8) ⏱ 45 min

NeoLoad controllers in K8s, baseline DB, flake handling, PR vs nightly.

---

### Set D — HPC customer engagement (R9) ⏱ 30 min narrative

Not boxes-only: science constraints → profile → port → validate → scale → compiler feedback.

---

## Mock Interview Scripts

### Mock 1 — Staff platform (R1) ⏱ 2.5 hr

| Block | Time | Prompt |
|-------|------|--------|
| Behavioral | 30m | Influence without authority; first 90 days |
| System design | 45m | Continuous profiling platform |
| Technical | 40m | p99 regression after deploy |
| Technical | 30m | Fleet cost down 15% without SLO breach |
| Your questions | 15m | |

**Evaluator notes (self):** Structure, tradeoffs, metrics, business tie-in.

---

### Mock 2 — ML infra (R5) ⏱ 2 hr

| Block | Time | Prompt |
|-------|------|--------|
| Technical | 45m | GPU util 40% — debug |
| Technical | 30m | NCCL timeout 64 GPUs |
| Behavioral | 25m | Researcher disagreement |
| System design | 30m | Fleet utilization program |

---

### Mock 3 — NeoLoad / enterprise (R8) ⏱ 2 hr

| Block | Time | Prompt |
|-------|------|--------|
| Technical | 45m | Design load+soak for microservices checkout |
| Technical | 30m | p95 doubled — RCA with APM |
| Technical | 30m | Correlation failure debugging |
| Behavioral | 15m | Executive report walkthrough |

---

### Mock 4 — HPC customer (R9) ⏱ 2 hr

| Block | Time | Prompt |
|-------|------|--------|
| Technical | 45m | Port MPI+OpenMP stencil to GPU |
| Technical | 30m | OpenACC data region bugs |
| Technical | 30m | Scaling efficiency drop 128 ranks |
| Behavioral | 15m | Customer wants GPU but I/O bound |

---

### Mock 5 — Engineering manager (R4) ⏱ 2 hr

| Block | Time | Prompt |
|-------|------|--------|
| Leadership | 45m | Unified QE roadmap; underperformer |
| Technical credibility | 30m | Flaky Playwright suite plan |
| Technical credibility | 30m | Load test strategy for payroll peak |
| Behavioral | 15m | Conflict with product on gates |

---

## Flashcards & Drills

### Daily 10-minute drill (rotate)

**Monday — Definitions:** 5 terms from Week 1 flash drill  
**Tuesday — Tool match:** Exercise 2.3 style  
**Wednesday — One STAR story** (3 min timed)  
**Thursday — Bottleneck table** (one row)  
**Friday — Whiteboard roofline or SLO math  
**Weekend — 45 min system design or mock block**

### SLO math practice

1. 99.9% monthly availability = max downtime minutes?  
2. If p99 target is 500ms and measured p99 is 650ms for 1 hour, is error budget burned? (depends on window — practice articulating nuance)

### HTTP / API quick fire

- Idempotency and perf testing implications  
- Connection keep-alive vs new connection  
- Rate limiting symptoms under load  

---

## Self-Scoring Rubrics

### System design (0–4 each, max 20)

| Criterion | 0 | 2 | 4 |
|-----------|---|---|---|
| Requirements clarifying questions | None | Some | Comprehensive |
| High-level architecture | Vague | Coherent | Scalable, clear boundaries |
| Deep dive (storage/cost/overhead) | Missing | Partial | Quantified tradeoffs |
| Failure modes & security | Ignored | Listed | Actionable mitigations |
| Rollout & adoption | None | Mentioned | Phased, metrics |

**Pass:** ≥ 14/20

### Technical RCA (0–4 each, max 16)

| Criterion | Description |
|-----------|-------------|
| Structured approach | Follows playbook |
| Layer isolation | Doesn't jump to conclusion |
| Metrics | Names specific signals |
| Actionable next steps | Experiments, not guesses |

**Pass:** ≥ 12/16

### Behavioral STAR (0–4 each, max 16)

| Criterion | Description |
|-----------|-------------|
| Specific "I" ownership | |
| Quantified results | |
| Under 3 minutes | |
| Relevant to question | |

**Pass:** ≥ 12/16

### NeoLoad / load test plan (0–4 each, max 16)

| Criterion | Description |
|-----------|-------------|
| Realistic workload model | |
| Data & correlation | |
| Pass/fail criteria | |
| Monitors all tiers | |

**Pass:** ≥ 12/16

---

## Personal Progress Log

Copy this table weekly.

| Week | Date | Exercises completed | Mock # | Score | Weak area | Next action |
|------|------|---------------------|--------|-------|-----------|-------------|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |
| 6 | | | | | | |
| 7 | | | | | | |
| 8 | | | | | | |
| 9 | | | | | | |
| 10 | | | | | | |
| 11 | | | | | | |
| 12 | | | | | | |

---

## STAR Story Worksheet (duplicate 8 times)

```markdown
### Story #___

**Title:**
**Target roles:** R__

**Situation:**
**Task:**
**Actions:**
- 
- 
- 

**Results (metrics):**
**Learning:**
**JD bullets this maps to:**
```

---

## Answer Cheat Sheets (expand in your own words)

### "Tell me about yourself" (2 min max)

1. Current role + years  
2. Specialty (perf domain)  
3. 1–2 marquee outcomes with numbers  
4. Why this role/company  
5. Stop — don't recite resume

### "GPU utilization low"

→ DataLoader → CPU preprocess → sync points → batch size → kernel occupancy → multi-GPU imbalance → checkpoint I/O → wrong GPU assigned

### "p99 up, p50 flat"

→ Tail issue: GC, slow dependency, lock, cache miss on path, one bad shard, regional issue

### "Influence without authority"

→ Shared pain → data → pilot → document standard → executive sponsor → measure adoption

---

## 300+ Practice Questions (by category)

### SLOs & reliability (30)
1. SLI vs SLO vs SLA  
2. Error budget policy example  
3. How load testing relates to availability SLO  
4. … *(continue in your log — add questions you miss in mocks)*

### Load testing (40)
1. Design load test for API-only microservice  
2. Stress test stopping criteria  
3. Soak test memory leak detection  
4. NeoLoad population vs user path  
5. Parameterization strategies  
…

### Profiling & OS (40)
1. eBPF vs sampling profiler  
2. Off-CPU analysis  
3. JVM GC tuning vs allocation reduction  
…

### GPU / ML (40)
1. Occupancy vs registers  
2. Tensor Core utilization  
3. DDP vs FSDP tradeoffs  
4. Inference batching for tail latency  
…

### HPC (40)
1. Strong scaling efficiency formula  
2. Ghost cell exchange cost  
3. OpenMP `schedule(dynamic)` when  
4. When OpenACC beats hand CUDA  
…

### Leadership (30)
1. First 90 days  
2. Hiring perf engineer  
3. Roadmap prioritization matrix  
…

### System design (40)
1. Continuous profiling  
2. Perf CI platform  
3. Chaos vs load testing boundaries  
…

**Instruction:** When you miss a question in practice, add it to your personal list in a `MY-MISSED-QUESTIONS.md` file.

---

## Next Steps After Completing 12 Weeks

1. Maintain **1 mock per month** per active job family.  
2. Refresh **3 STAR stories** before each real interview.  
3. Build **one real artifact** (GitHub): mini load test, profiler demo, or roofline notebook — optional but strong signal.

---

*Practice workbook v1.0 — May 2026*
