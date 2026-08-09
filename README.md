# Restaking Core

> **Moniversive Invariant Static (MIS)** module · settles on **CLRTY-1** (chain **1202**) · compiler **`misc`**  
> Creator **Chandler William Ferguson** · Org [`clarity-fintech/restaking-core`](https://github.com/clarity-fintech/restaking-core)

MIS-only (`.mis`). Compiler `bin/misc`. Settlement **clrty-1 / chain 1202**. Embed gates **3..=6**.

This repository is **self-contained and 100% downloadable**: it ships the prebuilt MIS
compiler at [`bin/misc`](bin/misc), so a fresh clone builds and checks with no extra setup.

---

## Contents

- [What this is](#what-this-is)
- [Quickstart](#quickstart)
- [Command reference](#command-reference)
- [Architecture & layout](#architecture--layout)
- [Kernel & invariants](#kernel--invariants)
- [Code sample — an invariant pack](#code-sample--an-invariant-pack)
- [Network binding (CLRTY-1)](#network-binding-clrty-1)
- [Verification / test output](#verification--test-output)
- [Bootstrap & portability](#bootstrap--portability)
- [Backlinks](#backlinks)

## What this is

**Restaking Core** is written entirely in **MIS** (Moniversive Invariant Static, `.mis`) — a
letter-hashed, invariant-static language. Every module compiles under the **`misc`** kernel
(foreign kernels are rejected) to a deterministic *typed-letter root*, so a check is a
cryptographic proof that the module's invariants hold.

- **26** `.mis` modules: **1** kernel · **5** packs · **17** sections · **3** command modules
- **9** active invariants and **4** outcomes in the backlink kernel (verified below)
- Settlement network **CLRTY-1 / chain 1202**, RPC `https://rpc.clarity-fintech.com`
- Embed gates **3..=6**; deep root anchored to `moniversive`

## Quickstart

```bash
git clone https://github.com/clarity-fintech/restaking-core
cd restaking-core

# 0. (optional) refresh / rebuild the compiler — a prebuilt bin/misc is already committed
make bootstrap

# 1. compile-check the kernel + every .mis module in the repo
make check

# 2. validate the command surface
make commands-check

# 3. print the live CLRTY-1 network binding
make network-connect
```

## Command reference

| Command | What it does |
|---|---|
| `make bootstrap` | Ensure/refresh `bin/misc` (copies a shared build, or rebuilds from the CLRTY-MIS-Kernel Rust source). |
| `make check` | Compile-check the kernel and **every** `.mis` file with `--compact-letters`; prints the kernel proof JSON. |
| `make commands-check` | Compile-check only the command catalog under `mis/commands/`. |
| `make network-connect` | Runs `commands-check`, then prints the CLRTY-1 (chain 1202) binding, RPC, and command module. |
| `bin/misc mis/kernel/RestakingCoreKernel.mis --check --compact-letters` | Check a single module directly with the compiler. |

Direct compiler invocation (what the Makefile runs under the hood):

```bash
bin/misc mis/kernel/RestakingCoreKernel.mis --check --compact-letters
find mis -name '*.mis' -print0 | xargs -0 -n1 bin/misc --check --compact-letters
```

## Architecture & layout

```
mis/kernel/RestakingCoreKernel.mis
mis/packs/avs_layer.mis
mis/packs/operator_set.mis
mis/packs/restake_bond.mis
mis/packs/shared_security.mis
mis/packs/slash_relay.mis
mis/sections/ClrtyMisKernelRepo.mis
mis/sections/GradientBand.mis
mis/sections/LanguageRoot.mis
mis/sections/MisAllCodeIndex.mis
mis/sections/MisArchitecture.mis
mis/sections/MisChainIntake.mis
mis/sections/MisCodeIndex.mis
mis/sections/MisEcosystemRepos.mis
mis/sections/MisKernelActiveOnly.mis
mis/sections/MisKernelSource.mis
mis/sections/MisNativeKernels.mis
mis/sections/MiscCompiler.mis
mis/sections/MlKernelRepo.mis
mis/sections/ProtocolStructures.mis
mis/sections/StaticMlCatalog.mis
mis/sections/TickArchitecture.mis
mis/sections/TickStakingGradient.mis
mis/commands/RestakingCoreCommandCatalog.mis
mis/commands/RestakingCoreCommands.mis
mis/commands/RestakingCoreNetworkBind.mis
manifests/commands_manifest.json
manifests/domain_full_index.json
manifests/mis_repo_manifest.json
```

- **`mis/kernel/`** — the module's kernel entry: invariants + outcomes that gate everything else.
- **`mis/packs/`** — invariant packs (the executable logic units).
- **`mis/sections/`** — MIS + CLRTY-1 + edge backlink sections binding this repo into the ecosystem.
- **`mis/commands/`** — the callable command catalog and its CLRTY-1 network binding.
- **`manifests/`** — machine-readable domain, command, and repo manifests.

## Kernel & invariants

`mis/kernel/RestakingCoreKernel.mis` — the kernel declares the invariants the compiler enforces on every check:

```rust
// Moniversive Invariant Static (MIS) — deep root
// Creator: Chandler William Ferguson
// Compile: bin/misc <file>.mis --check --compact-letters
// Title: Restaking Core Kernel

module RestakingCoreKernel {
  invariant letter_hash_bound: letter_hash_root != @0;
  invariant deep_root_moniversive: deep_root == moniversive;
  invariant settlement_chain: chain_id == 1202;
  invariant settlement_network_clrty1: settlement_network == clrty_1;
  invariant extension_mis: source_extension == mis;
  invariant kernel_is_misc: compiler_kernel == misc;
  invariant active_kernel_only: active_kernel == misc;
  invariant no_foreign_kernel: foreign_kernel_active == false;
  invariant creator_bound: creator == chandler_william_ferguson;
  invariant domain_bound: domain == restaking;
  invariant clrty1_architecture_bound: clrty1_architecture_bound == true;
  invariant ecosystem_catalog_bound: ecosystem_catalog_bound == true;

  invariant pack_shared_security_flag: pack_shared_security_bound == true;
  invariant pack_avs_layer_flag: pack_avs_layer_bound == true;
  invariant pack_restake_bond_flag: pack_restake_bond_bound == true;
  invariant pack_operator_set_flag: pack_operator_set_bound == true;
  invariant pack_slash_relay_flag: pack_slash_relay_bound == true;

  outcome assert_kernel_misc() {
```

## Code sample — an invariant pack

`mis/packs/avs_layer.mis`:

```rust
// Moniversive Invariant Static (MIS) — deep root
// Creator: Chandler William Ferguson
// Compile: bin/misc <file>.mis --check --compact-letters
// Title: Restaking Core · AvsLayer

module RestakingCoreAvsLayer {
  invariant letter_hash_bound: letter_hash_root != @0;
  invariant deep_root_moniversive: deep_root == moniversive;
  invariant settlement_chain: chain_id == 1202;
  invariant settlement_network_clrty1: settlement_network == clrty_1;
  invariant kernel_is_misc: compiler_kernel == misc;
  invariant pack_avs_layer_bound: pack_bound == true;
  invariant catalog_backlink: mis_catalog_bound == true;

  invariant avs_layer_021: avs_layer_021_bound == true;
  invariant avs_layer_022: avs_layer_022_bound == true;
  invariant avs_layer_023: avs_layer_023_bound == true;
  invariant avs_layer_024: avs_layer_024_bound == true;
  invariant avs_layer_025: avs_layer_025_bound == true;
  invariant avs_layer_026: avs_layer_026_bound == true;
  invariant avs_layer_027: avs_layer_027_bound == true;
  invariant avs_layer_028: avs_layer_028_bound == true;
```

## Network binding (CLRTY-1)

`make network-connect` binds the command surface to the settlement network:

```
CONNECTED clrty-1/1202 restaking-core
RPC https://rpc.clarity-fintech.com
MODULES mis/commands/RestakingCoreCommands.mis
```

## Verification / test output

`make check` passes — the kernel proof for this repo:

```json
{
  "ok": true,
  "module": "RestakingCoreBacklinkIndex",
  "kernel": "misc",
  "invariant_count": 9,
  "outcome_count": 4,
  "typed_letters": 1152,
  "active_kernel_only": true
}
```

*Reproduce:* `make check` → `"ok": true`, module `RestakingCoreBacklinkIndex`, 9 invariants, 4 outcomes, 1152 typed letters.

## Bootstrap & portability

- A **prebuilt macOS `bin/misc`** is committed, so `make check` works immediately after clone.
- On other platforms (or to rebuild), run **`make bootstrap`** — it rebuilds `misc` from the
  [CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel) Rust source via `cargo`,
  or copies a shared build if one is present in a parent checkout.
- The compiler accepts **MIS only**; foreign kernels are rejected by design.

## Backlinks

- [clarity-fintech/CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel) — the sole active `misc` compiler
- [clarity-fintech/moniversive_invariant_static_ML](https://github.com/clarity-fintech/moniversive_invariant_static_ML) — MIS language root
- [clarity-fintech/restaking-core](https://github.com/clarity-fintech/restaking-core) — this repository

---
MIS · CLRTY-1 (chain 1202) · Creator Chandler William Ferguson
