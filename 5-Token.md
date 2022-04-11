# Token

We start by looking into the ABI of the contract.
- `contract.balanceOf(player)` -> we see that we are given 20 tokens
- `contract.totalSupply()` -> we see the total supply is `21000000`


we see that the requirment for transfer is the following:
```js
require(balances[msg.sender] - _value >= 0);
```

The issue is that both `balances` and `_value` are unsigned integers so when we try to subtract wrap around happens.

The trick here was that we need to use another wallet address to call the `transfer` function. This is because we are doing `-`(subtraction) of unsigned numbers
it will never be negative, but will cause overflow. If we use same address(players addr) then it will cause underflow.

Ex: uint8(0) - uint8(1) = 255

So another wallet will have `0`, and sub will make it super large positive number which will pass the check. Then the balances will change.

## Solution

```js
contract.transfer("DIFF WALLET", 21)
```