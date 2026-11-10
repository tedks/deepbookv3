# Bazel package graph

The first child-owned Bazel target is `//scripts:typecheck`. It compiles the 14 existing TypeScript transaction script files using the `scripts/pnpm-lock.yaml` dependency graph and its TypeScript 5.6.2 peer package. The build emits JavaScript but does not execute it; these scripts can read a wallet, invoke the Sui CLI, and submit chain transactions at runtime.

With Bazel 8.7.0 available, build from this repository root:

```bash
bazel build //scripts:typecheck --jobs=2 --local_resources=memory=2048
```

This target does not cover the Rust indexer/schema or Move contracts. Cargo and Sui Move remain their native workflows. The Move locks reference the mutable `framework/mainnet` branch and request different compiler versions, so a portable Move action needs an immutable framework snapshot and matching compiler before qualification.
