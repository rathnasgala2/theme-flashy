# Changelog

All notable changes to `@rathnasgala2/theme-flashy` are documented here.

## Unreleased

## 2.1.0 - 2026-09-26

### Fixed (THD-M6, 2026-09-26 fourth pass)

- `theme.json`'s `"package"` field named `@rathnasgala2/theme-flashy@2.0.0` even after
  `package.json`'s `version` was bumped to `2.1.0` — a consumer resolving
  this theme's identity from `theme.json` (rather than `package.json`)
  would throw `THEME_CONTRACT_IDENTITY_MISMATCH`. Fixed to `@2.1.0`;
  digest chain regenerated in this same commit (no `LOCAL_RUNNERS` script
  changed, so only `integrity`/`evidenceDigest` shift, not
  `fixtureDigest`). `@rathnasgala2/theme-tooling`'s
  `package-identity:check` (new `verify` step) catches this class from now
  on.
- `sbom.cdx.json` is no longer committed in this repository. It used to be
  `cyclonedx-npm` scanning `@rathnasgala2/theme-tooling`'s own
  `package-lock.json` and attributing the result to this theme's identity
  — a design that diverged between a local machine and CI three times
  running (see `theme-tooling`'s own CHANGELOG 0.2.0 for the root cause).
  `sbom:generate` now builds a self-contained CycloneDX document directly
  from this package's own `name`/`version` (zero dependency components —
  this theme ships none); `.github/workflows/release.yaml` generates it
  fresh at release time and uploads it as a build artifact instead of
  committing it. `@rathnasgala2/theme-tooling` sibling checkout re-pinned
  accordingly.

2026-09-25 code-discipline review remediation (THD-H5): this file
previously carried three dated sub-headings under `## Unreleased` above a
`## 2.0.0 - 2026-09-22` heading, even though `2.0.0` has been on the
registry since 2026-09-22 — a state that made no sense read either way
(see the review's own explanation). The three dated entries below are
folded into this one `Unreleased` section (Keep a Changelog: exactly one
`Unreleased` section, dated headings below it), and `2.0.0`'s heading now
carries its actual release date. Everything in this section ships as the
next version; bumping `package.json`'s `version` for that release is an
owner decision (recommended: `2.1.0`, since nothing below is a breaking
change to the token/CSS-hook contract).

### Changed (Second contract 2.1.0 conformance pass, 2026-09-25)

- **THM-M3**: dropped `landmark-main-content` from `theme.json.slotHooks`
  — the CSS only ever targets `main` (`landmark-main`); the shared
  tooling's new set-equality check between `slotHooks` and actual CSS
  usage caught the drift.
- **THF-M2**: contract 2.1.0 adds three externalized contrast pairs
  (`surface-raised`/`surface` >= 1.3 in both palettes, `accent`/`text` as
  a non-text UI adjacency >= 3 in the dark palette). Moved
  `color-surface-raised` to a more distinct purple in each palette and
  the dark `color-accent` (and the `color-focus`/`color-link` tokens that
  mirror it) from `#ff7ae0` to `#f52ec2`; every existing pair
  (`color-on-accent`/`color-accent`, `color-accent`/`color-canvas`,
  `color-accent`/`color-surface`, and the unlisted `color-accent`/
  `color-code-canvas` adjacency the `pre` rule renders) stays
  comfortably above its floor.
- **THF-H1**: added `a:hover` (accent color, thicker underline) and
  `a:visited` (the `color-link-visited` token, defined since the first
  pass but never used by a selector). Gave the `article-end` block's
  spark-mark SVG an explicit `background-size`/`background-position`/
  `background-repeat` so it renders as a placed mark instead of tiling
  as a repeating texture.
- **THD-L1**: removed this theme's own root-only
  `prefers-reduced-motion` guard from `components.css` — the template's
  `gala-base` layer already applies the same `!important` freeze to
  every element, including `::before`/`::after`, so the theme's copy was
  redundant.
- **THD-L2**: `print.css`'s `a` rule no longer repeats
  `text-decoration-line: underline`, which the screen `a` rule in
  `components.css` already sets and print doesn't clear — only the
  print-specific color override remains.
- **THD-M10**: re-pinned all three workflows' sibling checkouts to
  `@rathnasgala2/theme-tooling` `8fd9b36` and `@rathnasgala2/template`
  `d2b2f0f`, and added a `visual` CI job running the shared Playwright +
  axe-core harness (`visual:check`), matching the pattern the other
  reference themes already carry.
- **THF-M3**: `text-decoration-skip-ink: auto` added to the flashy
  theme's link rule, now that the shared tooling's closed CSS property
  catalog admits it (`auto`/`none`/`all`).
- Re-pinned all three workflows' sibling checkouts again to
  `@rathnasgala2/theme-tooling` `ae2ee49`, which serves the `visual:check`
  fixture over loopback HTTP (the theme CSS now actually loads during the
  check), admits `text-decoration-skip-ink`, and adds `color-accent` on
  `color-code-canvas` to its own contrast pair catalog — the `pre` border
  adjacency this theme renders is now gated automatically instead of only
  by manual verification. `sbom.cdx.json` regenerated against the moved
  `theme-tooling` dependency tree.
- `theme.json`'s digest cycle regenerated against the pins above.

### Changed (Contract 2.1.0 adoption, 2026-09-25)

- `theme.json`: `contractVersion` moved to `2.1.0`, `stylingContractDigest`
  copied from `@rathnasgala2/template`'s own published `catalogDigest` for
  that contract revision. `cssLayers`/`stylesheets` are unchanged (this
  theme's three-file shape has no `gala-base` layer of its own — that
  layer is template-owned).
- Removed the `components.css` rules the template's new `gala-base` layer
  now provides: `img`'s duplicate `max-width: 100%` (`gala-base` also adds
  `height: auto`), and the header/main/footer fixed `padding` declarations
  (`gala-base` now supplies a responsive step). Removed the fifth inert
  `outline-color` declaration, under `forced-colors`, on `#main-content`
  (THD-H1: the four others were already removed; this one was missed).
