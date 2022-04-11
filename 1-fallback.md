# Fallback

Instance address: 0x9fA85D681E5f36E9E6b8Fdf1755789BfcEDAcDE2

* Fallback is the function called when no other function matches the name called.
	- Its like do X,Y,Z and if none of them are possible just go to `fallback`

* We can see the following code:

```js
  function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if(contributions[msg.sender] > contributions[owner]) {
      owner = msg.sender;
    }
  }
```

- We can contribute less then `0.001` ether at a time
- if contribution of `sender` are more then that of the owner, sender becomes owner.
    - owners contribution are set to `1000` initially.
    - We can't send 1001 ETH (that will be super slow process)

* Another function is:

```js
 receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
  }
```

This basically says that is we `contribute` with some `value` it will make us the owner.

## Solution

```js
sendTransaction({to: contract.address, value: toWei("0.001", "ether"), from: player})
```

