# Autoresearch Attempts

This file is append-only during `autoresearch` runs and is updated after each evaluated outcome.

It serves as an experiment log for approved and rejected candidates.

Add new entries at the end. Do NOT delete any historical entries recorded here!

## Entry Template

Use this exact structure for each appended attempt:

```md
## Attempt: <timestamp> - <candidate_version>

- status: `approved` | `rejected`
- commit: `<short_sha>` if approved, otherwise `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `<version>`
- seed_file: `<path>`
- candidate_version: `<version>`
- version_bump: `minor` | `major`
- hypotheses:
  - `<hypothesis 1>`
  - `<hypothesis 2>`
  - `<hypothesis 3>`
- implementation_summary: `<short summary of what changed>`
- evaluation_log_path: `autoresearch/approved_logs/<short_sha>-result.csv` or `<n/a>`
- wins/draws/losses: `<int>/<int>/<int>`
- score: `<float>`
- score_rate: `<float>`
- average_plies: `<float>`
- average_processing_time_ms: `<float or n/a>`
- average_positions_or_nodes: `<float or n/a>`
- inferred_conclusion: `<what future experiments should learn from this result>`
```




## Attempt: 2026-06-05T18:39:57Z - v2.1

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.0`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_0Engine.cs`
- candidate_version: `v2.1`
- version_bump: `minor`
- hypotheses:
  - `A lightweight quiet-history move-ordering table will improve alpha-beta cutoffs at 100ms per move without changing evaluation.`
- implementation_summary: `Cloned v2.0 into v2.1, renamed the public type/search entrypoint, and added a per-search quiet history table that rewards quiet beta-cutoff moves by side/from/to square.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `n/a/n/a/n/a`
- score: `n/a`
- score_rate: `n/a`
- average_plies: `n/a`
- average_processing_time_ms: `n/a`
- average_positions_or_nodes: `n/a`
- inferred_conclusion: `The quiet-history ordering hypothesis did not produce enough visible early separation to justify continuing this interrupted run. Future attempts should prefer changes that alter decisive move choice or endgame conversion rather than only same-evaluation ordering tweaks, unless they include a mechanism likely to affect paired scores rather than mostly mirrored draws.`


## Attempt: 2026-06-05T19:44:30Z - v2.2

- status: `approved`
- commit: `765feb6`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.0`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_0Engine.cs`
- candidate_version: `v2.2`
- version_bump: `minor`
- hypotheses:
  - `A compact piece-square positional evaluation term will improve direct move choice at 100ms per move more reliably than search-only ordering tweaks.`
- implementation_summary: `Cloned v2.0 into v2.2, renamed the public type/search entrypoint, and added static piece-square tables for pawns, knights, bishops, rooks, queens, and phase-blended kings to the local evaluation.`
- evaluation_log_path: `autoresearch/approved_logs/V2_2Engine-765feb6-result.csv`
- wins/draws/losses: `117/350/33`
- score: `292.0`
- score_rate: `0.5840`
- average_plies: `67.93`
- average_processing_time_ms: `103.171`
- average_positions_or_nodes: `13705.48`
- inferred_conclusion: `A small direct positional evaluation layer is a strong improvement over pure material plus endgame/repetition terms for the v2.0 engine. Future attempts should build on v2.2 and tune or extend evaluation terms carefully, while watching for any added per-node cost that could reduce the current node rate advantage.`


## Attempt: 2026-06-05T20:56:06Z - v2.3

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.2`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_2Engine.cs`
- candidate_version: `v2.3`
- version_bump: `minor`
- hypotheses:
  - `Caching the endgame phase for king piece-square scoring will preserve v2.2 evaluation semantics while increasing searched nodes enough to improve paired results.`
- implementation_summary: `Cloned v2.2 into v2.3, renamed the public type/search entrypoint, and changed evaluation snapshot construction to compute endgame phase once, collect king squares during the existing board scan, and score king PSTs from the cached phase instead of rescanning the board.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `65/359/76`
- score: `244.5`
- score_rate: `0.4890`
- average_plies: `79.41`
- average_processing_time_ms: `103.207`
- average_positions_or_nodes: `16057.04`
- inferred_conclusion: `The cached king-phase cleanup did increase candidate nodes versus v2.2 (16057.04 vs 14956.35 average positions/nodes), but the timing/search perturbation did not translate into strength and slightly underperformed. Future work should not promote pure semantics-preserving micro-optimizations unless they also demonstrate a move-choice or search-depth advantage in paired results.`


## Attempt: 2026-06-05T22:04:03Z - v2.4

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.2`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_2Engine.cs`
- candidate_version: `v2.4`
- version_bump: `minor`
- hypotheses:
  - `A small bishop-pair bonus will improve positional decisions on top of v2.2 with negligible extra evaluation cost.`
- implementation_summary: `Cloned v2.2 into v2.4, renamed the public type/search entrypoint, and added a 35 centipawn bishop-pair bonus to each side's existing positional total after bishop counts are collected.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `80/348/72`
- score: `254.0`
- score_rate: `0.5080`
- average_plies: `76.55`
- average_processing_time_ms: `103.299`
- average_positions_or_nodes: `14658.04`
- inferred_conclusion: `The bishop-pair bonus was directionally positive on raw score but too noisy and not statistically reliable against v2.2. Future evaluation changes should either be broader than a single small static bonus or targeted at specific conversion/draw problems, since small generic bonuses may add variance without clearing the paired confidence threshold.`


## Attempt: 2026-06-05T23:14:22Z - v2.5

- status: `approved`
- commit: `519f5a3`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.2`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_2Engine.cs`
- candidate_version: `v2.5`
- version_bump: `minor`
- hypotheses:
  - `A modest passed-pawn advancement bonus will improve conversion and pawn-structure decisions beyond v2.2's indirect endgame pawn-danger term.`
- implementation_summary: `Cloned v2.2 into v2.5, renamed the public type/search entrypoint, and added rank-scaled passed-pawn bonuses for pawns with no opposing pawn ahead on the same or adjacent files.`
- evaluation_log_path: `autoresearch/approved_logs/V2_5Engine-519f5a3-result.csv`
- wins/draws/losses: `118/315/67`
- score: `275.5`
- score_rate: `0.5510`
- average_plies: `79.30`
- average_processing_time_ms: `103.412`
- average_positions_or_nodes: `14199.67`
- inferred_conclusion: `Passed-pawn advancement scoring is a statistically reliable improvement over v2.2 despite slightly lower node throughput. Future work should build from v2.5 and prefer targeted pawn/conversion evaluation refinements over isolated generic bonuses.`


## Attempt: 2026-06-06T00:21:40Z - v2.6

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.5`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_5Engine.cs`
- candidate_version: `v2.6`
- version_bump: `minor`
- hypotheses:
  - `Protected passed pawns should be valued slightly more than bare passed pawns because they are harder to blockade and more likely to convert.`
- implementation_summary: `Cloned v2.5 into v2.6, renamed the public type/search entrypoint, and added a 14 centipawn bonus when a passed pawn is defended from behind by a friendly pawn on an adjacent file.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `93/322/85`
- score: `254.0`
- score_rate: `0.5080`
- average_plies: `75.00`
- average_processing_time_ms: `103.387`
- average_positions_or_nodes: `13999.47`
- inferred_conclusion: `A protected-passed-pawn bonus on top of v2.5 was directionally positive but not statistically reliable. Future pawn work should avoid simply stacking small passed-pawn bonuses and should instead target clearer pawn-race, blockade, or promotion-conversion features.`


## Attempt: 2026-06-06T01:28:59Z - v2.7

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.5`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_5Engine.cs`
- candidate_version: `v2.7`
- version_bump: `minor`
- hypotheses:
  - `A small doubled-pawn penalty may improve pawn-structure decisions without stacking more passed-pawn bonuses on top of v2.5.`
- implementation_summary: `Cloned v2.5 into v2.7, renamed the public type/search entrypoint, counted pawns per file during evaluation, and subtracted a 12 centipawn penalty for each extra same-color pawn on a file.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `93/311/96`
- score: `248.5`
- score_rate: `0.4970`
- average_plies: `75.10`
- average_processing_time_ms: `103.441`
- average_positions_or_nodes: `14137.95`
- inferred_conclusion: `A generic doubled-pawn penalty slightly underperformed v2.5 and did not improve the passed-pawn baseline. Future pawn-structure work should be more tactical or conversion-specific, such as blockade detection, pawn-race promotion distance, or king proximity to advanced passers, rather than adding broad static structure penalties.`


## Attempt: 2026-06-06T07:57:17Z - v2.8

- status: `approved`
- commit: `90def44`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.5`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_5Engine.cs`
- candidate_version: `v2.8`
- version_bump: `minor`
- hypotheses:
  - `A modest rook open-file and semi-open-file bonus will improve rook activity and conversion decisions beyond v2.5 without materially slowing evaluation.`
- implementation_summary: `Cloned v2.5 into v2.8, renamed the public type/search entrypoint, counted pawn files during evaluation, and added 18 centipawns for rooks on open files or 9 centipawns for rooks on semi-open files.`
- evaluation_log_path: `autoresearch/approved_logs/V2_8Engine-acdf45c-result.csv`
- wins/draws/losses: `247/110/143`
- score: `302.0`
- score_rate: `0.6040`
- average_plies: `91.29`
- average_processing_time_ms: `104.166`
- average_positions_or_nodes: `12100.18`
- inferred_conclusion: `Rook file activity scoring is a small but statistically reliable improvement on top of v2.5. Future work should build from v2.8 and can consider similarly targeted piece-activity terms, but the very narrow margin over v2.5 means new static evaluation terms still need full paired validation rather than relying on raw chess intuition.`


## Attempt: 2026-06-06T08:37:12Z - v2.9

- status: `approved`
- commit: `fcb62a2`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.8`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_8Engine.cs`
- candidate_version: `v2.9`
- version_bump: `minor`
- hypotheses:
  - `A modest knight outpost bonus will improve minor-piece activity decisions on top of v2.8 without materially slowing evaluation.`
  - `Using pawn-defended and not enemy-pawn-attacked advanced knight squares is more targeted than another broad pawn-structure bonus.`
