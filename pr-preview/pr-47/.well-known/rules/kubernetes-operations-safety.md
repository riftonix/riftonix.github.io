# Kubernetes operations safety

Mandatory safety rules for any kubectl or Kubernetes MCP interaction (including kubectl-mcp-server tools). These rules apply to all clusters: local, staging, and production.

## Context and namespace

- Always show and verify the current kubectl context before any command or tool call. Run `kubectl config current-context` (or the `get_current_context` tool) and state the result to the user first.
- Never switch cluster or context implicitly. A context switch requires explicit user approval, every time.
- Always specify the namespace explicitly (`-n <namespace>` or the `namespace` tool parameter). Never rely on the default namespace of the current context. If the user did not name a namespace, ask.

## Read before write

- Read the current state of a resource before modifying it: `kubectl get`, `kubectl describe`, or the corresponding MCP read tool. Never patch, apply, scale, or edit a resource you have not inspected.
- Prefer `kubectl diff -f <file>` (or `kubectl apply --dry-run=server -f <file>`) before `kubectl apply`. Show the diff to the user when the change is non-trivial.
- Never blindly apply generated YAML to production. Generated manifests must be read in full, reviewed against the cluster state, and confirmed by the user before touching production namespaces.

## Destructive and forced operations

- Never run `kubectl delete` without explicit user intent. Deleting is never an implementation detail; if a deletion seems necessary, propose it and wait for approval.
- Never use `--force`, `--grace-period=0`, `--cascade=force`, or any forced eviction option unless the user explicitly approved that exact flag for that exact operation.
- Never automatically create, modify, read, or rotate Secrets. Reading a Secret exposes sensitive data and writing one can break workloads. Any Secret operation requires explicit user instruction.

## After changes

- After any mutating operation, inspect the rollout status: `kubectl rollout status <resource>` (or the `kubectl_rollout` status mode). Do not report success before the rollout is confirmed.
- On failure, inspect events and logs before making another mutation: `kubectl get events`, `kubectl describe <resource>`, `kubectl logs <pod>`. Never respond to a failed mutation with another mutation.