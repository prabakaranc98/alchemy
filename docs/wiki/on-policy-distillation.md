# On-Policy Distillation (OPD)

**Layer:** Shape · **Role:** frontier-proven backbone **and** the baseline every novel route must beat

## What it is
Classic [distillation](distillation.md) trains the student on the *teacher's* data (off-policy). **On-policy distillation** instead has the **student generate its own rollouts**, then the **teacher supplies dense supervision (reverse-KL) on the states the student actually visits.** It combines RL's "learn from your own mistakes" with distillation's dense per-token signal. **Multi-Teacher OPD (MOPD)** routes several domain teachers into one student — *compact knowledge fusion in production*.

## Why it matters for Alchemy
Two roles at once:
1. **Backbone.** This is how frontier labs actually shrink models (Qwen3, GLM-5, MiMo). It will produce a working specialist cheaply — **matching full-RL reasoning performance at ~10× lower compute than [GRPO](rl-grpo.md).**
2. **The baseline to beat.** Every novel transplant route (transport, dictionary, JEPA) must clear *plain OPD* to justify itself. Establish it in **Exp 0**; it's the denominator of the transfer-vs-distillation delta.

The headline recipe ends here: *extract → map → install → **shape (OPD)**.*

## The key mechanic
- Student samples completions on-policy ⇒ trajectories.
- Teacher scores each student-visited token with **reverse KL** (dense, per-token), pushing the student toward the teacher's distribution *where the student actually is*.
- No reward model needed; supervision is the teacher itself.

## The catch
- **Reverse KL is mode-seeking** — the student concentrates on the teacher's dominant modes and can **lose diversity**. Watch this directly against the *general-capacity* goal (Open Question 6); entropy-aware variants exist.
- It transfers what the teacher *does*, not necessarily an interpretable, isolable module — so it's the baseline, not the interpretability contribution.

## References
- Thinking Machines Lab, *On-Policy Distillation* (2025); OPD unification, `2605.16826`
- Entropy-aware OPD, `2603.07079`

## Related
[distillation](distillation.md) (the off-policy version it fixes) · [rl-grpo](rl-grpo.md) (the route that can exceed the teacher) · [predictive-jepa](predictive-jepa.md) (the install step before shaping)
