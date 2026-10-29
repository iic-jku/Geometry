# Project conventions

## Repositories

- This is `iic-jku/Geometry` (remote `jku`), a fork of `ediloren/Geometry` (remote
  `upstream`), made to fix the triangulation behind `ediloren/FasterCap#8`. Remotes use SSH
  URLs (`git@github.com:…`).
- `iic-jku/FasterCap` builds it as the sibling `../Geometry` (`add_subdirectory`); its CI
  checks out this fork's `master`. The sources include FasterCap's `stdafx.h`, so it builds
  only as part of FasterCap.
- The repository is standalone, not in ediloren's fork network: new PRs default to
  `iic-jku/Geometry` `master`.

## Branches

- Every change to code or CMake files goes issue → branch → PR → `master`: one branch per
  issue, based on `master`, merged with a merge commit once its CI is green. Its test goes
  into `FasterCap` through a `FasterCap` PR, after the fix is in `master` here, as
  `FasterCap`'s CI builds this `master`.
- CI, `AGENTS.md`, `ReadMe.txt` and other docs, `.gitignore`: changes that don't touch the
  code are committed directly on `master`, without issue or PR.
- A branch is deleted once its PR is merged, on GitHub and locally.

## Scope

- One concern per branch; report unrelated findings instead of fixing them.
- Say in the commit body when behavior changes: results, signatures, meshes.
- Fix root causes in the shared code, not with special cases.

## Code

- Keep each file's line endings (CRLF), indentation (tabs) and naming (`C2DVector`,
  `C2DTriangulate`, `m_…`).
- `ASSERT` is `wxASSERT` (`GeoGlobal.h`): wxWidgets 3 evaluates it in release builds, and in
  console runs it only prints. It is no error handling: handle the case, or return an error
  the caller checks.
- Don't duplicate code: extend the shared code rather than adding a variant.

## Tests

- There are no tests here: test through FasterCap. Each bug fix gets a test that fails
  without it, as a geometry in FasterCap's `tests/geometries.py`, e.g. the concentric spheres
  of `ediloren/FasterCap#8` run with `-a`.
- Build FasterCap from `../work/FasterCap`, whose `Geometry` symlink points here (see
  FasterCap's AGENTS.md), and run `ctest --test-dir ../work/build --output-on-failure` before
  calling work done; report failures with their output.
- No loosened tolerances or re-baselined numbers without a physical explanation.

## Git

- Martin reviews in detail and commits himself: leave changes uncommitted in the working
  copy. For a change that should become several commits, stage it and propose the split.
  Proposed commit messages are welcome as a starting point. Don't commit or push unless
  asked; never commit scratch files.
- Martin keeps WIP stashes: never chain `git stash push`/`pop`. Test staged changes via
  `git checkout-index -a --prefix=<dir>/` or a temporary worktree.
- Commit subject: `Area: verb phrase`, 6–12 words, no articles. Area is a file, class or
  function, e.g. `Triangulate.cpp` or `C2DTriangulate::InsertConstrEdge()`. Usually no
  body; one or two lines for a non-obvious why or a behavior change.
- Commit and PR texts contain only what the reader can't get from the issue, diff or CI.
  PR: issue reference plus 1–4 sentences, no Summary/Changes/Testing headings, and a
  proposed title in the style of the commit subjects.
- Every PR references its issue: `Fixes #N` or `Part of #N`; without an issue, file one
  first. Issues in other repos are written `ediloren/Geometry#N`, `iic-jku/FasterCap#N`.
- Martin pushes and opens the PRs: for each finished branch, say whether it is pushed and
  propose the PR title and text.
- Issues show the smallest reproducing input and end with the versions used: this commit,
  the FasterCap commit and options, OS and CPU architecture, compiler.
- Prefer bullet lists to long sentences chained with semicolons; no period at the end of a
  bullet. Mark identifiers as code with backticks, every mention.
- No AI attribution: no `Co-Authored-By` trailer, no "Generated with …" line.
- Before handing over, re-read your diff against `master` and check it against this file.
