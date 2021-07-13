## Bootstrap
Setup a Pool for a pair of assets. The Pool account should be a LogicSig contract account.

### Transaction Group:
0. Pay - pay fees from Pooler to Pool
    - fees to cover Tx 1,2,3,4
    - Signed by Pooler
```
{
  "txn": {
    "type": "pay",
    "rcv": "{POOL_ADDRESS}",
    "snd": "{POOLER_ADDRESS",
    "amt": 675000,
    "fee": 1000,
    "note": "",
    ...
  },
  "sig": "{POOLER_SIG}",
}
```
1. App Call - OptIn call to Validator App with args ['bootstrap', asset1ID, asset2ID]
    - signed by Pool LogicSig

```
{
  "txn": {
    "type": "appl",
    "snd": "{POOL_ADDRESS}",
    "apid": {VALIDATOR_APP_ID},
    "apan": 1, // OnComplete: OptIn
    "apaa": ['Ym9vdHN0cmFw', '{ASSET1_ID}', '{ASSET2_ID}'] // ['bootstrap', asset1ID, asset2ID]
    "apas": [{ASSET1_ID}, {ASSET1_ID}] // or just [{ASSET1_ID}] if asset 2 is Algo
    "fee": 1000,
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

2. AssetConfig - create asset for liquidity token
    - signed by Pool LogicSig

```
{
  "txn": {
    "type": "acfg"
    "snd": "{POOL_ADDRESS}",
    "fee": 1000,
    "apar": {
      "an": "Tinyman Pool USDC-ALGO",
      "un": "TM1POOL"
      "au": "https://tinyman.org",
      "t": 18446744073709551615, // 0xFFFFFFFFFFFFFFFF
      "dc": 6,
    },
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

3. Asset OptIn - Pool opt in to Asset 1
    - signed by Pool LogicSig

```
{
  "txn": {
    "type": "axfer",
    "arcv": "{POOL_ADDRESS}",
    "snd": "{POOL_ADDRESS}",
    "fee": 1000,
    "xaid": {ASSET_1_ID},
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```
4. (Optional) Asset OptIn - Pool opt in to Asset 2
    - Only if Asset 2 is not Algo
    - signed by Pool LogicSig

```
{
  "txn": {
    "type": "axfer",
    "snd": "{POOL_ADDRESS}",
    "arcv": "{POOL_ADDRESS}",
    "fee": 1000,
    "xaid": {ASSET_2_ID},
    ...
  },
  "lsig": "{POOL_LOGICSIG}",
}
```

### Validator App State
#### Global State
None
#### Pool Account Local State
* `a1: {ASSET1_ID`
* `a2: {ASSET2_ID`
