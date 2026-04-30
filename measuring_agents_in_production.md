# Measuring Agents in Production

*Source: arXiv 2512.04123 — Melissa Z. Pan, Negar Arabzadeh, et al. (Anthropic, UC Berkeley, and others)*
*https://arxiv.org/abs/2512.04123*

---

## What This Is

An empirical study of LLM-based agents deployed in real-world settings: 20 production case studies + a survey of 306 practitioners across 26 domains. The core finding is that production deployment looks very different from benchmark performance.

---

## Key Statistics

| Metric | Finding |
|---|---|
| Step depth before human intervention | 68% of agents execute **≤10 steps** before a human steps in |
| Model customization | 70% rely on **prompting off-the-shelf models** rather than weight tuning |
| Evaluation method | 74% depend primarily on **human evaluation** |
| Top development challenge | **Reliability** — consistency over time, not raw capability |

---

## How Production Agents Actually Work

- **Simple and controllable**: Production teams favor predictable, auditable architectures over complex autonomous ones
- **Prompting-first**: Fine-tuning and weight tuning are rare — most orgs never go beyond prompt engineering
- **Human in the loop**: Human evaluation dominates over automated testing at scale
- **Short horizons**: Most production agents are shallow (≤10 steps) — long autonomous chains are the exception, not the norm
- **Systems-level reliability**: Reliability problems are addressed through system design (retries, fallbacks, human escalation), not model improvements

---

## The Benchmark-to-Production Gap

Agents that perform well on benchmarks commonly struggle in production with:

- **Ambiguous user requests** — real users don't phrase things like eval datasets
- **Incomplete information** — production tasks often lack the context benchmarks provide
- **Subtask failure recovery** — handling cascading failures mid-workflow
- **Cost-benefit tradeoffs** — benchmark scores ignore economic viability

---

## What to Measure in Production

Traditional accuracy metrics fail to capture production realities. The paper identifies these as essential:

| Dimension | What to track |
|---|---|
| Task completion | End-to-end success rate, not step-level accuracy |
| User satisfaction | Direct feedback, qualitative signals |
| Latency | P50, P95, P99 per workflow type |
| Cost efficiency | Cost per successful completion |
| Failure modes | Error classification, root cause analysis |
| Reliability over time | Consistency across sessions, not just average performance |

---

## Measurement Framework Recommendations

**Before deployment:**
- Establish baseline metrics with human performance benchmarks for comparison
- Define what "success" means for each task type before writing evals

**During production:**
- Real-time performance dashboards with cost tracking per interaction
- Continuous monitoring of agent behavior (not just model outputs)
- Incident tracking and root cause classification
- Feedback loops connecting user experience back to prompt/system improvements

**Human-in-the-loop:**
- Remains essential for critical tasks where agent autonomy poses risk
- Human evaluation is the ground truth — invest in making it scalable and consistent

---

## Practical Takeaways

1. **Don't over-engineer autonomy** — most production value comes from shallow, reliable agents, not long autonomous chains
2. **Prompt before you fine-tune** — 70% of production deployments never need weight tuning
3. **Reliability > capability** — the top challenge is consistency, not frontier performance
4. **Measure what matters in production** — cost, latency, user satisfaction, failure modes; not just benchmark scores
5. **Design for recovery** — production systems need retries, fallbacks, and human escalation paths baked in at the architecture level
