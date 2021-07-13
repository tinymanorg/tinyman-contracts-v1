## Redeem
Claim back 'change' due to slippage in Mint/Burn/Swap process.

### Transaction Group:

0. Pay - pay fees in Algo from Pooler to Pool
    - fees to cover Tx 1,2
    - Signed by Pooler
```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOL_ADDRESS}",
    "snd": "{POOLER_ADDRESS",
    "amt": 3000,
    "fee": 1000,
    ...
  },
  "sig": "{POOLER_SIG}",
}
```
1. App Call - NoOp call to Validator App with args ['redeem'], with Pooler account
    - Signed by Pool LogicSig

```
{
  "txn": {
    "type": "appl",
    "snd": "{POOL_ADDRESS}",
    "apid": {VALIDATOR_APP_ID},
    "apan": 0, // OnComplete: NoOp
    "apaa": ['cmVkZWVt'] // ['redeem']
    "apat": [{POOLER/SWAPPER_ADDRESS}],
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

2. (a) AssetTransfer - Transfer of asset from Pool to Pooler/Swapper
    - If asset is an ASA
    - Signed by Pool

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOLER/SWAPPER_ADDRESS}",
    "snd": "{POOL_ADDRESS}",
    "xaid": {ASSET_ID},
    "aamt": {ASSET_AMOUNT},
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

2. (b) Pay - Transfer of Algo from Pool to Pooler/Swapper
    - If asset is Algo
    - Signed by Pool

```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOLER/SWAPPER_ADDRESS}",
    "snd": "{POOL_ADDRESS"},
    "amt": {ASSET_AMOUNT},
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```




### Validator App State
#### Global State
None
#### Pool Account Local State
* `a1: {ASSET1_ID}`
* `a2: {ASSET2_ID}`
* `o{LIQUIDITY_ASSET_ID}: {o}` // total outstanding unredeemed liquidity asset amount
* `o{ASSET1_ID}: {o}` // total outstanding unredeemed asset 1 amount
* `o{ASSET2_ID}: {o}` // total outstanding unredeemed asset 2 amount

#### Pooler Account Local State
* `{POOL_ADDRESS}e{LIQUIDITY_ASSET_ID}: {e}` // excess liquidity asset amount available for redemption
* `{POOL_ADDRESS}e{ASSET1_ID}: {e}` // excess asset 1 amount available for redemption
* `{POOL_ADDRESS}e{ASSET2_ID}: {e}` // excess asset 2 amount available for redemption