- implementation_summary: `Cloned v2.8 into v2.9, renamed the public type/search entrypoint, and added a 14 centipawn bonus for knights on advanced outpost ranks when defended by a friendly pawn and not attacked by an enemy pawn.`
- evaluation_log_path: `autoresearch/approved_logs/V2_9Engine-66b524e-result.csv`
- wins/draws/losses: `271/106/123`
- score: `324.0`
- score_rate: `0.6480`
- average_plies: `91.84`
- average_processing_time_ms: `104.111`
- average_positions_or_nodes: `12267.83`
- inferred_conclusion: `A small targeted knight outpost term is a statistically reliable improvement on top of v2.8 and did not reduce throughput materially. Future work should build from v2.9 and continue with similarly specific piece-activity or king-safety features rather than generic pawn penalties.`


## Attempt: 2026-06-06T08:56:26Z - v2.10

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.9`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_9Engine.cs`
- candidate_version: `v2.10`
- version_bump: `minor`
- hypotheses:
  - `A small rook-on-seventh-rank activity bonus may improve conversion and attacking pressure on top of v2.9's open-file rook logic.`
- implementation_summary: `Cloned v2.9 into v2.10, renamed the public type/search entrypoint, and added a 16 centipawn bonus for rooks on the opponent's second rank.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `256/97/147`
- score: `304.5`
- score_rate: `0.6090`
- average_plies: `91.39`
- average_processing_time_ms: `104.754`
- average_positions_or_nodes: `11534.74`
- inferred_conclusion: `A generic rook-seventh-rank bonus degraded the stronger v2.9 baseline and reduced node throughput. Future rook activity work should not simply stack another static rook placement bonus on top of open-file scoring; it needs tighter conditions such as trapped king, targets on the seventh rank, or demonstrated conversion-specific compensation.`


## Attempt: 2026-06-07T09:53:14Z - v3.0

- status: `explicitly approved by user`
- commit: `5417662`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v2.9`
- seed_file: `engine_csharp/src/Engine.Core/V2/V2_9Engine.cs`
- candidate_version: `v3.0`
- version_bump: `major`
- hypotheses:
  - `Persisting the native transposition table across moves within a game should reuse prior search work and improve move ordering/cutoffs compared with v2.9's per-move fresh TT allocation.`
  - `Checking a normalized FEN opening lookup before native search should avoid spending the 100ms move budget on early-book positions while preserving legal SAN output through the existing BoardState move path.`
- implementation_summary: `Forked the v2.9 search lineage into V3_0Engine, added an optional V3_0SearchContext with a shared TtEntry array that SearchModels can keep alive for a game and reset between games, and added an OpeningBook.TryGetMove pre-search path keyed by normalized FEN for full-piece opening positions. The v3.0 native search otherwise keeps the v2.9 evaluation lineage, including rook open/semi-open file scoring and knight outpost scoring.`
- evaluation_log_path: `autoresearch/approved_logs/V3_0Engine-5417662-result.csv`
- wins/draws/losses: `254/103/143`
- score: `305.5`
- score_rate: `0.6110`
- average_plies: `100.21`
- average_processing_time_ms: `99.666`
- average_positions_or_nodes: `11522.13`
- inferred_conclusion: `The persistent TT plus opening-lookup architecture is stable enough to become the new approved seed, but its 0.6110 stockfish-1350 score rate is lower than v2.9's 0.6480 reference result. Future v3 experiments should build on the new per-game context/book infrastructure while measuring whether TT reuse, book coverage, and any later search/evaluation changes recover or exceed the previous v2.9 strength.`


## Attempt: 2026-06-08T04:41:43Z - v3.1

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.0`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_0Engine.cs`
- candidate_version: `v3.1`
- version_bump: `minor`
- hypotheses:
  - `A targeted king-shelter term that rewards a real pawn shield in front of a home-rank king and penalizes open or semi-open nearby files will improve middlegame king safety and move choice on top of v3.0.`
- implementation_summary: `Cloned v3.0 into v3.1, renamed the public type and search-context entrypoints, and added a middlegame-scaled king-shelter evaluation bonus that inspects the pawn shield directly in front of a back-rank king and penalizes missing shelter on nearby open or semi-open files.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `140/56/129`
- score: `188.0`
- score_rate: `0.5785`
- average_plies: `98.73`
- average_processing_time_ms: `99.904`
- average_positions_or_nodes: `7514.65`
- inferred_conclusion: `This king-shelter term did not show a clear partial edge over the v3.0 approval bar before the repeated evaluator termination, and it also reduced average node throughput materially versus the v3.0 approved reference. Future v3 work should prefer simpler targeted activity or search-control changes over additional king-safety evaluation unless the mechanism is both cheap per node and robust under the full evaluator contract.`


## Attempt: 2026-06-08T04:44:58Z - v3.2

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.0`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_0Engine.cs`
- candidate_version: `v3.2`
- version_bump: `minor`
- hypotheses:
  - `A generation-aware transposition-table replacement policy will preserve fresher entries in the persistent v3 table and improve move quality at 100ms without adding much per-node cost.`
- implementation_summary: `Added v3.2 as a focused transposition-table experiment that changes TT storage to prefer same-generation updates instead of blindly replacing persistent entries.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `245/91/164`
- score: `290.5`
- score_rate: `0.5810`
- average_plies: `98.59`
- average_processing_time_ms: `100.874`
- average_positions_or_nodes: `8166.80`
- inferred_conclusion: `The TT generation replacement policy produced a stable full run and remained above break-even versus stockfish-1350, but it was still weaker than the approved v3.0 seed and searched fewer positions on average. Future v3 work should treat TT replacement tuning as insufficient on its own and focus on changes that recover throughput or improve move quality more directly.`


## Attempt: 2026-06-08T06:00:46Z - v3.3

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.0`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_0Engine.cs`
- candidate_version: `v3.3`
- version_bump: `minor`
- hypotheses:
  - `A cheap bishop safe-mobility term will improve minor-piece activity decisions on top of v3.0 without the high per-node cost seen in the rejected v3.1 king-shelter scan.`
  - `Rewarding bounded diagonal reach to non-edge squares that are not controlled by enemy pawns is more targeted than retrying a generic bishop-pair bonus or another broad static piece-placement term.`
- implementation_summary: `Cloned v3.0 into v3.3, renamed the public type and search-context entrypoints, and added a capped bishop safe-mobility evaluation bonus that counts diagonal reachable non-edge squares not attacked by enemy pawns.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `251/103/146`
- score: `302.5`
- score_rate: `0.6050`
- average_plies: `100.47`
- average_processing_time_ms: `99.615`
- average_positions_or_nodes: `11980.85`
- inferred_conclusion: `The bounded bishop safe-mobility term recovered some throughput versus the approved v3.0 reference (11980.85 vs 11522.13 average positions/nodes) and remained statistically above break-even, but it weakened raw score slightly and pushed max_plies close to the rejection threshold. Future v3 evaluation work should not add bishop mobility in this form; stronger candidates likely need either better opening/context use or a move-quality change that reduces long capped games while beating the 0.6110 v3.0 score-rate bar.`


## Attempt: 2026-06-08T06:21:42Z - v3.4

- status: `approved`
- commit: `e45c109`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.0`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_0Engine.cs`
- candidate_version: `v3.4`
- version_bump: `minor`
- hypotheses:
  - `Making draw/repetition contempt material-aware will improve score by letting materially worse positions prefer repetition or 50-move draw chances instead of steering away from saves.`
  - `Keeping the existing draw-avoidance penalties for equal-or-better positions should preserve v3.0's decisiveness while reducing avoidable losses.`
- implementation_summary: `Cloned v3.0 into v3.4, renamed the public type and search-context entrypoints, and changed RepetitionDrawAdjustment so materially worse positions receive bounded draw-saving bonuses while equal-or-better positions retain the existing repetition and draw penalties.`
- evaluation_log_path: `autoresearch/approved_logs/V3_4Engine-0398feb-result.csv`
- wins/draws/losses: `277/92/131`
- score: `323.0`
- score_rate: `0.6460`
- average_plies: `97.87`
- average_processing_time_ms: `99.675`
- average_positions_or_nodes: `11335.12`
- inferred_conclusion: `The prior v3.0 draw/repetition contempt was too aggressive when materially worse. Making draw-saving material-aware produced a large raw-score improvement, fewer losses, and stayed under the max-plies threshold despite more willingness to save bad positions. Future v3 work should build from v3.4 and preserve material-aware draw behavior; further improvements should target opening/context use or similarly cheap policies that improve conversion without increasing max_plies_rate beyond 0.10.`


## Attempt: 2026-06-08T17:22:22Z - v3.5

- status: `approved`
- commit: `8b24935`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.4`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_4Engine.cs`
- candidate_version: `v3.5`
- version_bump: `minor`
- hypotheses:
  - `A principal-variation search pass on non-first moves will let v3.5 search more narrowly behind the existing TT move ordering, improving effective depth at 100ms without adding evaluation overhead.`
  - `Applying the same narrow-window re-search policy at the root should preserve move quality while reducing wasted full-window work on clearly inferior alternatives.`
- implementation_summary: `Cloned v3.4 into v3.5 and changed the root and recursive negamax loops to use principal-variation style zero-window searches on non-first ordered moves, with full re-search only when a narrow probe beats the current alpha/best score.`
- evaluation_log_path: `autoresearch/approved_logs/8b24935-result.csv`
- wins/draws/losses: `320/73/107`
- score: `356.5`
- score_rate: `0.7130`
- average_plies: `84.1620`
- average_processing_time_ms: `98.1941`
- average_positions_or_nodes: `11576.6619`
- inferred_conclusion: `The principal-variation zero-window re-search policy produced a large strength gain over v3.4 while keeping failures at zero and reducing capped games, so v3 search should continue to build around cheap search-control improvements that exploit existing move ordering rather than adding heavier per-node evaluation work.`


## Attempt: 2026-06-08T17:59:54Z - v3.6

- status: `approved`
- commit: `62e5166`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.5`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_5Engine.cs`
- candidate_version: `v3.6`
- version_bump: `minor`
- hypotheses:
  - `Root aspiration windows around the previous iteration score will reduce full-window work on stable positions and improve effective depth at 100ms, while fail-low and fail-high re-search keeps the result exact when the window is wrong.`
