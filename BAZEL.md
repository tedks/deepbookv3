# Bazel package graph

The first Bazel target is `//scripts:typecheck`. It compiles the 14 existing TypeScript transaction script files using the `scripts/pnpm-lock.yaml` dependency graph and its explicit TypeScript 5.6.2 dependency. The build emits JavaScript but does not execute it; these scripts can read a wallet, invoke the Sui CLI, and submit chain transactions at runtime. Bazel may fetch integrity-pinned dependencies when resolving the module and pnpm lock; no wallet or chain operation is part of this target.

The pinned Node toolchain supports Linux x86-64. With Bazel 8.7.0 available on that platform, build from this repository root:

```bash
bazel build //scripts:typecheck --jobs=2 --local_resources=memory=2048
```

This target does not cover the Rust indexer/schema or Move contracts. Cargo and Sui Move remain their native workflows. The Move locks reference the mutable `framework/mainnet` branch and request different compiler versions, so a portable Move action needs an immutable framework snapshot and matching compiler before qualification.
