# TEST branch — not the production record

Branch `test/imax-on-7.61`. **Experimental.** The published personal build lives on
[`atmos-activator-all-beds`](https://github.com/goodman-b/AetherEngine/tree/atmos-activator-all-beds);
nothing on this branch is installed by default, and no `/berris` document points at it as truth.

## What it is

Base: [`brandomoore/AetherEngine@plozz-7.61.0-integration`](https://github.com/brandomoore/AetherEngine/tree/plozz-7.61.0-integration)
(`56812084`, upstream engine 7.61.0 merged into Plozz's integration line). Three commits on top:

| Commit | What |
|---|---|
| `394a2b7e` | cherry-pick of upstream `5ef8b40f` — *"feat(audio): DTS:X IMAX tracks render their heights (#744)"*, first shipped in **7.62.0**. `CHANGELOG.md` resolved **"ours"**: this revision is not release 7.62.0 |
| `bc16c098` | `FFmpegBuild` `.upToNextMinor(from: "3.13.0")` → `"3.14.0"`. The pick reads the decoder's new read-only `x_presentation` and needs `x_heights` to decode the alternate presentations' four fixed heights — both exist only from 3.14.0 |
| `5f659081` | `.default(enabledTraits: ["TrueHDObjectAudio", "DTSXObjectAudio"])` — re-applied from the production branch's `c83a67ff`, because the app reaches this engine as a **path** dependency, which cannot carry `traits:` |

## Why it exists

To test two claims on real hardware, in one build:

1. **The fork shrinks when upstream moves.** This branch carries **none** of the production branch's
   activator cherry-pick (`a31fffd6`) — `plozz-7.61.0-integration` already contains that fix, so the pick
   is gone rather than maintained.
2. **DTS:X IMAX heights become reachable without waiting for Plozz's pin** — one upstream commit plus one
   dependency-minor bump. Motivation is future-proofing: a scan of the whole library on 2026-11-11
   (1 141 files, ffprobe 9.0.2) found **zero** DTS:X IMAX presentations, so nothing here fixes a
   present-day defect.

## What is NOT proven

No build, no install, no listening check at the time of writing. The fold gains the pick introduces
(D0 unity, D1/D3 −3 dB, D4 refused) are upstream's measurements on their own hardware, not ours — and our
sink is a Samsung HW-Q990B driven at 7.1.4. A successful compile means nothing about heights; the soundbar
badge proves nothing either, because an intact DTS:X bed reports as Dolby Atmos either way.

The app side needs **no change** to build against this branch, as long as the checkout layout stays
`<sibling>/plozz-engine/AetherEngine` next to the app (the dependency is a relative path).