- implementation_summary: `Cloned v3.5 into v3.6 and wrapped iterative deepening root searches in aspiration windows centered on the previous iteration score, with a factored root-search helper and automatic widening re-search on fail-low or fail-high before storing the exact root result.`
- evaluation_log_path: `autoresearch/approved_logs/62e5166-result.csv`
- wins/draws/losses: `343/49/108`
- score: `367.5`
- score_rate: `0.7350`
- average_plies: `84.2260`
- average_processing_time_ms: `97.8252`
- average_positions_or_nodes: `10700.0111`
- inferred_conclusion: `Root aspiration windows produced another meaningful v3 search-strength gain over the already strong v3.5 seed while keeping failures at zero and further lowering capped games. Future experiments should continue prioritizing cheap root and move-ordering search-control improvements that exploit iterative-deepening score stability, rather than adding heavier per-node evaluation terms.`

## Attempt: 2026-06-08T18:03:52Z - v3.7

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.6`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_6Engine.cs`
- candidate_version: `v3.7`
- version_bump: `minor`
- hypotheses:
  - `A persistent quiet-move ordering layer built from killer moves and quiet history will improve v3.6's PVS and aspiration-window cutoffs at 100ms without adding per-node evaluation cost.`
- implementation_summary: `Cloned v3.6 into v3.7 and extended the search context with persistent killer-move and quiet-history tables, then fed those heuristics into quiet move ordering and updated them on quiet beta cutoffs at the root and recursive negamax nodes.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `n/a/n/a/n/a`
- score: `n/a`
- score_rate: `n/a`
- average_plies: `n/a`
- average_processing_time_ms: `n/a`
- average_positions_or_nodes: `n/a`
- inferred_conclusion: `This attempt failed before evaluation because the move-ordering refactor introduced a build-breaking API mismatch in the sandbox candidate, so it provides no strength signal against v3.6. Future search-control experiments should keep the change surface equally narrow but verify helper accessibility and static-versus-instance call boundaries before handing control back to the orchestrator.`

## Attempt: 2026-06-08T18:09:45Z - v3.8

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.6`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_6Engine.cs`
- candidate_version: `v3.8`
- version_bump: `minor`
- hypotheses:
  - `Persistent killer-move ordering for quiet beta cutoffs will improve v3.6's PVS and aspiration-window searches at 100ms by getting strong refutations to the front faster in later sibling nodes.`
  - `A side-aware quiet-history table keyed by from/to squares will reinforce repeatedly successful quiet cutoffs across positions without changing evaluation cost, giving better move ordering than transposition-table moves alone.`
- implementation_summary: `Cloned v3.6 into v3.8 and added persistent primary/secondary killer moves plus a side-aware quiet-history table to the search context, then used those heuristics to score quiet move ordering and reward quiet beta cutoffs at the root and recursive negamax nodes while keeping the change self-contained in the engine file.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `n/a/n/a/n/a`
- score: `n/a`
- score_rate: `n/a`
- average_plies: `n/a`
- average_processing_time_ms: `n/a`
- average_positions_or_nodes: `n/a`
- inferred_conclusion: `This attempt produced no strength signal because the killer-move and quiet-history ordering changes failed at build time before evaluation. Future search-control experiments should keep the move-ordering surface narrow, verify all new helper signatures and call sites against the board API before return, and prefer compile-checked incremental additions over broader heuristic rewiring.`

## Attempt: 2026-06-08T18:25:38Z - v3.9

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.6`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_6Engine.cs`
- candidate_version: `v3.9`
- version_bump: `minor`
- hypotheses:
  - `Internal iterative deepening at deeper non-check nodes with no transposition-table move will seed a useful hash move for v3.6's existing PVS ordering, improving cutoffs at 100ms without adding evaluation cost.`
- implementation_summary: `Cloned v3.6 into v3.9 and added a contained internal-iterative-deepening fallback in negamax so deeper non-check nodes without a TT move first run a reduced-depth probe to populate and reuse a transposition-table move for the main ordered search.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `329/76/95`
- score: `367.0`
- score_rate: `0.7340`
- average_plies: `87.7500`
- average_processing_time_ms: `98.1286`
- average_positions_or_nodes: `10684.7203`
- inferred_conclusion: `Internal iterative deepening as a TT-move seeding fallback was effectively neutral on top of v3.6: it preserved stability and a strong paired lower bound, but slightly underperformed the approved seed on score_rate while also trimming nodes a bit. Future v3 search-control experiments should avoid spending extra work on generic reduced-depth bootstrap probes unless they produce a clearer move-ordering gain, and should instead target higher-leverage ordering or pruning changes that more directly improve root move selection within the same 100ms budget.`

## Attempt: 2026-06-09T05:47:08Z - v3.10

- status: `approved`
- commit: `41846bf`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.6`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_6Engine.cs`
- candidate_version: `v3.10`
- version_bump: `minor`
- hypotheses:
  - `Late-move reductions for quiet, later-ordered moves should let v3.10 search deeper on low-priority branches without weakening principal-variation re-search when a reduced move proves interesting.`
  - `A narrow killer-move ordering layer for quiet beta cutoffs should improve v3.10's existing PVS move ordering enough to make those late-move reductions safer and more effective at 100ms.`
- implementation_summary: `Cloned v3.6 into v3.10 and added quiet-move killer ordering plus a contained late-move reduction path for later-ordered quiet moves, with full PVS re-search whenever a reduced probe still raises alpha.`
- evaluation_log_path: `autoresearch/approved_logs/V3_10Engine-41846bf-result.csv`
- wins/draws/losses: `682/130/188`
- score: `747.0`
- score_rate: `0.7470`
- average_plies: `86.9570`
- average_processing_time_ms: `98.9572`
- average_positions_or_nodes: `8168.5526`
- inferred_conclusion: `The contained quiet-move killer ordering plus late-move reduction policy produced a real improvement over v3.6 under the full 1000-game Stockfish-1350 contract, clearing the prior 0.7350 score_rate while keeping capped games low and failures at zero. Future v3 work should keep building on cheap search-control changes that improve move ordering and reduce wasted work on low-priority branches, while being careful not to overextend reduction heuristics in ways that increase tactical misses or long drawn games.`

## Attempt: 2026-06-09T06:33:00Z - v3.11

- status: `approved`
- commit: `8214d59`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.10`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_10Engine.cs`
- candidate_version: `v3.11`
- version_bump: `minor`
- hypotheses:
  - `Adding a lightweight bishop mobility term will improve piece-activity move selection more reliably than another generic one-off static bonus.`
  - `Rewarding king-supported advanced passed pawns and penalizing directly blockaded passed pawns in endgames will improve conversion and reduce wasted winning lines.`
- implementation_summary: `Added bishop mobility scoring, added an endgame passed-pawn escort/blockade evaluation term based on king proximity to the square in front of advanced passers, and refactored king PST scoring to use the already-computed endgame phase inside the evaluation snapshot.`
- evaluation_log_path: `autoresearch/approved_logs/V3_11Engine-8214d59-result.csv`
- wins/draws/losses: `685/143/172`
- score: `756.5`
- score_rate: `0.7565`
- average_plies: `86.7240`
- average_processing_time_ms: `98.0551`
- average_positions_or_nodes: `10445.0749`
- inferred_conclusion: `The combined bishop-mobility and king-supported passed-pawn endgame evaluation changes produced a statistically reliable improvement over v3.10 while keeping failures at zero and max-plies safely below the threshold. Future experiments should continue exploring targeted low-cost activity and conversion terms, especially endgame features that change practical winning technique rather than broad generic structure bonuses.`

## Attempt: 2026-06-09T07:01:36Z - v3.12

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.11`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_11Engine.cs`
- candidate_version: `v3.12`
- version_bump: `minor`
- hypotheses:
  - `A modest rook-on-seventh-rank activity bonus should improve practical attacking and conversion move choice with minimal evaluation overhead.`
  - `Rewarding connected advanced passed pawns in endgames should improve promotion-race and winning-line conversion beyond the existing single-passer escort/blockade term.`
