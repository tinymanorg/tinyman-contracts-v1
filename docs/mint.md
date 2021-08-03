## Mint
Mint Pool assets in exchange for transferring assets to the Pool account.

### Transaction Group:
0. Pay - pay fees in Algo from Pooler to Pool
    - fees to cover Tx 1,4
    - Signed by Pooler
```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOL_ADDRESS}",
    "snd": "{POOLER_ADDRESS}",
    "amt": 2000,
    "fee": 1000,
    ...
  },
  "sig": "{POOLER_SIG}",
}
```
1. App Call - NoOp call to Validator App with args ['mint'], with Pooler account
    - Signed by Pool LogicSig

```
{
  "txn": {
    "type": "appl",
    "snd": "{POOL_ADDRESS}",
    "apid": {VALIDATOR_APP_ID},
    "apan": 0, // OnComplete: NoOp
    "apaa": ['bWludA=='], // ['mint']
    "apat": [{POOLER_ADDRESS}],
    "apas": [{ASSET1_ID}, {ASSET1_ID}, {LIQUIDITY_ASSET_ID}] // or just [{ASSET1_ID}, {LIQUIDITY_ASSET_ID}] if asset 2 is Algo
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

2. AssetTransfer - Transfer of asset 1 from Pooler to Pool
    - Signed by Pooler

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOL_ADDRESS}",
    "snd": "{POOLER_ADDRESS}",
    "xaid": {ASSET_1_ID},
    "aamt": {ASSET_1_AMOUNT},
    "fee": 1000,
    ...
  },
  "sig": "{POOLER_SIG}",
}
```

3. (a) AssetTransfer - Transfer of asset 2 from Pooler to Pool
    - If asset 2 is an ASA
    - Signed by Pooler

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOL_ADDRESS}",
    "snd": "{POOLER_ADDRESS}",
    "xaid": {ASSET_2_ID},
    "aamt": {ASSET_2_AMOUNT},
    "fee": 1000,
    ...
  },
  "sig": "{POOLER_SIG}",
}
```

3. (b) Pay - Transfer of Algo from Pooler to Pool
    - If asset 2 is Algo
    - Signed by Pooler

```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOL_ADDRESS}",
    "snd": "{POOLER_ADDRESS}",
    "amt": {ASSET_2_AMOUNT},
    "fee": 1000,
    ...
  },
  "sig": "{POOLER_SIG}",
}
```

4. AssetTransfer - Transfer of liquidity token asset from Pool to Pooler
    - Signed by Pool LogicSig
    - Amount is minimum expected amount of liquidity token allowing for slippage

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOLER_ADDRESS}",
    "snd": "{POOL_ADDRESS}",
    "xaid": {LIQUIDITY_ASSET_ID},
    "aamt": {LIQUIDITY_ASSET_AMOUNT},
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

#### Pooler Account Local State
* `{POOL_ADDRESS}e{LIQUIDITY_ASSET_ID}: {e}` // excess liquidity asset amount available for redemption
