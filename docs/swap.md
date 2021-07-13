## Swap
Swap one asset (ASA or Algo) for another with the Pool.

### Transaction Group:

0. Pay - pay fees in Algo from Swapper to Pool
    - fees to cover Tx 1,2
    - Signed by Swapper
```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOL_ADDRESS}",
    "snd": "{SWAPPER_ADDRESS",
    "amt": 2000,
    "fee": 1000,
    ...
  },
  "sig": "{SWAPPER_SIG}",
}
```
1. App Call - NoOp call to Validator App with args ['swap', (fixed-input) or 'fo' (fixed-output)], with Swapper account
    - Signed by Pool LogicSig
    - Argument 1 'fi' specifies that the input (sell) is fixed but the output may vary with slippage
    - Argument 1 'fo' specifies that the output (buy) is fixed but the input may vary with slippage

```
{
  "txn": {
    "type": "appl",
    "snd": "{POOL_ADDRESS}",
    "apid": {VALIDATOR_APP_ID},
    "apan": 0, // OnComplete: NoOp
    "apaa": ['c3dhcA==', 'Zmk=' or 'Zm8='], // ['swap', 'fi' (fixed-input) or 'fo' (fixed-output)]
    "apat": [{SWAPPER_ADDRESS}],
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

2. (a) AssetTransfer - Transfer of sell asset from Swapper to Pool
    - If sell asset is an ASA
    - Signed by Swapper

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOL_ADDRESS}",
    "snd": "{SWAPPER_ADDRESS}",
    "xaid": {SELL_ASSET_ID},
    "aamt": {SELL_ASSET_AMOUNT},
    "fee": 1000,
    ...
  },
  "sig": "{SWAPPER_SIG}",
}
```

2. (b) Pay - Transfer of Algo from Swapper to Pool
    - If sell asset is Algo
    - Signed by Swapper

```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOL_ADDRESS}",
    "snd": "{SWAPPER_ADDRESS}",
    "amt": {SELL_ASSET_AMOUNT},
    "fee": 1000,
    ...
  },
  "sig": "{SWAPPER_SIG}",
}
```

3. (a) AssetTransfer - Transfer of buy asset from Pool to Swapper
    - If buy asset is an ASA
    - Signed by Pool

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{SWAPPER_ADDRESS}",
    "snd": "{POOL_ADDRESS}",
    "xaid": {BUY_ASSET_ID},
    "aamt": {BUY_ASSET_AMOUNT},
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

3. (b) Pay - Transfer of buy asset from Pool to Swapper
    - If buy asset is Algo
    - Signed by Pool

```
{
  "txn": {
    "type": "pay",
    "rcv": "{SWAPPER_ADDRESS}",
    "snd": "{POOL_ADDRESS}",
    "amt": {BUY_ASSET_AMOUNT},
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
* `o{ASSET1_ID}: {o}` // total outstanding unredeemed asset 1 amount
* `o{ASSET2_ID}: {o}` // total outstanding unredeemed asset 2 amount

#### Swapper Account Local State
* `{POOL_ADDRESS}e{ASSET1_ID}: {e}` // excess asset 1 amount available for redemption
* `{POOL_ADDRESS}e{ASSET2_ID}: {e}` // excess asset 2 amount available for redemption