- Aligned `color-focus` with `color-accent` in the dark palette (light
  already matched): `gala-base` is the layer that now paints
  `outline-style` for the focus ring, so this theme's only remaining job
  is handing it an on-brand color via the token.

### Changed (THF-H1, THF-M1, THF-M2, THF-M3, THD-H8, THF-L1, THF-L2, 2026-09-25)

- **THF-H1**: built the theme's actual visual thesis instead of four
  sub-pixel refinements — an accent `border-top` band on the header, an
  accent `border-bottom` rule under `h1`/`h2`, accent `li::marker`s, and
  an accent-filled `article-end` block using `color-on-accent` as real
  foreground text for the first time.
- **THF-M1**: reverted `--gala-border-width` to a 1px hairline in both
  palettes (it was 2px, doubling every structural border/rule/`hr`/
  `select` the base rules were metricked for at 1px) and doubled it
  per-rule, via `calc(var(--gala-border-width) * 2)`, on every
  accent-adjacent element instead (`blockquote`, `pre`, the new header
  and heading accent rules).
- **THF-M2**: the shared `@rathnasgala2/theme-tooling` contrast gate this
  repository depends on now checks `color-accent on color-canvas` (3:1,
  both palettes) — the accent-as-border-color pairing this theme's own
  CSS renders. `color-accent on color-code-canvas` (the `pre` rule) is
  now gated by that shared script too (added in `theme-tooling` `ae2ee49`)
  and clears 3:1 in both palettes.
- **THF-M3**: toned down the underline treatment (`text-decoration-
thickness`/`text-underline-offset` from 0.15em/0.2em to 0.1em/0.15em)
  now that the accent appears throughout the page. `text-decoration-
skip-ink: auto` was added separately once the shared tooling's closed
  CSS property grammar admitted it (see the 2026-09-25 entry above).
- **THD-H8**: shipped this theme's first decorative asset —
  `mark-spark.svg`, a 16x16 sanitizer-clean passive SVG mark declared in
  `theme.json.assets`/`package.json.files`, tiled as a `background-image`
  over the accent-filled `article-end` block.
