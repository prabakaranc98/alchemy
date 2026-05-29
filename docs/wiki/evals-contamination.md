# Evals & Contamination

**Layer:** Framing / measurement · **Role:** how we know a transfer actually worked

## What it is
**Benchmark contamination** = public test sets have leaked into pretraining corpora, so a high leaderboard score can reflect memorization, not capability. **Contamination-resistant evaluation** measures capability with **held-out, freshly-constructed, or behaviorally-probed** evals the models couldn't have seen.

## Why it matters for Alchemy
The entire success criterion depends on trustworthy measurement. Alchemy reports a **delta**, not an absolute number:

> `delta = score(transfer) − score(distillation)`, on a **held-out / self-built eval — never a raw leaderboard number.**

Because both arms (transfer vs. [distillation](distillation.md) baseline) run on the *same* eval, even an imperfect eval gives a fair *relative* signal — but only if it isn't contaminated in a way that ceilings both arms. And since the student has **no teacher at inference** (the no-coupling boundary), the eval must score the *standalone* student's behavior.

## The key mechanic
- Prefer **self-constructed contrastive evals** for the target capability (held-out splits, fresh prompts).
- Score behaviorally (does the generation show the capability?) rather than via a leaked multiple-choice set.
- Report `transfer − distillation` *and* `transfer − raw student` (did anything move at all).

## The catch
- Self-built evals can be **biased toward the mechanism you installed** — design them before touching the model, keep a held-out split.
- A small/negative delta is a **valid result** (this capability isn't modular enough to move that way) — don't tune the eval until it's positive.

## References
- Broad contamination findings across LLM benchmarks (2023–2025); measure deltas, not absolutes.

## Related
[distillation](distillation.md) (the baseline arm) · [PRIMER](../PRIMER.md) ("what success means") · [icm-modularity](icm-modularity.md) (a null delta = low modularity)