- implementation_summary: `Added a rook-on-seventh-rank activity bonus with an extra incentive when the opposing king is stuck on the back rank, and added an endgame-weighted connected passed-pawn bonus for adjacent advanced passers while reusing the evaluator's existing passed-pawn tracking.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `677/151/172`
- score: `752.5`
- score_rate: `0.7525`
- average_plies: `84.6120`
- average_processing_time_ms: `97.8991`
- average_positions_or_nodes: `10439.0495`
- inferred_conclusion: `The added rook-on-seventh and connected-passed-pawn bonuses were directionally plausible but slightly underperformed the v3.11 seed, so stacking more small generic activity/conversion bonuses on top of v3.11 is not enough by itself. Future v3 experiments should favor more selective evaluation terms or search changes that materially alter tactical move choice, rather than broad static bonuses that mostly preserve existing plans.`

## Attempt: 2026-06-09T08:09:15Z - v3.13

- status: `approved`
- commit: `49960e8`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.11`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_11Engine.cs`
- candidate_version: `v3.13`
- version_bump: `minor`
- hypotheses:
  - `Conservative null-move pruning at deeper non-check nodes with non-pawn material should reduce wasted search on clearly safe fail-high branches and improve tactical move selection within the 100ms budget.`
- implementation_summary: `Added a guarded null-move pruning path to negamax with no-null-reply recursion control, a pawns-and-king-only endgame exclusion to reduce zugzwang risk, and native-board null-move make/unmake helpers while preserving the existing LMR/PVS framework.`
- evaluation_log_path: `autoresearch/approved_logs/V3_13Engine-49960e8-result.csv`
- wins/draws/losses: `708/139/153`
- score: `777.5`
- score_rate: `0.7775`
- average_plies: `86.3960`
- average_processing_time_ms: `98.3980`
- average_positions_or_nodes: `11407.9374`
- inferred_conclusion: `The guarded null-move pruning change was a clear improvement over v3.11, raising score_rate from 0.7565 to 0.7775 while keeping failures at zero and max-plies comfortably within the approval threshold. Future v3 experiments should continue exploring selective search-control changes with explicit zugzwang and endgame safety guards, since this branch benefited more from a contained pruning improvement than from stacking additional small static evaluation bonuses.`

## Attempt: 2026-06-09T08:36:59Z - v3.14

- status: `approved`
- commit: `ec54e97`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.13`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_13Engine.cs`
- candidate_version: `v3.14`
- version_bump: `minor`
- hypotheses:
  - `Adding shallow reverse futility pruning at non-check nodes with remaining non-pawn material should cut clearly fail-high branches near the horizon and improve root move selection within the 100ms budget.`
- implementation_summary: `Added guarded reverse futility pruning in negamax for shallow non-check nodes, using the existing static evaluation only when the side to move still has non-pawn material and the margin over beta is comfortably large.`
- evaluation_log_path: `autoresearch/approved_logs/V3_14Engine-ec54e97-result.csv`
- wins/draws/losses: `752/113/135`
- score: `808.5`
- score_rate: `0.8085`
- average_plies: `83.9430`
- average_processing_time_ms: `98.1973`
- average_positions_or_nodes: `12580.9138`
- inferred_conclusion: `The guarded shallow reverse futility pruning change was a strong improvement over v3.13, raising score_rate from 0.7775 to 0.8085 with zero failures and a comfortably acceptable max-plies rate. Future v3 experiments should keep exploring selective shallow pruning and move-filtering ideas that save search on clearly safe branches, while preserving the existing endgame and zugzwang guards that keep aggressive pruning from backfiring.`

## Attempt: 2026-06-09T09:04:13Z - v3.15

- status: `approved`
- commit: `901cf13`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.14`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_14Engine.cs`
- candidate_version: `v3.15`
- version_bump: `minor`
- hypotheses:
  - `Guarded shallow quiet-move futility pruning on later-ordered moves should save search at depth 1-2 in clearly non-improving non-check nodes, improving root move selection within the 100ms budget without changing evaluation.`
- implementation_summary: `Added a shallow quiet-move futility pruning path in negamax for later-ordered quiet moves at depths up to 2, reusing static evaluation when already available from reverse futility pruning and keeping the existing non-check and pawns-and-king-only safety guards.`
- evaluation_log_path: `autoresearch/approved_logs/V3_15Engine-901cf13-result.csv`
- wins/draws/losses: `788/99/113`
- score: `837.5`
- score_rate: `0.8375`
- average_plies: `80.3900`
- average_processing_time_ms: `98.6030`
- average_positions_or_nodes: `10260.1696`
- inferred_conclusion: `The guarded shallow quiet-move futility pruning change was a strong improvement over v3.14, raising score_rate from 0.8085 to 0.8375 while keeping failures at zero and reducing capped games to a low 0.0330 max_plies_rate. Future v3 experiments should continue exploring tightly guarded shallow move-pruning and search-selectivity ideas that trim clearly unpromising quiet branches without relaxing the existing endgame and tactical safety checks.`

## Attempt: 2026-06-09T09:31:57Z - v3.16

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.15`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_15Engine.cs`
- candidate_version: `v3.16`
- version_bump: `minor`
- hypotheses:
  - `A middlegame king-shield bonus that rewards pawns in front of the king and penalizes missing cover will improve defensive move choice more reliably than another search-only heuristic tweak.`
  - `A connected-passed-pawn bonus for adjacent advanced passers will improve endgame conversion beyond the existing single-passer and king-escort terms.`
- implementation_summary: `Added a tapered king-shield evaluation around each king that scores nearby pawn cover only outside pure endgames, and added a connected passed pawn bonus for adjacent advanced passers on neighboring files with similar ranks.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `756/111/133`
- score: `811.5`
- score_rate: `0.8115`
- average_plies: `83.8490`
- average_processing_time_ms: `99.0042`
- average_positions_or_nodes: `10068.7432`
- inferred_conclusion: `The added king-shield and connected-passed-pawn terms preserved stability and remained strong, but they reduced score_rate versus the approved v3.15 baseline and did not justify replacing it. Future v3.x attempts should avoid stacking broad static evaluation bonuses unless they target a clearly missing decision pattern with tighter conditions or are paired with a search change that converts the extra evaluation signal into stronger move selection.`

## Attempt: 2026-06-10T08:11:56Z - v3.17

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.15`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_15Engine.cs`
- candidate_version: `v3.17`
- version_bump: `minor`
- hypotheses:
  - `A conservative depth-1 razoring shortcut at quiet non-check nodes will skip clearly hopeless full move generation while still searching captures through quiescence, improving effective search selectivity on top of v3.15's futility pruning.`
- implementation_summary: `Added a guarded depth-1 razoring check in negamax that reuses static evaluation, excludes check and pawns-and-king-only positions, and falls back to quiescence when static evaluation is safely below alpha.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `778/102/120`
- score: `829.0`
- score_rate: `0.8290`
- average_plies: `83.5790`
- average_processing_time_ms: `98.9151`
- average_positions_or_nodes: `10486.3856`
- inferred_conclusion: `The depth-1 razoring shortcut remained stable with zero crashes, illegal moves, or timeouts, but it underperformed the v3.15 seed at 0.8290 versus 0.8375 and slightly increased capped games to 0.0450. Future v3 attempts should avoid adding this broad pre-move-generation razoring layer on top of the existing futility pruning, and should instead target more selective pruning conditions or move-ordering changes that preserve v3.15's tactical coverage.`

## Attempt: 2026-06-11T07:06:10Z - V4.0

- status: `approved`
- commit: `99698ba`
- evaluator_baseline: `stockfish-1350`
- seed_version: `v3.15`
- seed_file: `engine_csharp/src/Engine.Core/V3/V3_15Engine.cs`
- candidate_version: `V4.0`
- version_bump: `major`
- hypotheses:
  - `A capped side-aware quiet-history table for quiet beta-cutoff moves will improve v3.15-style killer/LMR/futility search ordering without changing evaluation or adding broad pruning risk.`
- implementation_summary: `Added a persistent quiet-history table to the V4.0 search context, scored quiet moves with a small capped history bonus below TT/capture/killer priority, and rewarded quiet beta cutoffs by remaining-depth-squared history increments while preserving the existing evaluation and pruning guards.`
- evaluation_log_path: `autoresearch/approved_logs/V4_0Engine-99698ba-result.csv`
- wins/draws/losses: `796/92/112`
- score: `842.0`
- score_rate: `0.8420`
- average_plies: `80.3890`
- average_processing_time_ms: `98.9959`
- average_positions_or_nodes: `9776.6691`
- inferred_conclusion: `Approved: the capped side-aware quiet-history ordering layer produced a small but real improvement over v3.15, raising score_rate from 0.8375 to 0.8420 with lcb95=0.8246, zero crash/illegal/timeout/harness failures, and max_plies_rate=0.0390. Future v4 experiments should continue favoring narrow move-ordering and search-selectivity changes that improve the existing LMR/futility stack without broadening pruning risk or adding evaluation cost.`

## Baseline Calibration: 2026-06-20 - V4.0 vs stockfish-1800

- evaluator_baseline: `stockfish-1800`
- engine_version: `V4.0`
- engine_file: `engine_csharp/src/Engine.Core/V4/V4_0Engine.cs`
- evaluation_log_path: `autoresearch/approved_logs/v4_0Engine-Stockfish1800-result.csv`
- wins/draws/losses: `143/182/675`
- score: `234.0`
- score_rate: `0.2340`
- average_plies: `100.89`
- average_processing_time_ms: `103.875`
- average_positions_or_nodes: `5245.45`
- paired_mean_score: `0.2340`
- paired_score_sd: `0.2576`
- lcb95: `0.2150`
- failures: `0`
- note: `This calibrates the latest approved seed against the current stockfish-1800 evaluator baseline. Future candidates compare score_rate against 0.2340 until a newer engine is approved under the same baseline.`


## Attempt: 2026-06-20T14:57:41Z - v4.1

- status: `approved`
- commit: `cdcc74f`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.0`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_0Engine.cs`
- candidate_version: `v4.1`
- version_bump: `minor`
- hypotheses:
  - `Quiet checking moves are tactically important enough that they should not be pruned by shallow quiet futility pruning or reduced by late-move reduction merely because they appear late in move ordering.`
  - `Exempting only quiet moves that give check should improve tactical coverage against stockfish-1800 while preserving the existing V4.0 evaluation, null-move, futility, killer, and quiet-history behavior for ordinary quiet moves.`
- implementation_summary: `Added a NativeBoard.MoveGivesCheck helper and used it to keep quiet checking moves out of shallow quiet futility pruning. Negamax now detects whether a searched move gives check after making it, and late-move reduction skips those checking moves while leaving the existing reduction policy unchanged for non-checking quiet moves.`
- evaluation_log_path: `autoresearch/approved_logs/V4_1Engine-v4_1-0620143922-result.csv`
- wins/draws/losses: `191/207/602`
- score: `294.5`
- score_rate: `0.2945`
- improvement_lcb95: `0.0326`
- average_plies: `100.8760`
- average_processing_time_ms: `101.5131`
- average_positions_or_nodes: `6567.6910`
- inferred_conclusion: `Approved under the improvement-bound rule: quiet-check preservation raised score_rate from the v4.0 Stockfish-1800 reference 0.2340 to 0.2945 with improvement_lcb95=0.0326, zero failures, and acceptable max_plies_rate=0.0470. The tactical signal is real but modest, so future work should combine quiet-check protection with higher-impact search or quiescence changes rather than repeatedly retuning the same exemption.`

## Attempt: 2026-06-20T15:18:21Z - v4.2

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.0`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_0Engine.cs`
- candidate_version: `v4.2`
- version_bump: `minor`
- hypotheses:
  - `A very small queen mobility term will improve queen activity and attacking move choice on top of v4.2's existing targeted piece-activity features.`
  - `Reusing the existing ray-mobility helper with a 1 centipawn per reachable queen square scale should keep evaluation overhead low enough for 100ms searches.`
- implementation_summary: `Added a QueenMobilityScale constant and scored queen ray mobility in evaluation using the existing bishop/rook ray mobility helper paths.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `194/190/616`
- score: `289.0`
- score_rate: `0.2890`
- improvement_lcb95: `-0.0345`
- average_plies: `101.8110`
- average_processing_time_ms: `100.9776`
- average_positions_or_nodes: `7098.9991`
- inferred_conclusion: `Rejected under the advancing-reference improvement rule: although queen mobility remained above the original v4.0 calibration, it scored 0.2890 against Stockfish-1800 after v4.1 became the reference, below v4.1's 0.2945 with improvement_lcb95=-0.0345. The small generic queen-mobility term is not a useful successor to v4.1 and should not be promoted as a seed.`

