# Daily 2-Hour Study & Practice Plan (From Scratch)

**Goal:** Go from **zero performance-engineering background** to interview-ready in **12 weeks** (~84 days).  
**Time:** **2 hours every day** — split into **Learn (50 min) + Practice (60 min) + Review (10 min)**.  
**Companion files:**
- Theory & calendar: `PERFORMANCE-ENGINEERING-INTERVIEW-MASTER-GUIDE.md`
- Exercises: `PERFORMANCE-ENGINEERING-PRACTICE-WORKBOOK.md`

---

## How every 2-hour day works

```
┌─────────────────────────────────────────────────────────┐
│  MIN 0–10   │ Warm-up: recap yesterday (flashcards)    │
│  MIN 10–60  │ LEARN: read/watch one concept block       │
│  MIN 60–115 │ PRACTICE: workbook exercise or hands-on  │
│  MIN 115–120│ REVIEW: write 3 bullets in your log      │
└─────────────────────────────────────────────────────────┘
```

**Rules**
1. **No skipping Review** — write in `MY-DAILY-LOG.md` (template at bottom).
2. If stuck >15 min, write the question in your log and move on; revisit on weekend.
3. **Weekends:** same 2 hours (optional: extra 30 min hands-on if you want).
4. **Say answers aloud** during Practice when marked 🎤.

---

## Phase 0 — Absolute basics (Days 1–14)

*You understand: what perf eng is, how computers run programs, what latency means, how to read a simple metric.*

---

### Week 1 — “What is performance engineering?”

