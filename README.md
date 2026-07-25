# Restaking Core

**Moniversive Invariant Static (MIS)** — Creator **Chandler William Ferguson**

MIS-only (`.mis`). Compiler `bin/misc`. Settlement **clrty-1 / chain 1202**. Embed gates **3..=6**.

Backlinked to [https://github.com/clarity-fintech/moniversive_invariant_static_ML](https://github.com/clarity-fintech/moniversive_invariant_static_ML), [https://github.com/clarity-fintech/CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel), and CLRTY-1 MIS catalogs.

## Layout

- `mis/kernel/RestakingCoreKernel.mis`
- `mis/packs/` — 5 packs × 20 = **100** invariants
- `mis/sections/` — MIS + CLRTY-1 architecture backlinks
- `mis/backlinks/MisBacklinkIndex.mis`
- `manifests/`

## Compile

```bash
bin/misc mis/kernel/RestakingCoreKernel.mis --check --compact-letters
find mis -name '*.mis' -print0 | xargs -0 -n1 bin/misc --check --compact-letters
```

## Org

[clarity-fintech/restaking-core](https://github.com/clarity-fintech/restaking-core)