## Attempt: 2026-06-20T15:39:29Z - v4.3

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.0`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_0Engine.cs`
- candidate_version: `v4.3`
- version_bump: `minor`
- hypotheses:
  - `Quiet checking moves are tactically important enough that shallow quiet futility pruning should not discard them before search.`
  - `Skipping late-move reduction for quiet checking moves should improve tactical coverage against stockfish-1800 while preserving existing reductions for ordinary quiet moves.`
- implementation_summary: `Added a NativeBoard.MoveGivesCheck helper and used it to exempt quiet checking moves from shallow quiet futility pruning. Negamax now detects checking moves after making them and skips late-move reduction for those moves while preserving the existing V4.3 evaluation, null-move, reverse futility, killer, and quiet-history behavior.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `198/161/641`
- score: `278.5`
- score_rate: `0.2785`
- improvement_lcb95: `-0.0456`
- average_plies: `100.0820`
- average_processing_time_ms: `101.3758`
- average_positions_or_nodes: `6627.6102`
- inferred_conclusion: `Rejected under the advancing-reference improvement rule: this quiet-check pruning/LMR variant scored 0.2785 against Stockfish-1800 versus the newly approved v4.1 reference 0.2945, with improvement_lcb95=-0.0456. The repeated quiet-check exemption is directionally useful versus v4.0, but weaker than v4.1 and not worth promoting.`

## Attempt: 2026-06-20T16:01:15Z - v4.4

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.0`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_0Engine.cs`
- candidate_version: `v4.4`
- version_bump: `minor`
- hypotheses:
  - `Quiet checking moves are tactically important enough against stockfish-1800 that they should not be discarded by shallow quiet futility pruning or weakened by late-move reduction.`
  - `A tightly limited quiet-check extension at remaining depths up to 2 can improve horizon tactical coverage more than the prior check-pruning exemption alone, while keeping the existing null-move, futility, killer, and quiet-history safeguards intact.`
- implementation_summary: `Added quiet-check awareness in negamax: searched moves now detect whether they give check after make-move, quiet checking moves are excluded from late-move reduction, shallow quiet futility pruning verifies and preserves checking moves, and quiet checks at remaining depths up to 2 receive a one-ply extension.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `199/200/601`
- score: `299.0`
- score_rate: `0.2990`
- improvement_lcb95: `-0.0253`
- average_plies: `102.3270`
- average_processing_time_ms: `101.7022`
- average_positions_or_nodes: `6773.7643`
- inferred_conclusion: `Rejected under the advancing-reference improvement rule: the shallow quiet-check extension scored 0.2990, only slightly above v4.1's 0.2945, and its improvement_lcb95=-0.0253 did not clear the confidence gate. The extension may be directionally useful, but the measured gain over v4.1 is too small for approval.`

## Attempt: 2026-06-20T16:21:57Z - v4.5

- status: `approved`
- commit: `623800e`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.0`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_0Engine.cs`
- candidate_version: `v4.5`
- version_bump: `minor`
- hypotheses:
  - `The current quiescence losing-capture filter is too broad because it skips any non-promotion capture where the attacker is more valuable than the victim, even when the captured piece is loose.`
  - `Only pruning those high-attacker-value captures when the destination is actually defended by the opponent should recover tactical material wins against stockfish-1800 with a limited node-cost increase.`
- implementation_summary: `Changed IsObviouslyLosingCapture so quiescence keeps high-value-attacker captures of lower-value pieces unless the target square is defended by the opponent; promotions and non-captures remain unpruned by this helper.`
- evaluation_log_path: `autoresearch/approved_logs/V4_5Engine-v4_5-0620160421-result.csv`
- wins/draws/losses: `318/170/512`
- score: `403.0`
- score_rate: `0.4030`
- improvement_lcb95: `0.0774`
- average_plies: `96.5110`
- average_processing_time_ms: `101.3519`
- average_positions_or_nodes: `7370.5555`
- inferred_conclusion: `Approved under the improvement-bound rule against the advancing v4.1 reference: the defender-aware quiescence losing-capture filter raised score_rate from 0.2945 to 0.4030 with improvement_lcb95=0.0774, zero failures, and acceptable max_plies_rate=0.0360. This is the strongest standalone V4 signal so far and should be preserved as a seed idea for future tactical-search work.`

## Attempt: 2026-06-20T16:43:26Z - v4.6

- status: `approved`
- commit: `04caa13`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.0`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_0Engine.cs`
- candidate_version: `v4.6`
- version_bump: `minor`
- hypotheses:
  - `Keeping the v4.5 defender-aware quiescence capture filter should recover material-winning captures that the older high-attacker-value filter pruned too broadly.`
  - `Protecting quiet checking moves from shallow quiet futility pruning and late-move reduction, with a one-ply extension only at remaining depths up to 2, should improve tactical horizon coverage while keeping the existing pruning stack bounded.`
- implementation_summary: `Changed quiescence losing-capture pruning to skip high-attacker-value captures only when the destination is opponent-defended. Added MoveGivesCheck and quiet-check-aware negamax handling: quiet checking moves are preserved from shallow quiet futility pruning, excluded from late-move reduction, and receive a one-ply shallow extension. Quiet move status is captured before MakeMove so late-move reduction uses the intended pre-move classification.`
- evaluation_log_path: `autoresearch/approved_logs/V4_6Engine-v4_6-0620162528-result.csv`
- wins/draws/losses: `370/179/451`
- score: `459.5`
- score_rate: `0.4595`
- improvement_lcb95: `0.0226`
- average_plies: `97.1440`
- average_processing_time_ms: `101.4047`
- average_positions_or_nodes: `6531.9059`
- inferred_conclusion: `Approved under the improvement-bound rule against the advancing v4.5 reference: combining the defender-aware quiescence filter with quiet-check protection and a bounded shallow extension raised score_rate from 0.4030 to 0.4595 with improvement_lcb95=0.0226, zero failures, and acceptable max_plies_rate=0.0360. This is the best V4 result so far; future experiments should build from v4.6 and continue with guarded tactical-search improvements rather than generic static evaluation additions.`

## Attempt: 2026-06-20T17:39:00Z - v4.7

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.6`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_6Engine.cs`
- candidate_version: `v4.7`
- version_bump: `minor`
- hypotheses:
  - `Combining the approved v4.1 quiet-check preservation with the approved v4.5 defender-aware quiescence capture filter and the approved v4.6 shallow quiet-check extension should improve tactical horizon coverage when the search reaches quiescence.`
  - `A single bounded quiet-check ply inside quiescence should let forcing non-capture checks interact with defender-aware capture pruning without opening the full quiet-move tree or materially increasing max-plies failures.`
- implementation_summary: `Cloned v4.6 into v4.7 and preserved the approved v4.1 quiet-check futility/LMR protection, v4.5 defender-aware quiescence capture filter, and v4.6 shallow quiet-check extension. Added one bounded quiet-check ply to quiescence: when not in check, quiescence still searches captures and promotions, but may also search quiet moves that give check, consuming a one-ply quiet-check budget so non-checking quiet moves remain excluded.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `226/186/588`
- score: `319.0`
- score_rate: `0.3190`
- improvement_lcb95: `-0.1734`
- average_plies: `103.9420`
- average_processing_time_ms: `105.3743`
- average_positions_or_nodes: `3329.5878`
- inferred_conclusion: `Rejected: adding a bounded quiet-check ply inside quiescence caused a major regression versus the approved v4.6 seed, dropping score_rate from 0.4595 to 0.3190 against stockfish-1800 with improvement_lcb95=-0.1734, zero failures, and max_plies_rate=0.0450. The canonical CSV shows average positions/nodes fell from v4.6's 6531.91 to 3329.59, matching the evaluator summary's visible node-rate collapse. The quiescence quiet-check pass appears to spend too much 100ms budget generating and testing quiet checking moves at horizon nodes, reducing effective main-search coverage. Future work should keep the approved v4.1/v4.5/v4.6 normal-search and defender-aware quiescence changes, but avoid broad quiet-check expansion in quiescence unless it is much more selective, ordered, or triggered only by concrete tactical conditions.`

## Attempt: 2026-06-20T18:20:32Z - v4.8

- status: `approved`
- commit: `<pending-manual-commit>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.6`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_6Engine.cs`
- candidate_version: `v4.8`
- version_bump: `minor`
- hypotheses:
  - `Moving the non-check quiescence stand-pat beta cutoff before capture generation will recover 100ms search budget lost to unnecessary legal-capture generation and ordering in already-good horizon positions.`
  - `Keeping V4.6's approved defender-aware capture filter and normal-search quiet-check extension intact, while only optimizing quiescence control flow, should improve effective depth without repeating V4.7's broad quiet-check expansion failure.`
- implementation_summary: `Cloned v4.6 into v4.8 and preserved the approved V4.5 defender-aware quiescence capture filter plus V4.6 quiet-check futility/LMR protection and shallow extension. In quiescence, non-check nodes now evaluate stand-pat and return on beta cutoff before generating and ordering capture moves; in-check nodes still generate legal evasions before evaluation because stand-pat is not legal.`
- evaluation_log_path: `autoresearch/approved_logs/V4_8Engine-385a585-result.csv`
- wins/draws/losses: `382/195/423`
- score: `479.5`
- score_rate: `0.4795`
- improvement_lcb95: `-0.0139`
- average_plies: `96.6290`
- average_processing_time_ms: `100.8318`
- average_positions_or_nodes: `7542.3228`
- inferred_conclusion: `Approved by manual override: moving the non-check quiescence stand-pat beta cutoff before capture generation improved raw score_rate from v4.6's 0.4595 to 0.4795 and raised node throughput from 6531.91 to 7542.32 average positions/nodes, with zero failures and max_plies_rate=0.0290. This did not clear the normal statistical gate because improvement_lcb95=-0.0139, but it is being kept as the next seed to preserve the directionally positive combined changes before continuing autoresearch. Future work should treat v4.8 as a pragmatic seed, not as a statistically proven improvement, and pair this budget-saving quiescence change with another low-cost move-quality improvement.`


