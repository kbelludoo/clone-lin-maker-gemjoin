# clone-lin-maker-gemjoin

Experimental **LIN** clone of MakerDAO / Sky `GemJoin.join` / `GemJoin.exit` — the
ERC20 collateral adapter (`vat.slip` ±wad plus `transferFrom` / `transfer`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/join.sol` sha256 `96fceff4511aa976db5c6b144d83f4dd1bde832dd2f49fd6e54e69343b9583f9` blob `42b01c91c8f2a6b776440e16dea2c94497d966b0`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_gemjoin.lin`](src/lin_maker_gemjoin.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_gemjoin_c11.c`](test/oracles/maker_gemjoin_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_gemjoin_kernel.c`](test/oracles/maker_gemjoin_kernel.c)
- **Proofs (lin-open only):** `examples/maker_gemjoin/` and `make maker-gemjoin-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256 wad. Not a production GemJoin replacement.

Named goldens (uint64-scale wad):

| id | call | value |
|---|---|---|
| JOIN usr_gem' | wad=1000 usr_gem=5e9 sender_tok=1000 | **5000001000** |
| JOIN adapter' | ERC20 held by adapter | **1000** |
| JOIN sender_tok' | ERC20 after transferFrom | **0** |
| EXIT usr_tok' | live=1 wad=1000 usr_tok=7 | **1007** |
| CAGE join | live=0 | **ok=0** |
| CAGE exit | live=0 | **ok=1** |

`live` / `usr_gem` / `sender_gem` / `adapter_token` / `sender_token` / `usr_token` / `wad` are arguments (Solidity uses storage `live`, `vat.slip`, `gem.transferFrom` / `gem.transfer`). Join after cage fail-closes; exit does not (inverse of DaiJoin). `7e18+3e18` fail-closes while naive i64 wraps.

## Reproduce proofs (in lin-open)

```
make maker-gemjoin-proof
```

## Not claimed

Uint256 wad / mainnet GemJoin / ERC20 transfer host / `vat.slip` host / `bytes32` ilk parity.
LIN replacing Maker, Sky, GemJoin, or the EVM. TVL or dollar savings. zk soundness.
`DaiJoin` wad→rad. Hidden adapter storage. `wad = 2**255` exit overflow.
