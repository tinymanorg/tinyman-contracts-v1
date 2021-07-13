## Burn
Burn Pool liquidity assets in exchange for removing assets from the Pool.

### Transaction Group:

0. Pay - pay fees in Algo from Pooler to Pool
    - fees to cover Tx 1,2,3
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
1. App Call - NoOp call to Validator App with args ['burn'], with Pooler account

```
{
  "txn": {
    "type": "appl",
    "snd": "{POOL_ADDRESS}",
    "apid": {VALIDATOR_APP_ID},
    "apan": 0, // OnComplete: NoOp
    "apaa": ['YnVybg=='], // ['burn']
    "apat": [{POOLER_ADDRESS}],
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

2. AssetTransfer - Transfer of asset 1 from Pool to Pooler
    - Signed by Pool
    - Amount is minimum expected amount allowing for slippage

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOLER_ADDRESS}",
    "snd": "{POOL_ADDRESS}",
    "xaid": {ASSET_1_ID},
    "aamt": {ASSET_1_AMOUNT},
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

3. (a) AssetTransfer - Transfer of asset 2 from Pool to Pooler
    - If asset 2 is an ASA
    - Signed by Pool
    - Amount is minimum expected amount allowing for slippage

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOLER_ADDRESS}",
    "snd": "{POOL_ADDRESS}",
    "xaid": {ASSET_2_ID},
    "aamt": {ASSET_2_AMOUNT},
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

3. (b) Pay - Transfer of Algo from Pool to Pooler
    - If asset 2 is Algo
    - Signed by Pool
    - Amount is minimum expected amount allowing for slippage

```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOLER_ADDRESS}",
    "snd": "{POOL_ADDRESS"},
    "amt": {ASSET_2_AMOUNT},
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

4. AssetTransfer - Transfer of liquidity token asset from Pooler to Pool
    - Signed by Pooler

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOL_ADDRESS}",
    "snd": "{POOLER_ADDRESS}",
    "xaid": {LIQUIDITY_ASSET_ID},
    "aamt": {LIQUIDITY_ASSET_AMOUNT},
    "fee": 1000,
    ...
  },
  "sig": "{POOLER_SIG}",
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