## Attempt: 2026-06-20T18:52:11Z - v4.9

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.9`
- version_bump: `minor`
- hypotheses:
  - `Making quiescence losing-capture pruning less aggressive for near-equal defended captures will preserve tactical minor-piece exchanges while still skipping clearly bad high-value captures.`
- implementation_summary: `Added a 100 centipawn material-loss margin to the defender-aware quiescence losing-capture filter, so defended captures are pruned only when the attacker exceeds the captured material by at least that margin; near-equal defended captures such as bishop-for-knight exchanges remain searchable.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `357/195/448`
- score: `454.5`
- score_rate: `0.4545`
- average_plies: `97.8290`
- average_processing_time_ms: `100.9907`
- average_positions_or_nodes: `7533.2822`
- inferred_conclusion: `Rejected: relaxing the defender-aware quiescence losing-capture filter to keep near-equal defended captures reduced score_rate from the approved v4.8 seed's 0.4795 to 0.4545, with improvement_lcb95=-0.0581 and no failure issues. The added tactical coverage did not compensate for the extra quiescence work or changed capture choices, while max_plies_rate remained acceptable at 0.0310. Future V4 experiments should preserve the stricter v4.8 defended-capture pruning and look for lower-cost tactical-search improvements rather than broadening quiescence capture coverage.`

## Attempt: 2026-06-20T19:12:43Z - v4.10

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.10`
- version_bump: `minor`
- hypotheses:
  - `Quiet checking moves are already tactically protected in v4.8, but when they are ordered late they can still consume extra search effort before receiving their extension.`
  - `Adding a bounded top-ply move-order bonus for quiet checking moves should improve tactical coverage without repeating v4.7's expensive quiescence quiet-check expansion.`
- implementation_summary: `Added a QuietCheckMoveOrderBonus and applied it only to quiet moves at plies within the existing quiet-check extension depth when MoveGivesCheck is true, preserving v4.8 quiescence behavior and all public APIs.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `361/173/466`
- score: `447.5`
- score_rate: `0.4475`
- average_plies: `98.2020`
- average_processing_time_ms: `100.9633`
- average_positions_or_nodes: `6558.3626`
- inferred_conclusion: `Rejected: the bounded quiet-check move-order bonus reduced score_rate from the approved v4.8 seed's 0.4795 to 0.4475, with improvement_lcb95=-0.0651. Node throughput also fell materially to 6558.36 average positions/nodes versus v4.8's 7542.32, while failures remained zero and max_plies_rate was acceptable at 0.0410. Future V4 work should avoid adding make/unmake-based quiet-check ordering tests across move ordering, even when depth-bounded, because the added overhead appears to reduce effective 100ms search coverage more than the ordering improvement helps.`

## Attempt: 2026-06-20T19:33:32Z - v4.11

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.11`
- version_bump: `minor`
- hypotheses:
  - `Scoring stalemate and already-drawn repetition/50-move nodes as exact draws will improve conversion and drawing defense versus v4.8, because the current search can treat stalemate as a material-winning or material-losing evaluated position instead of a terminal draw.`
- implementation_summary: `Added exact draw scoring for threefold/50-move positions in main search and quiescence, and changed no-legal-move non-check nodes to return a draw score instead of static evaluation.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `376/181/443`
- score: `466.5`
- score_rate: `0.4665`
- average_plies: `98.6870`
- average_processing_time_ms: `101.0656`
- average_positions_or_nodes: `7440.8966`
- inferred_conclusion: `Rejected: exact terminal draw scoring for repetition/50-move states plus stalemate did not improve over the approved v4.8 seed, scoring 0.4665 versus 0.4795 with improvement_lcb95=-0.0468. The change stayed stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0300, but likely removed some useful draw-contempt behavior or had too little positive impact to offset changed endgame choices. Future V4 work should preserve v4.8's practical draw-adjustment behavior and focus on lower-cost tactical search or ordering improvements rather than broad exact draw normalization.`

## Attempt: 2026-06-20T19:54:15Z - v4.12

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.12`
- version_bump: `minor`
- hypotheses:
  - `A cheap king-race bonus for advanced passed pawns in endgames will improve conversion decisions on top of v4.8 without expanding quiescence or adding broad move-order overhead.`
  - `Restricting the passed-pawn blockade penalty to defended enemy blockaders should avoid undervaluing passers blocked by loose pieces that may be tactically removable.`
- implementation_summary: `Added a small endgame-weighted passed-pawn race bonus when the enemy king is too far from the promotion square, and changed the existing direct-front blockade penalty to apply only when the enemy blockader is defended.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `372/193/435`
- score: `468.5`
- score_rate: `0.4685`
- average_plies: `96.7030`
- average_processing_time_ms: `101.0557`
- average_positions_or_nodes: `7313.0888`
- inferred_conclusion: `Rejected: the passed-pawn king-race bonus plus defended-blockader refinement scored 0.4685 versus the approved v4.8 reference 0.4795, with improvement_lcb95=-0.0439. The change was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0440, but reduced score and node throughput from v4.8's 7542.32 to 7313.09 average positions/nodes. Future V4 work should not add further endgame passed-pawn tuning on this path unless it is more selective or backed by tactical search support; preserve v4.8's practical quiescence/search behavior and look for lower-cost improvements with clearer tactical impact.`

## Attempt: 2026-06-20T20:15:00Z - v4.13

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.13`
- version_bump: `minor`
- hypotheses:
  - `Allowing immediate non-capture pawn promotions in quiescence should fix a narrow horizon blind spot without repeating the expensive broad quiet-check expansion from v4.7.`
- implementation_summary: `Changed pawn move generation so capture-only/quiescence move lists still include one-square promotion pushes when the promotion square is empty, while preserving the existing exclusion of ordinary quiet pawn pushes and double pushes.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `391/166/443`
- score: `474.0`
- score_rate: `0.4740`
- average_plies: `98.0230`
- average_processing_time_ms: `100.9505`
- average_positions_or_nodes: `7352.6299`
- inferred_conclusion: `Rejected: allowing immediate non-capture promotions in capture-only/quiescence move generation was stable with zero crash/illegal/timeout/harness failures and max_plies_rate=0.0290, but it scored 0.4740 versus the approved v4.8 reference 0.4795 with improvement_lcb95=-0.0396. The narrow horizon fix did not compensate for the added quiescence work or changed promotion handling, with average nodes falling to 7352.63 versus v4.8's 7542.32. Future V4 work should preserve v4.8's stricter quiescence budget behavior and avoid adding quiet horizon moves, including promotions, unless gated by a stronger tactical condition.`

## Attempt: 2026-06-20T20:36:15Z - v4.14

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.14`
- version_bump: `minor`
- hypotheses:
  - `A modest bishop-pair bonus should improve minor-piece trade and middlegame evaluation decisions on top of v4.8-derived piece activity without adding search overhead or widening quiescence.`
- implementation_summary: `Added a 24 centipawn bishop-pair positional bonus for each side when its existing evaluation scan counts at least two bishops.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `407/156/437`
- score: `485.0`
- score_rate: `0.4850`
- average_plies: `96.1230`
- average_processing_time_ms: `100.9285`
- average_positions_or_nodes: `7460.0190`
- inferred_conclusion: `Rejected: the 24 centipawn bishop-pair bonus was directionally positive on raw score_rate, improving from the approved v4.8 reference 0.4795 to 0.4850, but it did not clear the paired confidence gate with improvement_lcb95=-0.0277. The change was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0310, while average nodes were slightly below v4.8 at 7460.02 versus 7542.32. Future work should not rely on another small generic static evaluation bonus to pass approval; prefer changes with clearer tactical or search impact, or a more selective bishop-pair condition tied to open positions or endgame conversion.`

## Attempt: 2026-06-20T20:56:10Z - v4.15

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.15`
- version_bump: `minor`
- hypotheses:
  - `A bishop-pair bonus should be more useful when enough pawns have been exchanged for the bishops' long-range mobility to matter.`
  - `Gating the bonus by total pawn count should retain the directionally positive v4.14 bishop-pair signal while avoiding a broad static bonus in closed positions.`
- implementation_summary: `Added a 28 centipawn bishop-pair bonus for each side only when the total pawn count is 12 or less, preserving v4.8-derived search and quiescence behavior.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `377/181/442`
- score: `467.5`
- score_rate: `0.4675`
- average_plies: `97.3500`
- average_processing_time_ms: `100.9923`
- average_positions_or_nodes: `7297.4536`
- inferred_conclusion: `Rejected: the open-position gated bishop-pair bonus scored 0.4675 versus the approved v4.8 seed's 0.4795, with improvement_lcb95=-0.0457. It was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0390, but reduced average nodes to 7297.45 from v4.8's 7542.32 and did not preserve the directionally positive v4.14 generic bishop-pair signal. Future V4 work should avoid further bishop-pair bonus variants on this path and prioritize low-cost tactical/search changes or more concrete conversion features.`

## Attempt: 2026-06-20T21:16:22Z - v4.16

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.16`
- version_bump: `minor`
- hypotheses:
  - `Caching the per-node static evaluation used by shallow quiet futility pruning will recover search budget at 100ms without changing move ordering, evaluation values, or pruning decisions.`
- implementation_summary: `Changed quiet futility pruning to receive the nullable static evaluation by reference and populate it once per node, so multiple quiet futility checks reuse the same evaluation instead of recomputing it for each candidate move.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `397/174/429`
- score: `484.0`
- score_rate: `0.4840`
- average_plies: `96.9520`
- average_processing_time_ms: `100.9391`
- average_positions_or_nodes: `7511.0359`
- inferred_conclusion: `Rejected: caching the per-node static evaluation for quiet futility pruning was stable and directionally positive on raw score_rate, improving from v4.8's 0.4795 reference to 0.4840 with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0380. However, it did not clear the paired confidence gate with improvement_lcb95=-0.0293, and average nodes slightly declined to 7511.04 versus v4.8's 7542.32. Future work may preserve this as a low-risk cleanup only if combined with a stronger independent improvement, but it should not be treated as a standalone approved strength gain.`

## Attempt: 2026-06-20T21:37:01Z - v4.17

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.17`
- version_bump: `minor`
- hypotheses:
  - `Caching per-node static evaluation and the pawns-and-king-only guard across reverse futility, null-move gating, and quiet futility should recover 100ms search budget without changing pruning thresholds or evaluation semantics.`
