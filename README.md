# Horizon: technical findings on source provenance

This repository publishes a technical findings report on the [Horizon](https://github.com/horizon-rust/horizon) project, a Rust reimplementation of *Minecraft: Java Edition 26.3*.

> **This is not legal advice.** It is a factual foundation for counsel: what was observed, how it was verified, and which legal theories the facts may support. Nothing here has been tested in court. Verify everything with a lawyer.

## What the report covers

- **The shader crate was written from Iris's source.** The project's own removal commit says so, and its public "How Horizon is built" page repeats it. Iris is LGPL-3.0; Horizon shipped the crate under MIT.
- **The crate is still distributed.** It is gone from `HEAD` but reachable from `main` and from 166 pre-removal tags (`v2026.10.16.02` through `v0.2.46`). A normal `git clone` still transfers it.
- **OpenJDK/HotSpot GPLv2 code is in an MIT crate.** `crates/horizon-core/src/jmath.rs` says it ports `java.lang.FdLibm` and uses "HotSpot's tables, verbatim"; `trig.rs` ports fdlibm's `asin`/`atan`/`atan2`.
- **The Minecraft Rust is a translation, not a clean-room reimplementation.** 6,490 `/// Class.method` citations, a 14,668-row port map, committed data tables "copied from vanilla's sources", and six algorithm case studies (Perlin wrap/fudge constants and `NormalNoise` statistics, `Mth.atan2`, boat constants, redstone branch order, MushroomCow draw order, A* tie-breaking).
- **The "clean room" rewrite is unverified.** The spec (`cleanroom/shader-packs-spec.md`, revision 1) was never committed or shared; the same agent family wrote the old and new code; the old code and the decompiled sources are in the same repository; no isolated session or transcript is documented.

## Documents

- [`HORIZON_IP_ANALYSIS.md`](public/HORIZON_IP_ANALYSIS.md) — the full report: repository-level evidence, the six Minecraft algorithm studies, the OpenJDK/GPL finding, the Iris shader admission, the cleanup timeline, the "clean room" analysis, and recommended discovery.
- [`EVIDENCE_PROGRESS.md`](public/EVIDENCE_PROGRESS.md) — the evidence log: what is proven, what is a lead, what was checked and rejected, and what remains open.

## Method

Official Minecraft Java Edition 26.3 client and server JARs (SHA-1 verified against the project's own `research-lock.json`), decompiled with Vineflower 1.12.0; the full Horizon git history; Iris source (LGPL-3.0); OptiFine 26.2 preview (decompiled locally); YUNG's API and Lithostitched source for the secondary leads.

## What is not included

No decompiled Minecraft, OptiFine, Iris, YUNG, or Lithostitched source; no third-party jars; no Horizon clone; no pre-removal shader crate. The report quotes short excerpts for criticism and analysis only.

## Quotations and fair use

Short quotations from third-party code are used for commentary, criticism and analysis. They remain the property of their respective owners. This repository does not redistribute source files, jars or decompiled trees. If you are a rights holder and believe a quotation is excessive, open an issue and it will be reviewed.

## Caveats

- The Iris copying is admitted by the project; the OpenJDK port is stated in the code; the Minecraft translation is supported by the citations and algorithm comparisons.
- The OptiFine and YUNG/Lithostitched/CTOV claims are **leads, not findings**. The report says so.
- The legal conclusions are theories for counsel, not opinions. Verify with a lawyer.
- This is a point-in-time analysis of commit `68a348299e7e1f3f231397fa459936fc50084f26` (2026-11-02). Corrections are welcome as issues.

## Licence of this report

The original text of this report is © the author. Quotations remain under their owners' terms. No licence is granted for third-party material.