- **THF-L1**: folded the `header-actions` slot's `gap` into its one base
  declaration (`space-3`), removing a duplicate override that existed only
  because the base rule had been copied rather than parameterized.
- **THF-L2**: dropped the internal `S2-T14` task-packet reference from the
  character-block comment in `components.css`.

### Changed (Pins and generated evidence, 2026-09-25)

- CI (`ci.yml`, `nightly.yml`, `release.yaml`): sibling checkouts re-pinned
  to `@rathnasgala2/theme-tooling` `68dceb3` and `@rathnasgala2/template`
  `e66d877` (contract 2.1.0, unreleased).
- `theme.json`'s digest cycle and `sbom.cdx.json` regenerated against the
  above pins.

### Changed (THD-H1, 2026-09-25)

- Removed the four inert `outline-color`/`outline-width` declaration pairs
  from `components.css` (`#main-content`, `a`, `select`) — `outline-style`
  was never set alongside them, so they painted nothing (the initial value
  of `outline-style` is `none`), and the previous README/CHANGELOG claim
  that they produced a themed visible focus ring was false. A themed ring
  needs `:focus-visible`, unavailable in this template contract version
  (TPL-H2); a single comment in `components.css` documents where it will
  be restored.

### Changed (THD-C1, 2026-09-25)

- Added a bare-root `[data-gala-publication-root]` block (light palette)
  plus a `@media (prefers-color-scheme: dark)` override to `tokens.css`,
  before the two resolved-mode blocks, so every `--gala-*` token still has
  a real value when `data-gala-resolved-color-mode` is not set (no
  JavaScript, a text-mode crawler, or a pre-hydration paint) — previously
  the theme applied no styling at all in that case.

### Changed (THD-M6, 2026-09-25)

- Replaced this repository's own copy of `tooling/scripts`/`tooling/test`
  (25 files, identical across all five theme repositories except one
  package-name literal, and the `tooling-drift.test.mjs` guard that
  skipped in every CI configuration these repositories had, THD-H3) with a
  dependency on the new `@rathnasgala2/theme-tooling` repository, which
  now implements every gate once. `tooling/` here carries only
  `run.mjs` and a trimmed `package.json`. See
  `../theme-tooling/CHANGELOG.md` for what moved and what changed in the
  process (THD-H2/M2/M3/M4/M5).
- Deleted the root `package-lock.json` (THD-L5): the published
  `package.json` has never had a dependency for it to lock.
- Widened the closed `package.json` shape to carry `repository` (THD-H6):
  `npm publish --provenance` derives the source repository from that
  field and refuses to build a provenance statement without it.

### Changed (SCHEMA-REPIN-2.11.0, 2026-09-22)

- `tooling/package.json` re-pins `@rathnasgala2/schemas` from the LOCAL-1
  local tarball (`file:../../../local-packages/rathnasgala2-schemas-2.8.0.tgz`)
  to the exact published registry version `2.11.0`
  (`https://registry.npmjs.org/@rathnasgala2/schemas/-/schemas-2.11.0.tgz`,
  integrity `sha512-5hXxpLaXqEKKhoRLBBEq98rzJQ9K4u3b8nDMt/ZZmV2gL4XRmTOCwjF1U618UJ1CgmFnlifhQWRuDvQQVyFfTA==`).
  This fixes CI, which was failing on every push because the `file:` path
  does not exist on GitHub Actions runners. `urn:gala:schema:theme-contract:2.0.0`
  and the `build-input` root this tooling validates against are
  byte-identical between 2.8.0 and 2.11.0 (contract re-pin packet,
  2026-09-19); `tooling/package-lock.json` and `sbom.cdx.json` regenerated
  accordingly; full `npm run verify` re-run and green.
- Added `tooling/scripts/check-no-local-schema-pin.mjs` (wired into
  `verify` as `schema-pin:check`) so a `file:`/`local-packages` specifier
  for `@rathnasgala2/schemas` can never silently return.

