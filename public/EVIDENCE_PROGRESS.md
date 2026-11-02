# Horizon evidence — progress log

Last updated: 2 November 2026
Repository: `horizon-rust/horizon` @ `68a348299e7e1f3f231397fa459936fc50084f26`
Memo: `HORIZON_IP_ANALYSIS.md`

## Status

| # | Claim | Evidence | Confidence | Where |
|---|---|---|---|---|
| 1 | Rust is a translation of decompiled Minecraft 26.3 | 6,490 `Class.method` citations; 14,668-row port map; "Translate the vanilla method" skill; official jars + Vineflower pinned; 7,301/5,037 decompiled files reproduced | High | Memo §3, §4 |
| 2 | Six Minecraft algorithms reproduce arbitrary implementation choices | Perlin shuffle/wrap/fudge + `NormalNoise` constants (the 16-gradient table is Perlin's reference, excluded); `Mth.atan2` table/magic constant; boat constants; redstone branch order; MushroomCow draw order; A* 1.5 fudge + heap tie-breaking | High | Memo §4.1–4.6 |
| 3 | Committed data tables copied from vanilla source | `flat_tables.rs` "copied from vanilla's sources"; `block_state_data.txt` "generated from vanilla 26.3 BlockStateData.java" | High | Memo §5.1–5.2 |
| 4 | OpenJDK/HotSpot GPLv2 code shipped under MIT | `jmath.rs` "java.lang.FdLibm ports", "HotSpot's tables, verbatim"; `trig.rs` fdlibm ports; no notice; `Cargo.toml` MIT | High | Memo §4.7, §6.0 |
| 5 | Shader crate copied from Iris (LGPL-3.0) | Removal commit `4e7c25bb5` admits it; public docs page repeats it; 170 Iris refs; verbatim `CommentDirectiveParser` tests; `comment_directive` vs `findDirective` step-for-step; `resolve` vs `normalizeAbsolutePath` | **Very high (admission + code)** | Memo §4.8 |
| 6 | Shader crate shipped in tagged builds | 285 tags contain the introduction commit; **166 tags before removal** contain the crate (`v2026.10.16.02`–`v0.2.46`); `v0.2.46` verified | High | Memo §4.8(e) |
| 7 | Shader code still distributed after removal | `b22583d05` ancestor of HEAD; blob in `main`; no purge; normal clone transfers it | High | Memo §4.8(e), §7.12 |
| 8 | Provenance scrubbing after notice | `e80ee4d10` comment scrub; `7efdbdb39` docs reversal; `4fe648b67` website → private repo; `234d5e1cd` post-hoc policy; four removals in 85 minutes on 24 Oct | Medium-high (pattern) | Memo §3.10 |
| 9 | OptiFine code copied for graphics efficiency | Sampled OptiFine 26.2 modules; implementations differ | **Not supported** in sampled modules | Memo §5.5 |
| 10 | YUNG/Lithostitched/CTOV code copied | YUNG's API is LGPL-3.0; Lithostitched has no license. Old Rust modules are data converters/reimplementations, structurally different from the Java. Not shown. | **Not proven** | Memo §3.10 |
| 11 | New "clean-room" shader crate reuses old/Iris code | Structural comparison: 40 shared fn names of 204 old/346 new, only 20 non-generic, **0 shared comments**; 80 shared strings are format identifiers. Spec missing. | **Structurally different; unverified** | Memo §4.9 |
| 12 | The "clean room" claim is not credible on the record | Old crate in `.git`; `AGENTS.md` documents `.local` decompiled sources; same agent family (Claude Opus 5.5); no session ID/prompt/transcript; spec never committed or shared; old crate has verbatim Iris tests and function translations; new crate's differences are equally consistent with a second AI pass | Analysis (strong) | Memo §6.8 |
| 13 | No court case does not make it legal | Statutory elements met by admissions; existing doctrine (derivative work, § 101, GPL § 0/2(b)) applies; absence reflects enforcement choices | Analysis | Memo §6.8(d) |

## Artifacts

- `HORIZON_IP_ANALYSIS.md` — the memo.
- `analysis/sources/client`, `analysis/sources/server` — decompiled Minecraft 26.3 (Vineflower 1.12.0).
- `analysis/optifine/of-src` — decompiled OptiFine 26.2 preview (`srg/net/optifine`).
- `analysis/horizon-shaderpack-pre-removal/` — the deleted shader crate at `e423e570c`.
- `iris-repo/` — Iris source (LGPL-3.0).
- `yungs-api-repo/` — YUNG's API source (LGPL-3.0).
- `lithostitched-repo/` — Lithostitched source (no license file).
- `optifine-repo/` — official OptiFine repo (docs only).
- `horizon-repo/` — full-history clone.

## Open work

1. **Release binaries.** Download a game release in the `v2026.10.16.02`–`v0.2.46` range and search the binary for shader-crate strings (`DRAWBUFFERS`, `Iris`, `CommentDirectiveParser`). The source shipped in 166 tags; prove the binaries did too.
2. **The clean-room process.** Demand session IDs, system prompts, model versions, full transcripts, the spec and every revision, the identity of who wrote the spec and what they read, and whether the implementation session had repository/`.local` access.
3. **Forks.** Use the GitHub API to enumerate forks whose default branch still contains `b22583d05`.
4. **Other third-party code.** FSR/AMD (credited), ETF/EMF, Continuity/Fusion, CIT Resewn — check notices and provenance.
5. **More Minecraft algorithms.** `NoiseRouterData`, `Entity.move`, `LivingEntity.travel`, `FeatureSorter`, `RandomSpreadStructurePlacement`, `JigsawPlacement`.
6. **Launcher shader feature.** The launcher's shader pack browsing/installing was removed with the crate; check release artifacts for it.

## Corrections

- **2026-11-02 — Perlin gradient table.** §4.1(a) originally treated the 16-entry gradient table, including the four repeated directions, as a Minecraft-specific fingerprint. That was wrong: Ken Perlin's published improved-noise reference and GPU Gems chapter 5 document the same 16-slot table and the four repeats (the duplicates let a 4-bit hash index 16 slots). The table is excluded from the evidence; §4.1 now rests on the constructor's draw order, the `wrap` boundary, the `1.0E-7F` fudge and the `NormalNoise` constants. (Flagged by external review.)
