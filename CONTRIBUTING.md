# Contributing to lightning-onion

## Building and testing

```sh
go build ./...
go vet ./...
go test ./...
```

These are the same commands CI runs, so run them before opening a pull request.

## Pull requests

- Keep each change focused; split unrelated work into separate pull requests.
- Add a `CHANGELOG.md` entry for anything that changes behaviour, under the
  topmost `## [X.Y.Z]` heading in the matching `### Added` / `### Changed` /
  `### Fixed` subsection. If the last release just shipped and no heading is
  open yet, add one with the version the change warrants.
- Once your pull request has a number, append `(#N)` to the changelog bullets it
  introduces. The release notes are generated from that text, so a bullet
  without its reference loses the link back to the discussion.

## Versioning

There is no `VERSION` file. `CHANGELOG.md` is the only place the version is
recorded; the released version is the git tag the release workflow creates from
it. This module is consumed through the Go module proxy, so there is nothing to
build or inject.

## How releases are cut

Releases are manual. `.github/workflows/release.yml` is `workflow_dispatch`-only
and does the tagging itself:

```sh
gh workflow run release.yml --repo flokiorg/lightning-onion
```

It resolves the version from the topmost `## [X.Y.Z]` heading in `CHANGELOG.md`
(or from the optional `version` input, given as a bare number with no `v`),
re-runs the CI gate, extracts that changelog section as the release notes, then
creates and pushes the annotated `vX.Y.Z` tag. This module is consumed
through the Go module proxy, so the release carries no artifacts.

Do not create the tag by hand — the workflow creates it, and a manual tag would
collide with the one it makes.
