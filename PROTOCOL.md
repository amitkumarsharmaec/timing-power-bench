# Protocol summary — timing-power-bench v1.0 (pre-launch draft)

Normative source: the paper (arXiv:2610.18890) and the maintainer build spec
`P3_Benchmark_Repo_Spec.md` §3–§7. This file is the public summary that will
be frozen as PROTOCOL.md v1.0 at launch; numbers below are the spec's launch
values.

## Round structure
- Designs/configurations are public; the round configurations (clock period,
  utilisation, floorplan per design) are revealed only when a round opens.
- Entrants pre-register (method summary, action types, budget, seeds) against
  the dev configurations; scores come from the round configurations.
- Rounds are monthly at launch (`round-2026-11`, ...). A closed round's rows
  are immutable; corrections appear as errata rows, never edits.

## Acceptance gate (per transaction, in order)
1. Snapshot current metrics (power by component, WNS/TNS, DRC count).
2. Apply the recovery action speculatively.
3. Re-analyse; pre-existing violations are held at snapshot values.
4. Apply proposed repairs subject to the repair-spending cap:
   `REPAIR_FRACTION = 0.5` of the transaction's measured gross saving.
5. Commit only if: net saving after repair > 0; WNS regression ≤ 10 ps;
   no new DRC violations; repair spend within cap.
6. Otherwise roll back and log the rejection reason. The run log is the
   audit trail.

## Scoring
- Disqualify (listed, unranked) if: final DRC > baseline; final WNS <
  baseline WNS − 10 ps; bundle fails validation; out-of-environment tools
  used without the open-environment flag.
- Primary metric: net total-power saving % vs baseline, over committed
  transactions only. Headline: median across the round set.
- Secondary (tie-break order): leakage saving %, dynamic saving %, TNS
  change, runtime, transactions attempted/committed/rolled back.

## Entrant contract
- Input: frozen post-route baseline checkpoint + read-only metrics snapshot.
- Output: ordered transaction list (JSON schema in entrants/README.md at
  launch). Only the runner applies transactions; entrants never touch the
  database directly.
- Runs inside the pinned Docker environment, no network, no proprietary
  dependencies; fixed seeds declared at pre-registration.
