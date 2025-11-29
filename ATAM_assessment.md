# ADD — Iteration 3 (AIDAP) — ATAM Assessment & Artifacts

This document performs Iteration 3 ATAM activities for AIDAP: it contains  
(A) an ATAM risk assessment table,  
(B) descriptions of risks, non-risks, sensitivity points, and tradeoffs, and  
(C) an ATAM utility tree.  

It concludes with an evaluation of whether the ATAM deliverables are satisfied and a short checklist of evidence required to claim satisfaction.

Status: draft — update with empirical results, logs, and performance measurements to finalize.

---

## Executive summary

Iteration 3 focuses on applying the ATAM to evaluate how well our architecture supports the key requirements of the **Sync Subsystem** — the part responsible for periodic LMS/Registration/Calendar updates (UC3).  

Our prioritized quality attributes for this iteration are:

- **Scalability** (handling peak sync and notification load)
- **Interoperability** (stable communication with external systems)
- **Availability & Reliability** (fault tolerance, retries, circuit breakers)

These qualities were selected because the sync pipeline interacts with unstable external systems and therefore introduces the highest architectural risk. Below are the ATAM artifacts required for assessment.

---

## A. ATAM Risk Assessment Table

| ID | Risk / Area | Severity | Likelihood | Impact | Mitigation | Detection | Recovery |
|---:|-------------|:--------:|:----------:|--------|------------|-----------|----------|
| R1 | Sync overload during peak load | High | Medium | Slowed updates, delayed notifications | Stagger job intervals, MQ backpressure, worker autoscaling | Monitor queue lag and job durations | Re-enqueue sync jobs and expand worker pool |
| R2 | External system outage causing repeated failing sync calls | High | High | Cascading failures and blocked workers | Circuit breaker + exponential retry | Adapter error logs, breaker state | Reopen breaker after cooldown; replay job |
| R3 | Partial or inconsistent sync data | Medium | Medium | Incorrect exam dates, outdated course info | Atomic “fetch→transform→validate→store” pipeline | Sync validation logs | Roll back to last-good snapshot |
| R4 | MQ congestion from concurrent notifications + sync traffic | Medium | Medium | Increased latency for UC1 & UC2 | Scale workers, partition queues | MQ throughput metrics | Add workers, drain queue |
| R5 | Adapter schema drift (API changes) | Medium | High | Parsing failures, missing fields | Versioned adapters, schema validation | API error patterns | Hotfix adapters, fallback to cached data |
| R6 | Retry misconfiguration causing job pileups | Medium | Medium | Message storms, queue saturation | Jittered exponential backoff, retry caps | Retry-handler logs | Kill stuck jobs; reschedule |
| R7 | Circuit breaker too sensitive | Low | Medium | Updates pause unnecessarily | Tune thresholds based on error type | Circuit breaker dashboard | Manual override, adjust config |
| R8 | Sync interval misconfigured (too frequent) | Low | Medium | Excessive load on external APIs | Enforced minimum interval | Scheduler logs | Reset schedule, cooldown |

---

## B. Risks, Non-Risks, Sensitivity Points, and Tradeoffs

### **Risks (expanded)**

- **R1 – Sync overload**: If multiple sync jobs launch simultaneously during peak hours, they may overload both AIDAP and external systems. MQ buffering and job staggering are essential.
- **R2 – External outages**: LMS/Registration APIs frequently become unreachable. Without circuit breakers, AIDAP may continuously retry and collapse.
- **R3 – Partial updates**: A sync could fail halfway, leaving academic data inconsistent. The structured sync pipeline mitigates this.
- **R4 – MQ congestion**: Since UC2 (notifications) and UC3 (sync) share the MQ, high load from both may produce delays.
- **R5 – API schema drift**: External systems may change field names or formats without notice. Adapters must validate and isolate failures.
- **R6 – Retry storms**: Incorrect retry settings can flood external systems or overwhelm our own workers.
- **R7 – Overactive circuit breaker**: If breaker thresholds are too low, sync pauses too easily, leading to stale academic data.
- **R8 – Overly frequent sync intervals**: Misconfigured scheduler values can cause runaway API calls.

