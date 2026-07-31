READ ${CODE_ROOT:-$HOME/Code}/agent-scripts/AGENTS.md BEFORE ANYTHING (skip if missing). If missing, also try: $HOME/repos/agent-scripts/AGENTS.md

# AGENTS.md

## Project

- This repository builds the OpenTelemetry Operator for Kubernetes.
- The Go module is `github.com/open-telemetry/opentelemetry-operator`.
- Use Go 1.21.
- The operator manages OpenTelemetry Collectors and workload auto-instrumentation.
- Read `CONTRIBUTING.md` for contribution requirements.
- Read `DEBUG.md` for debugging guidance.
- Read `RELEASE.md` for release guidance.

## Map

- `main.go` starts the operator.
- `apis/v1alpha1` and `apis/v1beta1` define Kubernetes APIs and webhooks.
- `controllers` contains reconcilers.
- `internal/manifests` builds managed Kubernetes resources.
- `pkg/collector` contains Collector behavior and upgrade logic.
- `pkg/instrumentation` and `pkg/sidecar` contain pod mutation logic.
- `cmd/otel-allocator` builds the target allocator.
- `cmd/operator-opamp-bridge` builds the OpAMP bridge.
- `autoinstrumentation` contains language instrumentation images.
- `config` contains CRDs, RBAC, webhooks, samples, and deployment manifests.
- `bundle` contains Operator Lifecycle Manager bundle artifacts.
- `tests` contains Chainsaw end-to-end suites and test applications.
- `docs/api.md` contains generated API documentation.

## Commands

- Run the full pre-commit gate with `make precommit`.
- Run unit tests with `make test`.
- Run linters with `make lint`.
- Format Go code with `make fmt`.
- Run Go vet with `make vet`.
- Run the CI gate with `make ci`.
- Build all Go binaries with `make all`.
- Run the operator locally with `make install run`.
- Regenerate project artifacts with `make generate manifests bundle api-docs reset`.
- Verify generated artifacts with `make ensure-generate-is-noop`.
- Prepare end-to-end tests with `make prepare-e2e`.
- Run end-to-end tests with `make e2e`.
- Reset changes from end-to-end preparation with `make reset`.
- Run OpenShift end-to-end tests with `make e2e-openshift`.
- Create a changelog entry with `make chlog-new`.
- Validate changelog entries with `make chlog-validate`.

## Rules

- Keep the repository compliant with Operator SDK and Kubebuilder scaffolding.
- Regenerate manifests after changes to Go API struct definitions.
- Include generated changes from `bundle`, `config`, `bundle.Dockerfile`, `apis/v1alpha1/zz_generated.*.go`, and `docs/api.md` when their sources change.
- Add a unit test for each bug fix.
- Add documentation and unit or end-to-end tests for new features.
- Add a unique `.yaml` changelog entry under `.chloggen`, unless the pull request uses `[chore]` or the `Skip Changelog` label.
- Do not edit `CHANGELOG.md` as a source file. Release processing generates it from `.chloggen`.
- Keep Collector configuration compatibility in mind. The Collector format can change between versions.
- Do not assume the operator validates Collector configuration content.
- Treat `deploy`, `undeploy`, `install`, and `uninstall` as operations against the current Kubernetes context.
- Install cert-manager before an in-cluster deployment that uses webhooks.
- Keep the runtime container non-root. The operator image runs as UID and GID `65532`.
- Preserve the scratch runtime image and its CA certificate bundle.
