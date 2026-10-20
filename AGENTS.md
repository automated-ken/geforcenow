# AGENTS.md

This repository contains the snap packaging (and colocated YARF UI tests) for
the **geforcenow** snap, published by kenvandine.

## Automated maintenance

This repository is maintained in part by the `automated-ken` fleet-maintenance
system (https://github.com/kenvandine/automated-ken). Automated agents may:

- Open pull requests bumping the packaged application/runtime version
- Queue YARF UI test runs on a registered remote runner (real hardware polling the
  automated-ken dashboard for jobs) against candidate/edge builds before promoting a
  release
- Review and comment on PRs, including AI-assisted screenshot review of UI test
  results

## Tests

YARF UI test suites belong under `tests/suite/` in this repository. They are
executed by a registered remote runner (physical/real hardware enrolled with the
automated-ken dashboard), which polls the dashboard for queued jobs, downloads/
installs the target snap build, runs the YARF suite locally, and uploads
screenshots/results directly back to the dashboard. No GitHub Actions workflow is
involved in running tests.

## Conventions

- Do not remove the `tests/suite/` directory; it is required for automated release
  validation. There is no test-running GitHub Actions workflow in this repo by
  design — tests run on a registered remote runner.
- Redundant upstream-polling / sync-release workflows that duplicate automated-ken's
  own version-bump automation should be removed to avoid conflicting/duplicate PRs.

## Pinned dependencies

- `snap/snapcraft.yaml` `sdl2` part: keep `source-tag` on the SDL **2.x** line
  (`release-2.*`). Never bump to `release-3.x` (SDL3): NVIDIA's client links
  `libSDL2-2.0.so.0` and the build swaps in our SDL2 build, which SDL3 can't
  replace. Automated version-bump agents must skip major-version changes here.