#### Day 1 — What problem do we solve?
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Read: [Google SRE — Monitoring](https://sre.google/sre-book/monitoring-distributed-systems/) sections “Definitions” and “Why monitor” (skim). Write: *What is latency? What is throughput?* in your own words. |
| Practice | 60m | Open any website → Chrome DevTools → Network tab → reload → note **TTFB** and **total time** for 3 requests. Screenshot + label each part (DNS, wait, download). |
| Review | 10m | Log: 3 terms learned today. |

**Scratch concepts:** User waits for response = **latency**. Requests per second = **throughput**.

---

#### Day 2 — How a request travels
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Draw from memory (then fix): Browser → DNS → Load balancer → App server → Database → back. Watch: “HTTP request lifecycle” (any 10–15 min video). |
| Practice | 60m | Draw diagram in notebook. Mark where time can be spent at each hop. List 5 things that could make a page slow. |
| Review | 10m | 🎤 Explain the path to an imaginary friend in 2 minutes. |

---

#### Day 3 — Percentiles (why average lies)
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Read Master Guide §3.1 (Latency & statistics). Example: 10 requests (ms): 100,100,100,100,100,100,100,100,100,5000 → average vs p95. |
| Practice | 60m | Workbook: calculate p50 and p95 by hand for: `50,50,50,50,50,50,50,50,50,200`. Then: `50×99 + 2000` for 100 requests — average vs p99? |
| Review | 10m | Log: “p95 means 95% of requests were faster than ___” |

---

#### Day 4 — CPU, memory, disk, network (simple)
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Read: Brendan Gregg USE method (1 page summary). Four resources: **CPU, Memory, Disk I/O, Network**. One sentence each: what “saturated” looks like. |
| Practice | 60m | On your Mac: Activity Monitor → while opening a heavy app, watch CPU % and Memory. Write: which resource spiked? |
| Review | 10m | Match symptom → resource: “disk light solid” → ? , “CPU 100%” → ? |

---

#### Day 5 — What is load testing?
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Master Guide §3.5 (load test types). Write definitions: **load, stress, soak** in plain English with one example each (coffee shop analogy: many customers, too many customers, all day). |
| Practice | 60m | Workbook Exercise 3.3 🎤 — explain aloud. Design on paper: “test a login page with 100 users” — what would you measure? |
| Review | 10m | 3 bullet definitions in log. |

---

#### Day 6 — SLI, SLO, SLA (intro)
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Read Master Guide §3.2. Analogy: SLI = speedometer reading, SLO = your target speed, SLA = ticket if you break contract with customer. |
| Practice | 60m | Workbook Exercise 1.1 (start) — fill **one row** only (Login API). Don’t rush all three. |
| Review | 10m | 🎤 SLI vs SLO in one sentence each. |

---

#### Day 7 — Week 1 recap
| Block | Time | Activity |
|-------|------|----------|
| Learn | 25m | Re-read your Week 1 log notes. |
| Practice | 85m | Workbook Exercise 1.2 🎤 (investigation drill) — even if shaky, full 8 min aloud. Workbook Exercise 1.4 flash drill. |
| Review | 10m | Rate yourself 1–5 on: latency, percentiles, SLI/SLO, load test types. |

**Week 1 checkpoint:** Can explain latency, p95, and one SLI without notes.

---

### Week 2 — “How to investigate slowness”

#### Day 8 — The 6-step playbook
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Memorize Master Guide §3.3 investigation playbook. Write each step on index card. |
| Practice | 60m | Scenario: “Website slow after lunch.” Walk all 6 steps on paper with 2 hypotheses per step. |
| Review | 10m | Recite steps from memory. |

---

#### Day 9 — Layers of the stack
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Master Guide §3.4 (bottleneck layers). Draw vertical stack: UI → API → DB → cache → OS → hardware. |
| Practice | 60m | Workbook Exercise 1.3 (metric interpretation table). |
| Review | 10m | “DB CPU 95%, app CPU 25%” → likely problem layer? |

---

#### Day 10 — Logs and metrics (beginner)
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Concepts: **metric** (number over time), **log** (event line), **trace** (request journey). Datadog “What is APM” intro page (skim). |
| Practice | 60m | If Docker installed: run `docker run -d -p 80:80 nginx` → `curl -w "@-"` timing (or use browser). Else: read curl man for `-w time_total`. Record 5 curl times. |
| Review | 10m | Difference metric vs log in one line. |

---

#### Day 11 — Introduction to profiling
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Concept: **profiler** = tells you *which functions* used CPU time. Read flame graph intro (Brendan Gregg — first 2 screens). |
| Practice | 60m | Install Python if needed. Run Workbook `sample_hotpath.py` with: `python -m cProfile -s cumtime sample_hotpath.py` |
| Review | 10m | Which function was hottest? |

---

#### Day 12 — Flame graphs (read one)
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | Study one flame graph image online. Width = time, bottom = root, top = leaf. |
| Practice | 60m | Workbook Exercise 2.1 — document top 2 frames from cProfile output (text table is fine). |
| Review | 10m | Draw a tiny 3-box flame graph on paper for: main → foo → bar. |

---

#### Day 13 — HTTP & APIs for perf testers
| Block | Time | Activity |
|-------|------|----------|
| Learn | 50m | HTTP methods GET/POST, status codes 200/404/500, headers (auth, content-type). REST API = URL + JSON. |
| Practice | 60m | Use https://httpbin.org — `curl` GET /delay/1, POST /post. Note times. What happens with /status/503? |
| Review | 10m | 5 status codes and meanings. |

---

#### Day 14 — Week 2 recap + SLO finish
| Block | Time | Activity |
|-------|------|----------|
| Learn | 25m | Review Week 2 log. |
| Practice | 85m | Finish Workbook Exercise 1.1 (all 3 API rows). Redo Exercise 1.2 🎤 — compare to Day 7. |
| Review | 10m | Checkpoint: playbook + one flame graph concept. |

**Week 2 checkpoint:** Can run 6-step investigation and basic cProfile.

---

## Phase 1 — Core skills (Days 15–42)

*You understand: tracing, load test design, observability basics, CI gates, intro GPU concepts.*

### Week 3 — Tracing & distributed systems intro

| Day | Learn (50m) | Practice (60m) |
|-----|-------------|----------------|
| **15** | OpenTelemetry: trace, span, parent/child (otel.io docs intro) | Draw trace tree for: checkout → payment → DB (Workbook 2.2 on paper) |
| **16** | Tail latency: why p99 matters in microservices | Workbook 2.2 full answers |
| **17** | Caching basics: browser, CDN, app cache, DB | Workbook 6.2 caching table (preview — fill what you know) |
| **18** | Connection pools, thread pools (conceptual) | Scenario: “pool exhausted” — symptoms + fix on paper |
| **19** | Little’s Law (L = λW) — read Master Guide §3.1 | Word problem: 100 users, 2 sec avg response → concurrency? |
| **20** | NeoLoad concepts: population, scenario, VU | Workbook 3.1 — fill **Objectives** and **Scenarios** only |
| **21** | **Recap** | Workbook 3.3 🎤 + 3.4 one row |

---

### Week 4 — Load testing & NeoLoad

| Day | Learn (50m) | Practice (60m) |
|-----|-------------|----------------|
| **22** | Correlation: extract token, reuse in next request | Workbook 3.2 correlation doc |
| **23** | Think time, ramp-up, pacing | Workbook 3.1 complete full plan |
| **24** | Pass/fail criteria tied to SLO | Add pass/fail to your 3.1 plan |
| **25** | APM intro: Datadog or Dynatrace “distributed tracing” | Workbook 4.2 APM reconciliation |
| **26** | CI/CD vocabulary: PR, pipeline, gate | Workbook 4.1 CI pipeline sketch |
| **27** | Regression baselines | Workbook 4.3 regression policy |
| **28** | **Recap** | Workbook 4.4 dashboard spec + Week 4 checkpoint |

**Week 4 checkpoint:** One complete load test plan on paper; explain correlation.

---

### Week 5 — Linux & scripting for perf

| Day | Learn (50m) | Practice (60m) |
|-----|-------------|----------------|
| **29** | Linux top/htop: CPU, MEM columns | Run `top` or Activity Monitor; note top 3 processes |
| **30** | `curl` timing, `ab` or `hey` basic load (install hey if needed) | 100 requests to httpbin; record avg and max time |
| **31** | JavaScript basics for perf: JSON.parse, fetch (MDN skim) | Write 5-line Node or browser script: fetch URL, log time |
| **32** | Docker basics: image, container, port | `docker run` nginx + curl loop 20 times |
| **33** | Kubernetes concepts: pod, service, HPA (video 15m) | Draw K8s diagram for 3 replicas behind load balancer |
| **34** | Prometheus: metric name, label, scrape (intro) | Workbook 4.4 — add one Prometheus-style metric name |
| **35** | **Recap** | Workbook 2.3 tool matching + flash drill |

---

### Week 6 — Databases & middleware

| Day | Learn (50m) | Practice (60m) |
|-----|-------------|----------------|
| **36** | DB bottleneck signs: slow queries, locks, pool wait | Workbook 8C.2 monitors placement (J2EE stack on paper) |
| **37** | Index concept: why full table scan is slow | Draw table 1M rows — with vs without index lookup |
| **38** | N+1 query problem (ORM) | Write example pseudo-code N+1 vs 1 query |
| **39** | Message queues: async helps throughput | Scenario: email send async — latency vs reliability tradeoff |
| **40** | Workbook 6.1 🎤 p99 regression scenario | Full spoken RCA 10 min |
| **41** | Workbook 6.3 capacity headroom | Math + narrative |
| **42** | **Recap** | Workbook 6.4 timed system design (45m) — first attempt, score with rubric |

**Week 6 checkpoint:** Explain DB + cache + queue in one request path.

---

## Phase 2 — Choose your track (Days 43–70)

**Pick ONE primary track** (2 hours/day still). Skim others on Sundays optionally.

| Track | For roles | Workbook weeks |
|-------|-----------|----------------|
| **A — Web / load test** | R7, R8, R4 (partial) | W3, W8C, mocks 3 |
| **B — GPU / ML** | R5, R3, R6 (partial) | W5, W7A, mocks 2 |
| **C — HPC / NVIDIA customer** | R9, R2 | W5 roofline, W7B, mock 4 |
| **D — Staff / platform** | R1, R3 | W6, system designs, mock 1 |
| **E — Manager** | R4 | W8B, mock 5 |

---

### Track A — Web / load test (Days 43–56)

| Day | Learn | Practice |
|-----|-------|----------|
| 43 | NeoLoad populations deep dive (docs) | Refine Workbook 3.1 with populations |
| 44 | Test data management strategies | Document data plan for 1000 users |
| 45 | JMeter vs NeoLoad comparison | Workbook 8C.3 🎤 2 min |
| 46 | Multi-tier monitoring | Workbook 8C.2 |
| 47 | J2EE/WebLogic/Tomcat concepts (skim) | Map tiers for enterprise app |
| 48 | ELK / logs search basics | What log line proves timeout? |
| 49 | Workbook 8C.1 executive report — section 1–2 | Write exec summary |
| 50 | Finish 8C.1 report | Full report with tables |
| 51 | Mock 3 part 1 🎤 | Design load+soak checkout |
| 52 | Mock 3 part 2 | p95 doubled RCA |
| 53 | STAR stories S4, S6 (load/CI) | Write in MY-STAR-STORIES.md |
| 54 | Agile + Jira QA process (skim) | DevTestOps bullets |
| 55 | Practice correlation failure debug | Workbook 3.2 aloud |
| 56 | **Track recap** | Full Mock 3 timed 2hr |

---

### Track B — GPU / ML (Days 43–56)

| Day | Learn | Practice |
|-----|-------|----------|
| 43 | GPU vs CPU: parallel cores, memory | Draw GPU simple diagram |
| 44 | CUDA: host, device, kernel, stream | Watch: CUDA intro 20m |
| 45 | Why GPU util can be low | Workbook 5.1 checklist write |
| 46 | Nsight Systems: timeline reading | Workbook 5.3 narrative |
| 47 | Nsight Compute: occupancy, memory | Workbook 5.2 roofline math |
| 48 | PyTorch training loop parts | Label: forward, backward, step |
| 49 | DataLoader bottlenecks | Workbook 7A.1 table |
| 50 | NCCL / AllReduce concept | Workbook 7A.2 steps 1–8 |
| 51 | NCCL debug continued | Steps 9–15 |
| 52 | Fleet utilization metrics | Workbook 7A.3 |
| 53 | Mock 2 part 1 🎤 | GPU util 40% debug |
| 54 | Workbook 7A.4 researcher questions | 🎤 10 questions |
| 55 | STAR S5 GPU story draft | Metrics |
| 56 | **Track recap** | Mock 2 full |

---

### Track C — HPC (Days 43–56)

| Day | Learn | Practice |
|-----|-------|----------|
| 43 | MPI: ranks, broadcast, Allreduce | Diagram 4 ranks |
| 44 | OpenMP: parallel for | Read simple OpenMP loop example |
| 45 | OpenACC: data regions intro | Workbook 7B.2 bug find |
| 46 | Fortran vs C array layout | Row-major vs column-major drawing |
| 47 | Stencil / ghost cells | Workbook 7B.1 port strategy table |
| 48 | Strong vs weak scaling | Workbook 7B.3 |
| 49 | Roofline model deep | Workbook 5.2 + HPC examples |
| 50 | Correctness vs fast-math | When optimization breaks science |
| 51 | Workbook 7B.4 customer memo | Draft half |
| 52 | Finish 7B.4 | Full memo |
| 53 | Assembly reading (intro skim) | Optional: view objdump one function |
| 54 | Mock 4 part 1 | MPI+OpenMP port 🎤 |
| 55 | STAR customer story | |
| 56 | **Track recap** | Mock 4 full |

---

### Track D — Staff platform (Days 43–56)

| Day | Learn | Practice |
|-----|-------|----------|
| 43 | Continuous profiling concept | Read Parca or Datadog CP page |
| 44 | Sampling vs instrumentation overhead | Compare tradeoffs table |
| 45 | eBPF intro (what it can trace) | List 5 use cases |
| 46 | JVM profiling (JFR concept) | When to use vs Python |
| 47 | System design Set A requirements | List 10 requirements |
| 48 | System design Set A architecture | Box diagram 45m |
| 49 | Error budgets + product tradeoffs | Master Guide §3.2 scenarios |
| 50 | Fleet utilization / cost | 15% cost reduction brainstorm |
| 51 | Mock 1 part 1 — behavioral | 2 STARs 🎤 |
| 52 | Mock 1 system design | 45m timed |
| 53 | Influence without authority STAR | |
| 54 | Build vs buy STAR | |
| 55 | First 90 days outline | R1 style |
| 56 | **Track recap** | Mock 1 full |

---

### Days 57–70 — Secondary skills + merge

Everyone (all tracks) — 2 hours/day:

| Day | Focus |
|-----|--------|
| 57 | Read **other track** Day 43 Learn (30m) + your track Practice |
| 58 | Workbook 5.1 if not done; else 6.1 RCA |
| 59 | Behavioral: draft STAR S1 (regression story) |
| 60 | STAR S2 (tooling story) |
| 61 | STAR S3 (influence story) |
| 62 | STAR S7 or S8 (mentorship or failure) |
| 63 | System design second prompt (pick from Master Guide §9) |
| 64 | 🎤 “Tell me about yourself” 2 min timed |
| 65 | Workbook flash drills — all Week 1 terms |
| 66 | Workbook 2.3 + 5.4 ML vs HPC |
| 67 | Company research template (pick 1 company) |
| 68 | Questions to ask interviewers — write 10 |
| 69 | Weak area from progress log — remediate |
| 70 | **Phase 2 exam:** 2hr self-test (see below) |

**Phase 2 self-test (Day 70, 2 hours)**
- 20m: Define 10 terms cold
- 30m: Investigation drill 🎤
- 45m: System design or load test plan (your track)
- 25m: 2 STAR stories 🎤

---

## Phase 3 — Interview sprint (Days 71–84)

| Day | Activity (2h) |
|-----|----------------|
| **71** | Mock interview (full) — record yourself |
| **72** | Review recording; list 10 gaps |
| **73** | Fix gap #1–3 with Learn+Practice |
| **74** | Mock technical only (45m) + review |
| **75** | Mock behavioral only (45m) + review |
| **76** | Second company research + tailor pitch |
| **77** | System design #3 timed |
| **78** | All 8 STARs 🎤 under 3 min each (use timer) |
| **79** | Workbook missed questions file — 20 items |
| **80** | NeoLoad OR GPU OR HPC drill (your weakest) |
| **81** | Mock with friend or AI — full loop |
| **82** | Day-before checklist (Master Guide §11) |
| **83** | Light review only — flashcards |
| **84** | Rest or 1 mock — **you are interview-ready** |

---

## Scratch-level concept glossary (study 2 terms/day in Review)

| Term | One-line meaning |
|------|------------------|
| Latency | Time to complete one operation |
| Throughput | Operations completed per unit time |
| p95 | 95% of requests faster than this value |
| SLI | What you measure |
| SLO | Target for the SLI |
| Bottleneck | Slowest part limiting the whole system |
| Profiler | Tool showing where CPU/time is spent |
| Trace | Record of one request across services |
| Load test | Simulating many users to measure behavior under traffic |
| Correlation | Capturing dynamic values (tokens) between requests |
| Saturation | Resource at limit (queues form) |
| Regression | Performance got worse vs baseline |
| GPU utilization | % time GPU cores busy (low ≠ always bad) |
| MPI | Message Passing for multi-node HPC programs |

---

## MY-DAILY-LOG.md (copy to docs/)

```markdown
# Daily Log

## Day __ / 84 — Date: ____

**Phase:** 0 / 1 / 2 / 3  
**Track:** A / B / C / D / none  

### Learn (50m)
Topic:
Notes (3 bullets):
1.
2.
3.

### Practice (60m)
Exercise:
Outcome:

### Review (10m)
Hardest concept today:
Question for later:
Confidence 1–5:

### Tomorrow
Next day # from plan: __
```

---

## If you miss a day

- **Miss 1 day:** Continue to next day number (don’t restart).
- **Miss 3+ days:** Repeat last **Recap** day before continuing.
- **Behind schedule:** Finish Phase 0–1 fully before Track phase — don’t skip Days 1–14.

---

## Quick reference: your first 7 days start tomorrow

| Day | One-line focus |
|-----|----------------|
| 1 | Latency & browser Network tab |
| 2 | Request path diagram |
| 3 | Percentiles math |
| 4 | CPU/memory/disk/network |
| 5 | Load vs stress vs soak |
| 6 | SLI/SLO intro |
| 7 | Week 1 recap + investigation drill |

**Open:** `PERFORMANCE-ENGINEERING-PRACTICE-WORKBOOK.md` only when the plan says "Workbook Exercise X."

---

*2 hours/day × 84 days ≈ 168 hours — enough for scratch → interview ready if you practice aloud and log daily.*