- implementation_summary: `Negamax now keeps a nullable per-node only-pawns-and-king result alongside the existing nullable static evaluation. Reverse futility pruning, null-move pruning, and quiet futility pruning reuse those cached values; quiet futility now populates staticEval by reference so later quiet moves in the same node avoid recomputing evaluation.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `368/187/445`
- score: `461.5`
- score_rate: `0.4615`
- average_plies: `97.9920`
- average_processing_time_ms: `100.8832`
- average_positions_or_nodes: `7409.8898`
- inferred_conclusion: `Rejected: caching the pawns-and-king-only guard alongside static evaluation reduced score_rate from the approved v4.8 seed's 0.4795 to 0.4615 with improvement_lcb95=-0.0509. The change stayed stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0390, but average nodes fell to 7409.89 versus v4.8's 7542.32 and move quality regressed. Future V4 experiments should avoid further semantics-preserving guard/evaluation caching in this path unless compile-time profiling identifies a clear hotspot; prioritize tactical search or ordering changes with direct move-quality impact.`

## Attempt: 2026-06-20T21:57:25Z - v4.18

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.18`
- version_bump: `minor`
- hypotheses:
  - `Caching the per-node static evaluation used by shallow quiet futility pruning will avoid repeated evaluation work within the same node while preserving pruning thresholds and move semantics.`
  - `Combining that low-risk budget cleanup with the previously directionally positive 24 centipawn bishop-pair evaluation term may provide enough independent move-quality signal to improve over the v4.8 reference.`
- implementation_summary: `Changed quiet futility pruning to populate the nullable static evaluation by reference so later quiet futility checks in the same node reuse it, and added a 24 centipawn bishop-pair positional bonus for each side when the existing evaluation scan counts at least two bishops.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `410/163/427`
- score: `491.5`
- score_rate: `0.4915`
- average_plies: `95.5360`
- average_processing_time_ms: `100.7331`
- average_positions_or_nodes: `7397.5725`
- inferred_conclusion: `Rejected: combining per-node quiet-futility static-eval caching with the 24 centipawn bishop-pair bonus improved raw score_rate to 0.4915 versus the approved v4.8 reference at 0.4795, but did not clear the paired confidence gate with improvement_lcb95=-0.0214. The run was stable with zero crash/illegal/timeout/harness failures and max_plies_rate=0.0300, but average nodes fell to 7397.57 versus v4.8's 7542.32. Future work may treat this combination as a directionally positive signal, but it is still not strong enough as a standalone successor; prioritize changes that preserve or improve throughput while adding clearer tactical move-quality impact rather than stacking small static evaluation and caching tweaks.`

## Attempt: 2026-06-20T22:17:50Z - v4.19

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.19`
- version_bump: `minor`
- hypotheses:
  - `Caching the per-node static evaluation used by shallow quiet futility pruning will avoid repeated evaluation work within the same node while preserving pruning thresholds and move semantics.`
  - `Trying eligible null-move pruning before full legal move generation will recover 100ms search budget on beta-cutoff nodes without widening quiescence or adding evaluation overhead.`
- implementation_summary: `Changed quiet futility pruning to populate the nullable static evaluation by reference so later quiet futility checks in the same node reuse it. Moved the existing null-move pruning block ahead of ordered legal move generation while preserving its depth, check, and pawns-only guards.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `373/171/456`
- score: `458.5`
- score_rate: `0.4585`
- average_plies: `97.4970`
- average_processing_time_ms: `100.7305`
- average_positions_or_nodes: `7565.2786`
- inferred_conclusion: `Rejected: moving null-move pruning ahead of legal move generation plus per-node quiet-futility static-eval caching increased average nodes slightly to 7565.28 versus the v4.8 seed's 7542.32, but reduced score_rate to 0.4585 against the approved 0.4795 reference with improvement_lcb95=-0.0540. The run was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0420, so the regression appears to be move-quality/search-shape related rather than a reliability issue. Future V4 work should avoid reordering null-move pruning before legal move generation in this engine despite the small throughput gain; any static-eval caching should only be reused if paired with an independent move-quality improvement and preferably without changing pruning order.`

## Attempt: 2026-06-20T22:38:11Z - v4.20

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.20`
- version_bump: `minor`
- hypotheses:
  - `Caching the per-node static evaluation used by shallow quiet futility pruning will avoid repeated evaluation work within the same node while preserving pruning thresholds and move semantics.`
  - `Scoring only true stalemate nodes as exact draws should improve conversion and drawing defense without repeating the rejected broader exact repetition/50-move normalization.`
- implementation_summary: `Changed quiet futility pruning to populate the nullable static evaluation by reference so later quiet futility checks in the same node reuse it, and changed no-legal-move non-check nodes to return an exact draw score while leaving repetition and 50-move draw contempt behavior unchanged.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `366/199/435`
- score: `465.5`
- score_rate: `0.4655`
- average_plies: `97.9560`
- average_processing_time_ms: `100.8397`
- average_positions_or_nodes: `7433.0399`
- inferred_conclusion: `Rejected: combining per-node quiet-futility static-eval caching with exact stalemate draw scoring reduced score_rate to 0.4655 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0463. The run was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0420, but average nodes fell to 7433.04 versus v4.8's 7542.32 and move quality regressed. Future V4 work should avoid pairing the quiet-futility caching cleanup with stalemate exact-draw scoring; exact draw normalization appears to weaken this engine's practical draw-contempt behavior, and caching should only be reused with a stronger independent move-quality improvement.`

## Attempt: 2026-06-20T22:59:38Z - v4.21

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.21`
- version_bump: `minor`
- hypotheses:
  - `A modest bishop-pair bonus should preserve the directionally positive v4.14 signal while adding no search overhead or quiescence expansion.`
  - `Extending only safe checking captures at the final normal-search ply should improve tactical horizon coverage without repeating the costly quiet-check expansion failures.`
- implementation_summary: `Added a 24 centipawn bishop-pair positional bonus for each side and added a one-ply extension for checking captures at remaining depth 1 when the capture is not obviously losing, preserving v4.8-derived quiescence and pruning behavior.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `377/198/425`
- score: `476.0`
- score_rate: `0.4760`
- average_plies: `97.9640`
- average_processing_time_ms: `100.9137`
- average_positions_or_nodes: `7383.9649`
- inferred_conclusion: `Rejected: the bishop-pair bonus plus safe checking-capture extension scored 0.4760 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0369. The run was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0430, but average nodes fell to 7383.96 versus v4.8's 7542.32 and the added tactical/evaluation coverage did not improve move quality enough. Future V4 work should avoid combining small static bishop-pair tuning with extra horizon extensions on this path, preserve v4.8's quiescence budget behavior, and seek lower-overhead tactical-search changes or stronger independent move-quality signals.`

## Attempt: 2026-06-20T23:19:57Z - v4.22

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.22`
- version_bump: `minor`
- hypotheses:
  - `A modest pawn-shield bonus for back-rank or second-rank kings while queens remain will improve king-safety decisions beyond the existing king piece-square table without adding search overhead.`
  - `Scaling the shield term out with endgame weight should avoid fighting the existing endgame king-centralization and mop-up logic.`
- implementation_summary: `Added middle-game-scaled pawn-shield scoring around each king using friendly pawns one and two ranks in front of the king, gated to queen-present positions and home-side king ranks.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `388/168/444`
- score: `472.0`
- score_rate: `0.4720`
- average_plies: `96.0160`
- average_processing_time_ms: `100.8864`
- average_positions_or_nodes: `7301.4332`
- inferred_conclusion: `Rejected: the queen-present king pawn-shield term reduced score_rate to 0.4720 versus the approved 0.4795 baseline, with improvement_lcb95 -0.0408. The feature did not cause crashes, illegal moves, or excessive max-plies games, but it appears to distort move choice enough to underperform; future attempts should avoid adding generic king-shelter bonuses on top of the current king PST/search mix unless they are tied to concrete attack/line-opening signals.`

## Attempt: 2026-06-20T23:39:28Z - v4.23

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.23`
- version_bump: `minor`
- hypotheses:
  - `Caching the per-node static evaluation used by shallow quiet futility pruning will avoid repeated evaluation work within the same node while preserving pruning thresholds, move ordering, and evaluation semantics.`
- implementation_summary: `Changed quiet futility pruning to receive the nullable static evaluation by reference and populate it once per node, so later quiet futility checks in the same node reuse the cached evaluation instead of recomputing it.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `391/180/429`
- score: `481.0`
- score_rate: `0.4810`
- average_plies: `95.6370`
- average_processing_time_ms: `100.9252`
- average_positions_or_nodes: `7408.1962`
- inferred_conclusion: `Rejected: per-node quiet-futility static-eval caching was stable and slightly improved raw score_rate to 0.4810 versus the approved 0.4795 reference, but it did not clear the paired confidence gate with improvement_lcb95=-0.0321. Average nodes fell to 7408.20 versus the v4.8 reference, so this cleanup should not be treated as a standalone strength gain; future V4 attempts need a stronger independent move-quality improvement rather than relying on semantics-preserving caching.`

## Attempt: 2026-06-20T23:59:12Z - v4.24

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.24`
- version_bump: `minor`
- hypotheses:
  - `Caching the per-node static evaluation used by shallow quiet futility pruning will avoid repeated evaluation work within the same node while preserving pruning thresholds, move ordering, and evaluation semantics.`
- implementation_summary: `Changed quiet futility pruning to receive the nullable static evaluation by reference and populate it once per node, so later quiet futility checks in the same node reuse the cached evaluation instead of recomputing it.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `380/186/434`
- score: `473.0`
- score_rate: `0.4730`
- average_plies: `96.4270`
- average_processing_time_ms: `100.7624`
- average_positions_or_nodes: `7478.9818`
- inferred_conclusion: `Rejected: per-node quiet-futility static-eval caching reduced score_rate to 0.4730 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0393. The change was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0320, while average nodes were 7478.98 versus the v4.8 reference. Future V4 attempts should not rely on this semantics-preserving caching as a standalone improvement; prioritize stronger move-quality changes and avoid further quiet-futility caching variants unless paired with an independently compelling tactical or evaluation gain.`

