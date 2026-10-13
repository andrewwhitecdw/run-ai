# Doc Bug Findings — run-ai monorepo

Local tracker for tiny documentation/factual bugs found by cross-checking prose
against the repo. Records findings, evidence, the smallest fix for each, and
upstream status.

Legend: `[pr]` upstream PR open · `[fork]` fixed on a fork branch, no upstream PR
yet · `[filed]` upstream issue exists · `[pending]` not filed, not fixed.

## Already handled

| Status | Item |
|---|---|
| [pr] | CONTRIBUTING.md:143 — CODEOWNERS listed in CI Layer 1 lint (deleted one word). Filed upstream as [#2457](https://github.com/dsx-ai-factory/run-ai/issues/2457). Maintainer opened #2490; we took up their offer to send ours instead → PR [#2495](https://github.com/dsx-ai-factory/run-ai/pull/2495) (fixes #2457 + #2460, branch `docs/layer1-ci-list`). |
| [pr] | CONTRIBUTING.md:143 — "CodeQL diff scan" claimed on fork PRs; no CodeQL job in Layer 1 CI → [#2460](https://github.com/dsx-ai-factory/run-ai/issues/2460). Covered by PR #2495. |
| [pr] | CONTRIBUTING.md:208 — `.github/CODEOWNERS` and `docs/CODEOWNERS` don't exist; Step 4 omits `validate` target → [#2461](https://github.com/dsx-ai-factory/run-ai/issues/2461). Covered by PR [#2496](https://github.com/dsx-ai-factory/run-ai/pull/2496) (branch `docs/contributing-codeowners-step4`). |
| [pr] | MAINTAINERS.md `/test` → `/ok-to-test` and `backport/v2.X` → `backport-<branch>` → upstream PR [#2497](https://github.com/dsx-ai-factory/run-ai/pull/2497) (branch `docs/maintainers-cmd-and-label`). Opened just before the "fork only" instruction. |

Note: fork main still carries the one-word fix `4bf8c161c`, now superseded by
PR #2495's fuller rewrite — expect a trivial conflict when syncing fork main
after #2495 merges; drop the fork commit then.

## Fixed on fork branches, upstream PRs intentionally not opened yet

User instruction (2026-10-13): keep these on the fork only for now; max 5 open
upstream PRs at a time (3 open as of 2026-10-13: #2495, #2496, #2497). When a
slot frees, open in this order: (1) `docs/cruft-cleanup`, (2)
`docs/service-readme-corrections`, (3) `docs/dev-docs-small-fixes`. Each branch
is cut from upstream `main` and DCO-signed, ready to `gh pr create` on request.

| Branch | Commit | Contents |
|---|---|---|
| `docs/service-readme-corrections` | `b1f74526d` | Nine READMEs: bff dead `pkg/client/README.md` links; datavolumes phantom `make coverage`; metrics-exporter phantom `make clean` + false docker-image claim; anyworkload missing `restartrules`/`nodeaffinity` plugins and phantom `make docker-build` (→ `local-deploy`); Go 1.26→1.27 in notifications-service, agent, cluster-sync; runai-chart dep table (kai `v0.0.3`→`v0.18.4`, karta source `/karta` dropped); runai-mcp-server `make docker` → `make docker-build`. |
| `docs/cruft-cleanup` | `c273f6bc4` | CLAUDE.md: 4 dead index links removed (redoc, git-sync, valkey, provisioner-service), notifications-service added; schema-validation README `GO_VERSION` 1.21→1.24.6; bug-report template `kubectl version --short`→`kubectl version`; fern-docs-ci stale NON-BLOCKING comment deleted; runai-cli review instructions fake `auth-client`/`common/logging` examples repointed to real modules; skills/README missing `runai-diagnose-failed-workload` added; local-dev.md counts 25+25/~50 → 23+19/over 40 and false crd-upgrader "no Go source" claim removed. |
| `docs/dev-docs-small-fixes` | `9d92329f1` | docs/AGENTS.md MCP reference → `docs/agents/mcp-server/deploy.md`; external-dependencies.md karta source `/karta` dropped; services.md nonexistent oauth2-proxy row dropped. |

## Highest-value tiny fixes (one-liners, verified)

| # | Location | Bug | Smallest fix |
|---|---|---|---|
| 1 | `MAINTAINERS.md:26` | References a `/test` command that doesn't exist; real trigger is `/ok-to-test` | s/`` `/test` ``/`` `/ok-to-test` ``/ |
| 2 | `MAINTAINERS.md:27` | "Applying `backport/v2.X` labels" — `backport.yaml:56` matches `backport-*` (dash); a slash label never fires | "backport-`<branch>` labels (e.g. `backport-release-3.0`)" |
| 3 | `.github/workflows/README-schema-validation.md:140` | `GO_VERSION: "1.21"` in env block; workflow (`validate-cluster-config-schema.yml:27`) and the README's own line 22 say `1.24.6` | s/1.21/1.24.6/ |
| 4 | `.github/ISSUE_TEMPLATE/01_bug_report.yaml:33` | Tells users `kubectl version --short`; flag removed in kubectl 1.28, command errors | `kubectl version` |
| 5 | `runai-mcp-server/README.md:112` | `make docker` fails — no such target (`make: *** No rule to make target 'docker'`); shared makefiles provide `docker-build`/`docker-push` | `make docker-build` |
| 6 | `control-plane/notifications-service/README.md:65` | Prose says "Go 1.26"; go.mod says `go 1.27.2` | "Go 1.27" |
| 7 | `runai-cluster/cluster/agent/README.md:57` | Prose says "Go 1.26"; go.mod says `go 1.27.1` | "Go 1.27" |
| 8 | `runai-cluster/cluster/cluster-sync/README.md:53` | Prose says "Go 1.26"; go.mod says `go 1.27.1` | "Go 1.27" |
| 9 | `runai-cluster/cluster/runai-chart/README.md:93` and `docs/developer/external-dependencies.md:40` | Karta chart source cited as `oci://ghcr.io/dsx-ai-factory/workload-map/karta`; `Chart.yaml` declares `oci://ghcr.io/dsx-ai-factory/workload-map` (extra `/karta`) | drop `/karta` (2 files) |
| 10 | `runai-chart/README.md:92` | Dependency table pins `kai-resource-management` at `v0.0.3`; `Chart.yaml` says `v0.18.4` | update table |
| 11 | `skills/README.md:26-33` | Lists 3 skills; a 4th (`runai-diagnose-failed-workload`, with SKILL.md/BENCHMARK.md/evals) exists on disk | add bullet |

## Dead references (small deletions or repoints)

- [pending] `CLAUDE.md:95` — dead link `control-plane/redoc` (OpenAPI pipeline now at `control-plane/infra/openapi`). Remove line or repoint.
- [pending] `CLAUDE.md:107` — dead link `runai-cluster/cluster/git-sync` (does not exist anywhere). Remove line.
- [pending] `CLAUDE.md:118` — dead link `runai-cluster/cluster/valkey` (only a client package at `runai-common-packages/cache/internal/valkey`). Remove line.
- [pending] `CLAUDE.md:169` — dead link `runai-installer/provisioner-service` (`runai-installer/` has only `clients/`, `installer/`). Remove line (optionally add `runai-installer/clients`).
- [pending] `CLAUDE.md:56-96` — service index claims "Missing documentation (0 services)" but omits `control-plane/notifications-service` (has README.md, CLAUDE.md, artifact-metadata.json; in `local-dev.sh` CP_GO_SERVICES). Add index line.
- [pending] `docs/developer/services.md:62` — `oauth2-proxy` row links `runai-cluster/cluster/oauth2-proxy`, which doesn't exist. Delete row or repoint.
- [pending] `docs/AGENTS.md:52` — "Reference" column points to `docs/installation-and-configuration/mcp-server.md` (doesn't exist); real docs at `docs/agents/mcp-server/deploy.md`. Update path.
- [pending] `.github/instructions/runai-cli/review.instructions.md:70,81` — "Correct" go.mod example cites nonexistent `runai-clients/auth-client` (real clients: assets-service, catalog-service, cli-exposer, cluster-api, datavolumes-service, k8s-objects-tracker, policy-service, workloads-manager) and nonexistent `common/logging` (real: `runai-common-packages/logging`). Use real paths in example.
- [pending] `control-plane/bff-service/README.md:36,109` — links `pkg/client/README.md`, which doesn't exist (sibling services audit/metrics/org-unit have one). Write it or drop the links.

## Nonexistent make targets documented as real (copy-paste fails)

- [pending] `control-plane/datavolumes-service/README.md:148` — documents `make coverage`; no such target in the service Makefile or `build/makefile/*.mk` (only bff-service defines one locally). Delete the table row.
- [pending] `runai-cluster/cluster/metrics-exporter/README.md:10,22` — documents `make clean` (no target) and claims `make build` creates a docker image `runai/metrics-exporter` (it only compiles binaries to `bin/`). Remove Clean section and the image claim.
- [pending] `runai-cluster/cluster/anyworkload-controller/README.md:18,57` — documents `make docker-build`; no such target (only `docker-build-generic` via `local-deploy`). Repoint to `make local-deploy`.
- [pending] `runai-cluster/cluster/anyworkload-controller/README.md:5-8,14-16,28` — README omits plugins that `cmd/main.go:25,30` actually imports: `restartrules` (on disk) and dromos `nodeaffinity`. Add to prose and layout tree.

## Bigger clusters (issue-sized, not one-liners)

- [pending] **docs/ anchor slugs** — ~100+ links across ~20 files use wrong hash anchors. Examples:
  - `#ngc-api-key` ×42, `#docker-registry` ×10, `#generic-secret` ×5 → `credentials.md` (targets are `<Accordion>` blocks, not headings; repoint to `#adding-new-credentials` or add explicit anchors).
  - `#retrieve-required-oidc-values` ×14 → heading is "Retrieve Required OIDC **Flags**" (`retrieve-required-oidc-flags`).
  - `cluster-config.md#nvidia-run-ai-services-*` ×12 → colon stripped by slugger: `nvidia-runai-services-*`.
  - RBAC overview slug: 4 files use two different wrong spellings; correct is `#role-based-access-control-rbac-in-nvidia-runai`.
  - `#roles-in-nvidia-run-ai` / `#custom-role-management-api-only` (multiple files) → `roles-in-nvidia-runai` / `custom-roles-api-only`.
  - Legacy Fern hash anchors `#a13adq7eth7w` ×4, `#i3c9jpfzerlq` ×3 in `over-quota.md` → real heading slugs (`step-1-logging-in`, etc.).
  - Singles: `#show-hide-details` → `showhide-details`; `#setting-up-data-and-storage` → `setting-up-data--storage`; `#updating-the-default-priority-mapping` → `updating-the-default-mapping`; `#workload-types-defaults` → `workload-types-and-defaults`; `#environments-created-by-nvidia-run-ai` → `...-runai`; `#placement-strategy-bin-pack-and-spread` → `placement-strategy---bin-pack-and-spread`; `#control-plane-certificate` → `control-plane-certificate-wildcard`; `datasources.md#pvc`/`#secret`/`#configmap` → no such headings.
- [pending] **`local-dev.md`** — service counts wrong: "25 + 25 Go services" is actually 23 (`local-dev.sh:158` CP_GO_SERVICES) + 19 (`local-dev.sh:185` RC_GO_SERVICES), so "~50 Go services" (lines 3/13/40) is 42. Also line 101 falsely claims `runai-crd-upgrader` has "no Go source, just packages kubectl" — it has `cmd/main.go`, `pkg/upgrader/`, a `build-go` Makefile target, and builds via the standard `build_go_service` path.
- [pending] **`build/makefile/README.md`** — extensively stale: references two makefile trees that have never existed in git history (`control-plane/infra/makefile/`, `runai-cluster/common/build/makefile/`); line 59 says `index.mk` includes `mirrord.mk` (doesn't exist; actual includes are `testenv.mk`/`packages.mk`); lines 96/105-106 state GO_VERSION_DEFAULT 1.26.6, MOCKERY_VERSION v2.53.3, GOLANGCI_LINT_VERSION v2.9.0 vs actual `build/makefile/golang.mk` 1.26.8, v3.8.1, v2.14.0.
- [pending] **`.github/workflows/fern-docs-ci.yaml:99-102`** — stale comment claims nav check is "NON-BLOCKING FOR NOW ... currently reports 263 missing. Flip to blocking"; step already has `continue-on-error: false` (line 104) and the check passes clean (`python3 ./tools/check-fern-nav.py fern` → MISSING: 0). Delete the stale comment block.

## Verified clean (checked, no action)

- README.md — all 18 relative links/assets resolve; root Makefile `validate` passes.
- SECURITY.md, SUPPORT.md, CODE_OF_CONDUCT.md — internal references resolve.
- CONTRIBUTING.md — ruff 0.15.15 (`.github/requirements/ruff.txt`), golangci-lint v2.9.0 (`pr-checks.yaml:121`), helm v3.16.2, `release-3.0` branch, Discussions enabled, `good first issue`/`help wanted`/`pipeline/build-failed`/`pipeline/e2e-failed` labels, `/ok-to-test`, dco via `workflow_call`, `local-dev.sh`, karta dependency, plugin examples — all match reality.
- assets-service, catalog-service, audit-service, metrics-service, org-unit-service READMEs — links, targets, and OpenAPI pins verified.
- `fern/VERSIONING.md`, `tools/check-fern-nav.py`, `tools/materialize-fern-versions.sh`, `runai-mcp-server` README otherwise — verified clean.
- Low-confidence (skipped): `backport/release-3.0` and `pipeline/jira-failed` labels don't exist yet, but no in-repo workflow references them — likely created on demand by the private Layer 2 pipeline.

## Caveats

- docs/ anchor findings assume Fern's current GitHub-style slugger (verified on Fern's live site 2026-10-13). Confirm any fix with `fern check` before a PR.
- Repo AGENTS.md: do not proactively edit `docs/` — those rows need explicit user approval.
- Helm charts changed → per repo rules run `helm dependency update` + `helm lint` + `helm template` on `control-plane/control-plane-chart`.


---

## Desloppify scan — 2026-10-13

Command: `desloppify scan --path /home/andrewh/code/run-ai --by-language --no-badge`
(languages scanned independently: go, java, javascript). State lives in
`.desloppify/` + `scorecard.png` — both untracked locally, intentionally not
committed. Findings recorded here; **no fixes applied**.

### Headline scores

| Score | Value | Note |
|---|---|---|
| overall / strict | **21.6/100** (target 85) | Misleadingly low — see below |
| objective (mechanical only) | 86.4/100 | The real current state |
| verified | 86.4/100 | |

**Why strict is 21.6:** 75% of the score is subjective review, and all 20
subjective dimensions are unassessed (scored 0) — `desloppify review --prepare`
has never been run here. Mechanical health is ~86%. The low headline number is
a "not reviewed" artifact, not evidence of terrible code.

### Dimension health (open issues: 16,962; 15,862 files / 3.2M LOC)

| Dimension | Health | Items | Tier | Action |
|---|---|---|---|---|
| Test health | **39.4%** | 63,716 checks | T4 | autofix ← biggest drag (3,436 fix items) |
| Code quality | 93.7% | 14,851 | T3 | autofix |
| Duplication | 94.0% | 54,537 | T3 | refactor |
| File health | 96.5% | 7,036 | T3 | refactor |
| Security | 98.5% | 9,400 | T4 | move |

### Security flag (tool says "review before other cleanup")

~276 open security issues, but the visible ones are `hardcoded_secret_name`
heuristics — constant/variable *names* that mention secrets
(`AssetSyncSecretDataKeyPassword`, `ngcAPIKeyEnvironmentVariable` in
`control-plane/assets-service/pkg/...`), not leaked credential values. Likely
mostly false positives; needs one triage pass (`desloppify show security`) to
confirm before dismissing or fixing.

### Structural debt by area (top)

| Area | Items |
|---|---|
| runai-cluster/cluster | 3,183 |
| control-plane/runai-cli | 1,796 |
| control-plane/frontend | 1,228 |
| control-plane/workloads | 1,156 |
| control-plane/assets-service | 766 |
| control-plane/cluster-service | 631 |
| control-plane/identity-manager | 620 |
| control-plane/authorization | 599 |

### Tooling gaps (detectors under-reported this scan)

- stylelint, pmd, eslint: tooling unavailable → those detectors produced nothing
- Several detectors returned 0 marked "likely transient" (deprecated, naming,
  smells, unused, etc.) — a rerun with tooling installed may add findings

### Suggested next steps (per desloppify's own queue)

1. `desloppify review --prepare` — 20 subjective dimensions (75% of score)
2. Triage the 276 security findings (likely mass-dismiss as naming heuristics)
3. Test health focus (39.4%) — 3,436 auto-fixable items
4. Install stylelint/pmd/eslint and rescan for complete coverage
