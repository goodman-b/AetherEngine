# Fork notice — personal engine branch

Parent: [`superuser404notfound/AetherEngine`](https://github.com/superuser404notfound/AetherEngine)
by Vincent Herbst, reached here through [`brandomoore/AetherEngine`](https://github.com/brandomoore/AetherEngine).
Branch `atmos-activator-all-beds` = upstream `55430e28` + two commits. Used by the
[Plozz personal build](https://github.com/goodman-b/Plozz/tree/atmos-heights-back).

## The two commits

1. **`d073b5f3` — run `AtmosOutputActivator` for every rendered bed, not only TrueHD-Atmos beds.**
   This is a **cherry-pick of upstream `66352e320615409a49c40398e46d4579ae9fa6b7`**, shipped in
   engine 7.56.0. It is taken as a cherry-pick rather than re-implemented: an earlier hand-written
   port left a dead flag, contradicted the header documentation, and shipped no tests. The
   cherry-pick brings the upstream commit's own test suites (25 tests, 2 suites, passing here).
   `CHANGELOG.md` is resolved **"ours"** during the pick — this revision is not release 7.56.0 and
   must not claim to be.
   Linkage evidence: the symbol the fix removes, `audioBedIsFromDolbyAtmos`, goes from 7 to **0**
   occurrences in the built framework.
2. **`9b731285` — enable the object-audio traits by default on this personal copy**
   (`.default(enabledTraits:)` in the package manifest). Needed because the consumer reaches this
   engine as a SwiftPM **path** dependency, and a path dependency cannot carry `traits:` — so the
   enablement has to live here rather than in the app.

## Licensing

**LGPL-3.0 plus the author's Apple Store / DRM exception**, both preserved unmodified. The
exception is an *additional permission* under GPL §7 granting distribution through app stores and
TestFlight; it is not a restriction and is left exactly as written. Object-audio output engages
Dolby and DTS patent territory, so this branch is a personal build for one household device, not a
distribution of a decoder product.

## No binaries

Nothing built is published — no `.framework`, no `.xcframework`, no `.ipa`, no `.app`. Source only.

## Verifying the change

```
swift test --filter AtmosOutputActivator      # the cherry-picked commit's own suites
```
Beyond unit tests, the real acceptance signal is external: the connected soundbar must report
**Dolby Atmos** rather than plain **Dolby TrueHD**. A green build means nothing on its own here —
the failure mode is silent, correct-looking stereo/surround with no heights.

## Reading this branch

Compare against **`55430e28`** — the revision Plozz pins and the base of this
branch — not against `main` of this fork. Measured against the pin this branch is exactly
**3 commits ahead, 0 behind**; against `main` it looks like ~49 commits because the pinned
revision is not an ancestor of this fork's `main`. No upstream commit is rewritten or hidden.
