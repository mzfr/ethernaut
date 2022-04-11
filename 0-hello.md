Ethernaut Address: 0xD991431D8b033ddCb84dAD257f4821E9d5b38C33
Player Address: 0xB83c1038Fde9fA12e7554289B83Df954ea7157d4

# Hello World

The hint said see `ABI` so I ran `contract.abi` and saw the `password` function.

```js
contract.password()
Promise { <state>: "pending" }
​
    <state>: "fulfilled"
    <value>: "ethernaut0"
```

There was another `authenticate` function. It required only 1 argument i.e `passkey`

```json
{
  "inputs": [
    {
      "internalType": "string",
      "name": "passkey",
      "type": "string"
    }
  ],
  "name": "authenticate",
  "outputs": [],
  "stateMutability": "nonpayable",
  "type": "function",
  "signature": "0xaa613b29"
}
```

So I ran `contract.authenticate("ethernaut0")` and once that was done I just used `contract.getCleared()` to check if it worked and got `true` as the return value.