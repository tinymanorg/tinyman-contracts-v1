
## Overview
The Tinyman contracts consist of a single stateful smart contract (the Validator) and a template for a stateless smart contract for each Pool.

The Validator app is created/deployed to the network by the Tinyman team. The Pool contract accounts are created from the Pool LogicSig template by the first Pooler who wishes to provide liquidity.

Pool contract account addresses are discovered or verified by Swappers/Poolers using the Pool LogicSig template to generate a specific Pool LogicSig and retrieving the address.

## Operations
The following sections describe each of the possible operations that can be performed with the Tinyman smart contract app.

### Create
Create/deploy the Validator App

[Create Docs](create.md)

### Bootstrap
Setup a Pool for a pair of assets.

[Bootstrap Docs](bootstrap.md)

### OptIn
Swappers/Poolers must OptIn to the Validator App

[OptIn Docs](optin.md)

### Mint
Mint Pool assets in exchange for transferring assets to the Pool account.

[Mint Docs](mint.md)

### Burn
Burn Pool liquidity assets in exchange for removing assets from the Pool.

[Burn Docs](burn.md)

### Swap
Swap one asset (ASA or Algo) for another with the Pool.

[Swap Docs](swap.md)

### Redeem
Claim back 'change' due to slippage in Mint/Burn/Swap process.

[Redeem Docs](redeem.md)


### Redeem Fees
Redeem Protocol Fees to the Creator account

[Redeem Fees Docs](redeem_fees.md)