---

### **Non-Risks (validated strengths)**

- **NR1 – Adapter Pattern**: Our adapter layer isolates external API differences, maximizing interoperability (QA3).
- **NR2 – Layered Architecture**: Sync failures do not block the NLU or presentation layer (UC1 remains stable).
- **NR3 – Cloud-native deployment**: Independent autoscaling of Syncer workers, MQ workers, and NLU ensures availability and scalability (QA2, QA4).
- **NR4 – Structured Sync Pipeline**: Clear fetch → transform → validate → store stages reduce the chance of corrupted data.
- **NR5 – Background scheduler**: Sync operations occur predictably and avoid random traffic spikes.

---

### **Sensitivity Points (small change → big effect)**

- **SP1 – Sync frequency**: Changing the sync interval dramatically affects system load and data freshness.
- **SP2 – Retry/backoff configuration**: Even small adjustments to retry caps or backoff timing can destabilize systems.
- **SP3 – Circuit breaker thresholds**: Highly sensitive to error frequency; small misconfigurations cause major availability shifts.
- **SP4 – MQ worker count**: Too few workers → backlog; too many → wasted CPU and increased costs.
- **SP5 – External API rate limits**: Minor reductions in rate limits can break sync reliability.

---

### **Tradeoffs (selected)**

- **Freshness vs Load**: More frequent sync improves data freshness but increases load and risk.
- **Retry aggressiveness vs API protection**: More retries improve reliability but can spam external systems.
- **Bulkhead isolation vs complexity**: Isolating each external adapter increases resilience but adds operational overhead.
- **Unified queue vs multiple queues**: A single MQ simplifies deployment but increases cross-traffic contention.
- **Circuit-breaker strictness vs availability**: Stricter thresholds protect resources but decrease sync frequency.

---

## C. ATAM Utility Tree

**Utility Goal**: Ensure reliable, scalable, and interoperable synchronization of academic data while maintaining system availability.

### **Performance (High Priority)**
- **P1**: Each sync job completes in ≤5 seconds per external system.
- **P2**: MQ lag remains <1 second under typical peak load.
- **P3**: UC1 and UC2 response times remain unaffected during peak sync activity.

### **Availability & Reliability (High Priority)**
- **A1**: Sync retries succeed within bounded exponential backoff.
- **A2**: Circuit breaker prevents cascading failures during outages.
- **A3**: No partial writes during failed sync attempts.

### **Interoperability (High Priority)**
- **I1**: External API schema changes do not break the Syncer Service.
- **I2**: Adapters provide stable interfaces regardless of external variability.

### **Scalability (Medium Priority)**
- **S1**: Sync worker pools scale horizontally with load.
- **S2**: MQ expands automatically under sustained traffic.

### **Maintainability (Medium Priority)**
- **M1**: Adapters can be hot-fixed within <2 hours when external APIs change.

---

## D. Do these ATAM deliverables get satisfied?

**Partially.**  
We produced the core ATAM artifacts required:

- ✔ ATAM risk assessment table  
- ✔ Expanded analysis of risks, non-risks, sensitivity points, and tradeoffs  
- ✔ Completed utility tree aligned with the Sync Subsystem  
- ✔ Clear mapping to use cases, quality attributes, concerns, and constraints  

To fully satisfy the assessment, we still need to incorporate:

### **Remaining Evidence Required**
- Empirical logs from a prototype or mocked sync execution  
- MQ throughput and lag measurements under simulated load  
- Circuit breaker threshold validation logs  
- Documentation of retry/backoff policy testing  
- A brief demo or simulation script for sync failure scenarios  

Once these empirical artifacts are added to the repository, the ATAM evaluation will be fully complete.

---