## Attempt: 2026-06-21T00:19:30Z - v4.25

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.25`
- version_bump: `minor`
- hypotheses:
  - `A small side-to-move tempo bonus should improve initiative and move-choice discrimination on top of v4.8 without adding search overhead or touching quiescence behavior.`
  - `Disabling the tempo bonus in pure pawn endgames should avoid encouraging zugzwang-prone positions where having the move can be a liability.`
- implementation_summary: `Added an 8 centipawn tempo bonus to evaluation when either side still has non-pawn material, preserving v4.8-derived search, pruning, move generation, and quiescence behavior.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `385/174/441`
- score: `472.0`
- score_rate: `0.4720`
- average_plies: `98.0410`
- average_processing_time_ms: `100.7322`
- average_positions_or_nodes: `7610.7792`
- inferred_conclusion: `Rejected: the 8 centipawn non-pawn-material tempo bonus increased average nodes slightly versus v4.8 but reduced score_rate to 0.4720 against the approved 0.4795 reference, with improvement_lcb95=-0.0413. The change was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0320, but the added initiative bias appears to distort move choice more than it helps. Future V4 attempts should avoid generic tempo tuning on this path and prioritize more concrete tactical or search-control improvements that preserve v4.8's quiescence budget behavior.`

## Attempt: 2026-06-21T00:40:06Z - v4.26

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.26`
- version_bump: `minor`
- hypotheses:
  - `Exempting existing killer moves from late-move reduction should let previously observed quiet beta-cutoff moves receive full-depth verification when they appear later in move order.`
  - `Exempting high quiet-history moves from late-move reduction should improve tactical search allocation at 100ms without changing evaluation or quiescence behavior.`
- implementation_summary: `Changed late-move reduction so quiet moves matching the current ply's primary/secondary killer or reaching at least half the quiet-history cap are searched at the normal child depth instead of the reduced probe depth.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `374/202/424`
- score: `475.0`
- score_rate: `0.4750`
- average_plies: `99.3380`
- average_processing_time_ms: `101.0961`
- average_positions_or_nodes: `7375.8620`
- inferred_conclusion: `Rejected: exempting killer and high quiet-history moves from late-move reduction reduced score_rate to 0.4750 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0377. The change was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0440, but average nodes fell to 7375.86, suggesting the extra full-depth quiet searches cost budget without improving move quality. Future V4 work should avoid broad LMR exemptions based only on killer/history signals; any LMR tuning should be more selective or paired with evidence that it preserves throughput.`

## Attempt: 2026-06-21T01:00:21Z - v4.27

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.27`
- version_bump: `minor`
- hypotheses:
  - `Doubling the persistent transposition-table size from 2^18 to 2^19 entries will reduce collision churn across a game and improve move reuse at 100ms without changing evaluation, pruning thresholds, quiescence behavior, or move generation.`
- implementation_summary: `Increased TtSizeBits from 18 to 19, doubling the V4_27SearchContext transposition table from 262144 to 524288 entries while preserving all public APIs and search/evaluation semantics.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `385/173/442`
- score: `471.5`
- score_rate: `0.4715`
- average_plies: `96.4010`
- average_processing_time_ms: `100.9424`
- average_positions_or_nodes: `7375.7488`
- inferred_conclusion: `Rejected: doubling the persistent TT from 2^18 to 2^19 reduced score_rate to 0.4715 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0416. The run was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0390, but average nodes fell to 7375.75, so the larger table likely hurt cache locality or search shape more than it reduced useful collisions. Future V4 work should keep the v4.8 table size and avoid pure TT-capacity increases unless paired with evidence of collision pressure or a replacement-policy change that preserves throughput.`

## Attempt: 2026-06-21T01:21:00Z - v4.28

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.28`
- version_bump: `minor`
- hypotheses:
  - `Root-level quiet beta cutoffs are strong per-game ordering signals that should feed the existing side-aware quiet-history table, improving later move ordering without changing evaluation, pruning thresholds, quiescence behavior, or adding extra move generation.`
- implementation_summary: `Changed quiet beta-cutoff handling so ply-0 quiet cutoffs reward quiet history while still avoiding root killer-slot updates; non-root killer and quiet-history behavior is otherwise unchanged.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `371/192/437`
- score: `467.0`
- score_rate: `0.4670`
- average_plies: `100.7900`
- average_processing_time_ms: `101.1606`
- average_positions_or_nodes: `7383.1709`
- inferred_conclusion: `Rejected: feeding root-level quiet beta cutoffs into quiet history reduced score_rate to 0.4670 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0456. The run was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0560, but average nodes fell to 7383.17 versus v4.8's 7542.32 and move quality regressed. Future V4 work should avoid root quiet-history reinforcement in this form; root cutoffs likely overfit aspiration-window/search-order artifacts and pollute later per-game quiet ordering more than they help.`

## Attempt: 2026-06-21T01:41:00Z - v4.29

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.29`
- version_bump: `minor`
- hypotheses:
  - `Removing the make/unmake-based quiet-check veto from already-guarded shallow quiet futility pruning will recover 100ms search budget without touching evaluation, move ordering, quiescence generation, or the existing depth/non-check/pawn-endgame safety gates.`
- implementation_summary: `Changed CanFutilityPruneQuietMove so eligible shallow quiet moves are pruned directly when static evaluation plus the futility margin cannot reach alpha, instead of first calling MoveGivesCheck for each pruneable quiet move.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `369/188/443`
- score: `463.0`
- score_rate: `0.4630`
- average_plies: `95.9910`
- average_processing_time_ms: `100.7497`
- average_positions_or_nodes: `7600.0031`
- inferred_conclusion: `Rejected: removing the quiet-check veto from shallow quiet futility pruning increased average nodes to 7600.00 versus the v4.8 reference but reduced score_rate to 0.4630, with improvement_lcb95=-0.0491. The change was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0330, so the regression is a move-quality issue rather than reliability. Future V4 attempts should preserve the existing MoveGivesCheck safety check in quiet futility pruning despite its make/unmake overhead; pruning quiet checking moves at shallow depth loses too much tactical coverage.`

## Attempt: 2026-06-21T02:01:05Z - v4.30

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.30`
- version_bump: `minor`
- hypotheses:
  - `Quiescence delta pruning should preserve capture moves that give check, because these are concrete tactical forcing moves and can matter even when their immediate material swing is below alpha.`
- implementation_summary: `Changed quiescence delta-pruning so a capture that would otherwise be delta-pruned is still searched if making the move gives check, paying the make/unmake check test only for already-pruneable captures.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `378/150/472`
- score: `453.0`
- score_rate: `0.4530`
- average_plies: `95.9230`
- average_processing_time_ms: `101.0389`
- average_positions_or_nodes: `7329.5315`
- inferred_conclusion: `Rejected: preserving delta-pruned quiescence captures that give check reduced score_rate to 0.4530 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0595. The run was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0300, but average nodes fell to 7329.53 from v4.8's 7542.32 and move quality regressed. Future V4 work should avoid adding make/unmake-based checking-capture exemptions to quiescence delta pruning; preserve v4.8's strict quiescence budget and look for lower-overhead tactical or ordering changes.`

## Attempt: 2026-06-21T02:21:27Z - v4.31

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.31`
- version_bump: `minor`
- hypotheses:
  - `A small recapture move-ordering bonus should improve alpha-beta cutoff quality in tactical exchanges without changing evaluation, pruning thresholds, quiescence breadth, or legal move generation.`
- implementation_summary: `Tracked the previous real move destination through native board make/unmake state and added a 1200-point ordering bonus for captures landing on that square, preserving all public APIs and search/evaluation semantics.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `358/169/473`
- score: `442.5`
- score_rate: `0.4425`
- average_plies: `97.5430`
- average_processing_time_ms: `101.1412`
- average_positions_or_nodes: `7369.9932`
- inferred_conclusion: `Rejected: the recapture move-ordering bonus reduced score_rate to 0.4425 versus the approved v4.8 reference at 0.4795, with improvement_lcb95=-0.0704. The run was stable with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0400, but average nodes fell to 7369.99 and move quality regressed sharply. Future V4 work should avoid this simple last-destination recapture ordering bonus; it likely over-prioritizes exchange continuations and disrupts the existing MVV/LVA, TT, killer, and history ordering balance.`

## Attempt: 2026-06-21T03:56:36Z - v4.32

- status: `rejected`
- commit: `<n/a>`
- evaluator_baseline: `stockfish-1800`
- seed_version: `v4.8`
- seed_file: `engine_csharp/src/Engine.Core/V4/V4_8Engine.cs`
- candidate_version: `v4.32`
- version_bump: `minor`
- hypotheses:
  - `Prioritizing quiet checking moves at the root and first two descendant plies will expose forcing queen/bishop checking nets earlier in alpha-beta without changing evaluation, pruning thresholds, or quiescence breadth.`
  - `Restricting the check-ordering bonus to early quiet moves should keep the overhead localized to tactically critical root continuations instead of repeating rejected broad quiet-check or quiescence expansions.`
- implementation_summary: `Added an early-ply quiet-check move-ordering bonus for quiet moves at ply 0-2 by reusing the existing MoveGivesCheck test during ordering; all evaluation, pruning, move generation, quiescence filtering, TT sizing, and public API shape are unchanged.`
- evaluation_log_path: `<n/a>`
- wins/draws/losses: `367/170/463`
- score: `452.0`
- score_rate: `0.4520`
- average_plies: `99.6820`
- average_processing_time_ms: `101.2474`
- average_positions_or_nodes: `6490.8536`
- inferred_conclusion: `Rejected: early-ply quiet-check ordering reduced score_rate to 0.4520 versus the approved 0.4795 baseline, with zero crash/illegal/timeout/harness failures and acceptable max_plies_rate=0.0370. Average nodes fell sharply to 6490.85, so the make/unmake MoveGivesCheck cost during ordering hurt throughput and likely disrupted the tuned move-order balance more than it helped. The latest blunder still shows a queen-check mating net: after the candidate's optimistic Qxa5 at ply 57, Stockfish replied Qxh3+ and mate followed with Qh1#. Future V4 work should avoid make/unmake quiet-check scoring in move ordering; this failure points to a deeper king-safety/tactical horizon problem around opponent checking resources, not a problem solved by simply pushing own quiet checks earlier.`