### Changed (THEMES-2.8.0, 2026-09-18)

- `tooling/package.json` pins `@rathnasgala2/schemas` to the packed
  `rathnasgala2-schemas-2.8.0.tgz` tarball (sha256
  `6352293855cdcff9054d43ced876740644f6b45bc813eda3990b646ec9bef563`,
  LOCAL-1), up from 2.6.1; `tooling/package-lock.json` integrity and
  `sbom.cdx.json` regenerated. `urn:gala:schema:theme-contract:2.0.0`, the
  `build-input` root and the published `examples/valid/build-input/canonical.json`
  this tooling validates against are byte-identical between 2.6.1 and 2.8.0;
  the 2.7.0-2.8.0 delta is confined to the deployment roots, the OpenAPI
  bundle and the App catalogs, none of which this repository consumes.

### Added (FOLLOW-UP SUPPLY-CHAIN-JS, 2026-09-17)

- `tooling/scripts/resolve-template-dir.mjs` and `tooling/test/tooling-drift.test.mjs`
  updated to `theme-default`'s canonical copies: `resolveTemplateDir()` now
  also honours `WORKSPACE_ROOT` (DEC-015 name), checked after the existing
  `GALA_TEMPLATE_DIR` override and before the fixed relative default
  (`<WORKSPACE_ROOT>/template`), fixing resolution from a git worktree one
  level deeper than the real checkout (LOCAL-38); `tooling-drift.test.mjs`
  resolves its `theme-default` canonical source the same way and reads
  `@rathnasgala2/theme-flashy`'s own package-identity literal from `package.json` at run time
  instead of a hardcoded name, and skips (with a printed reason) rather than
  failing when `theme-default` cannot be found. Added
  `tooling/test/resolve-template-dir.test.mjs`.

## [2.0.0] - 2026-09-22

### Added

- Visual character: High-saturation magenta accent with bold weights, a thicker default `border-width`/`focus-width`, generously rounded `radius-*`, an accent-colored blockquote/code-block rule and a thicker underline on links.
- Initial closed package file set: `package.json` (dependency-free,
  script-free, 4-key closed shape), `theme.json` (all 35 tokens for light
  and dark palettes, `stylesheets`/`cssLayers` three-file shape, the
  51-hook `slotHooks` subset this theme's CSS uses, budgets, and the
  digest chain), `tokens.css`/`components.css`/`print.css`, and a
  compact-JCS `LICENSE` license-evidence file (SPDX `Apache-2.0`).
- WCAG 2.2 AA contrast for every named token pair in both palettes,
  `outline-color`/`outline-width` on every interactive hook (no
  `outline-style` override, no `:focus` pseudo-class available in this
  template contract version — corrected 2026-09-25, THD-H1: these two
  longhands alone never painted a visible ring, and were removed),
  `forced-colors: active` system-color mappings, and a defensive
  `prefers-reduced-motion: reduce` rule.
- `tooling/` local dev/test/SBOM project (private, unpublished, its own
  lockfile) with the closed-hook CSS conformance test, the WCAG contrast
  test, the theme-contract schema test, the closed-package-file-set test,
  the forbidden-constructs absence test, the digest-cycle generator/test,
  the template-conformance byte-equality test (two builds against
  `@rathnasgala2/template`'s `main` branch, consumed by path), and a
  tooling-drift test asserting `tooling/scripts/*.mjs` is byte-identical
  to `theme-default`'s canonical copy except the one documented
  package-identity literal.
- SBOM (`sbom.cdx.json`, CycloneDX 1.6, generated for the published package
  surface — which has zero runtime dependencies).

### Notes

- `fixtureDigest`/`evidenceDigest` are this repository's own genuine local
  conformance evidence (five of the eight DEC-097 runner IDs:
  `schema`/`semantic`/`package`/`css`/`absence`), not the DEC-097-mandated
  _shared_ fixture release/result the not-yet-existing `S2-T11` reusable
  CI workflow will eventually produce and re-issue across all five theme
  packages. See README "What `fixtureDigest`/`evidenceDigest` are, and are
  not."
