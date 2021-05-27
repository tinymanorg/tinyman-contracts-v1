## OptIn
OptIn to the Validator App for Poolers/Swappers.
The user must OptIn to the app once before doing any mint/burn/swap/redeem transactions.

### Transaction:
0. App Call - OptIn to the Validator App
    - signed by Pooler/Swapper

```
{
  "txn": {
    "type": "appl",
    "snd": "{POOOLER/SWAPPER_ADDRESS}",
    "apid": {VALIDATOR_APP_ID},
    "apan": 1, // OnComplete: OptIn
    "apaa": []
    ...
  },
}
```


### Validator App State Changes
#### Global State
None
#### User Account Local State
None
