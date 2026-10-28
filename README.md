# timing-power-bench

A pre-registered public evaluation protocol for post-route power recovery in
open EDA flows — the companion benchmark for:

**Amit Sharma, "AI-Driven, Timing-Aware Power Optimization," arXiv:2610.18890**
https://arxiv.org/abs/2610.18890 · Hugging Face Papers: https://huggingface.co/papers/2610.18890

## Status: pre-launch scaffold (October 2026)

This scaffold reserves the repo structure and citation plumbing ahead of the
v1.0 launch. The flow scripts, transactional runner, and reference entrants
described in the build spec are **not yet implemented** — do not cite results
from this repo yet; there are none. First leaderboard will contain only the
two reference entrants (greedy heuristic, gated random), run by the maintainer.

Full build spec: `P3_Benchmark_Repo_Spec.md` in the citation-engine goal files
(maintainer copy; summary in PROTOCOL.md here).

## What the benchmark will measure

- Entrants propose an ordered list of power-recovery transactions against a
  frozen post-route baseline (OpenROAD / OpenROAD-flow-scripts + OpenSTA,
  Nangate45 at launch).
- A deterministic acceptance gate — not the entrant's model — commits or rolls
  back each transaction: commit only if net power saving after repair is
  positive, no analysis view regresses beyond published guardbands
  (launch: WNS regression ≤ 10 ps, no new DRC violations), and repair spend
  stays within the cap (`REPAIR_FRACTION = 0.5` of measured gross saving).
- Headline score: median net total-power saving % across the round's
  design/configuration set. Designs at launch: `gcd` (smoke), `ibex`, `aes`.

## Honest caveats (from the paper and the spec — read before comparing)

- Open-flow results are **relative method-comparison results, not signoff
  numbers**. OpenSTA/OpenROAD timing is not signoff STA.
- Open libraries (Nangate45, later Sky130 / ASAP7) offer far fewer Vt flavours
  and drive-strength options than commercial libraries, so achievable savings
  will be smaller than in commercial post-route flows. That constraint applies
  equally to every entrant, which is what makes scores comparable.
- The paper reports **no trained-model results**; the learned ranker is a
  proposed v2 entrant slot. v1 ships deterministic gate + non-learned
  reference entrants only.

## How to cite

If you use this benchmark or report results from it, please cite the protocol
paper:

```bibtex
@misc{sharma2026timingpower,
  title         = {AI-Driven, Timing-Aware Power Optimization},
  author        = {Sharma, Amit},
  year          = {2026},
  eprint        = {2610.18890},
  archivePrefix = {arXiv},
  primaryClass  = {cs.AR},
  url           = {https://arxiv.org/abs/2610.18890}
}
```

A Zenodo DOI for the benchmark release itself will be minted at v1.0.0
(GitHub–Zenodo integration, enabled before the first release) and recorded
here and in `CITATION.cff`. Until then, cite the paper above only.

## Licence

Code: Apache-2.0 (LICENSE file added at launch). Protocol/docs text: CC BY 4.0.
Design sources and PDK files are not covered — they remain under their
upstream licences and are fetched from ORFS at pinned versions, never vendored.
