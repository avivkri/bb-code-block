# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Canonical documentation

This is one of seven Budibase plugin forks that share an identical build, release, and upgrade setup. **The full documentation lives in `../minikube-ground/dev-lab-setup/docs/budibase-plugins.md`** (the `minikube-ground` repo, cloned as a sibling of this one) — read it before changing the build, the release workflow, or the `svelte` version. It covers the rollup pipeline, the release mechanics, the codemod's known gaps, and the mandatory rebuild procedure after a Budibase upgrade.

Only the facts specific to this repo are below.

## This plugin

A syntax-highlighted code block component. Fork of `rosnerdev/bb-code-block` (unmaintained since 2022).

- Plugin name / component key: `bb-code-block` → `plugin/bb-code-block`, doc id `plg_bb-code-block`
- Current version: 1.1.1 · branch `master`
- Settings: `code` (text), `theme` (select: Dark/Light)
- Single component, `src/Component.svelte`; no children

## Repo-specific notes

- **Bundles third-party Svelte source.** `svelte-highlight@6.2.1` and `highlight.js` are real runtime dependencies; `svelte-highlight` ships `.svelte` files that get compiled by *this* repo's Svelte compiler. That makes this plugin the most sensitive of the seven to Svelte version skew — it is the one that surfaced the `u is not a function` failure when built against a Svelte newer than the host's. Keep the pin exact.
- **`@budibase/backend-core` was added by hand.** The Svelte 5 codemod only bumps that devDependency when it already exists, but the rollup config it writes imports `validate()` from it. Without it the build dies with `Cannot find package '@budibase/backend-core'`.
- The highlight theme is injected through `<svelte:head>{@html …}` from `svelte-highlight/styles/atom-one-*`. If the code renders unstyled, check that head injection rather than the component.
- `HighlightAuto` guesses the language and mislabels short snippets (a two-line JS sample can come out tagged `abnf`). That is upstream highlight.js auto-detection, not a migration artefact.
