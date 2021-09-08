---
title: Diving in ERC20 & ERC721 standards
date: Fri Jun 25 17:56:30 +08 2021
categories:
  - "Solidity"
---

### ERC20 Token Standard

The [ERC20](https://eips.ethereum.org/EIPS/eip-20) standard defines an *Application Programming Interface* for tokens within Smart Contracts. The main use-case of this standard is to dictate how should a cryptocurrency be defined by a Smart Contract.

ERC20-compliant tokens can be characterized by the following properties:

- They are be **interchangeable** with one another (fungibility).
- They are **reusable** by diverse applications like wallets, DEXes, etc.
- They are **divisible**, and have a defined total supply amount.
- Any ERC20 transaction will produce a detailed receipt.

In practice, the standard is actually pretty simple. These are the necessary **functions** to implement:

```solidity
// Returns the name of the token.
function name() public view returns (string)
// Returns the symbol of the token, usually a shorter version of the name.
function symbol() public view returns (string)
// There is no floats in Solidity. Token usually adopt the standard of 18 decimals to mimic the relationship between Ether and Wei.
function decimals() public view returns (uint8)
// Returns the amount of tokens in existence.
function totalSupply() public view returns (uint256)
// Returns the amount of tokens owned by an address (account).
function balanceOf(address _owner) public view returns (uint256 balance)
// The ERC-20 standard allow an address to give an allowance to another address to be able to retrieve tokens from it.
// This getter returns the remaining number of tokens that the spender will be allowed to spend on behalf of owner.
function allowance(address _owner, address _spender) public view returns (uint256 remaining)
// Moves the amount of tokens from the function caller address (msg.sender) to the recipient address.
function transfer(address _to, uint256 _value) public returns (bool success)
// Moves the amount of tokens from sender to recipient using the allowance mechanism. amount is then deducted from the caller's allowance.
function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
// Set the amount of allowance the spender is allowed to transfer from the function caller (msg.sender) balance.
function approve(address _spender, uint256 _value) public returns (bool success)
```
...as well as **Events**:

```solidity
event Transfer(address indexed _from, address indexed _to, uint256 _value)
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```

*Battle-tested implementations of the ERC20 standard are already publicly available and are recommended to follow, see [OpenZeppelin contract](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/token/ERC20)*

{{% notice note %}}
*Homework*: implementation of my own basic ERC20 token available at https://github.com/0xEval/learn-solidity/tree/main/kopi-erc20
{{% /notice %}}

### ERC721 Non-Fungible Token Standard (NFT)

Non-Fungible Tokens - *NFTs* in short - are a type of token that can be used to represent **ownership** of **unique** items. They are a sort of digital version of deeds with special properties.

{{% notice info %}}
**Fungibility** is the ability of a good or asset to be interchanged with other individual goods or assets of the same type. 
{{% /notice %}}

Based on the above, we can say that **non-fungible** assets are non-interchangeable because they have unique properties.

- Money is a prime example of something fungible, where a $1 bill is easily convertible into four quarters or ten dimes, etc.  

- On the other hand, a Pokemon card is a **non-fungible** asset as its intrinsic value can be tied to multiple unique factors such as year of production, physical condition, etc.

NFTs let us tokenize various types of assets. While it is generally associated with digital art or collectibles, the scope for NFTs is in fact much larger. There will be many more extravagant possibilities in the future.

{{< figure src="https://storage.opensea.io/files/a654bd999d3b9a5d67b7ac4a06d6d2b0.svg" width="200"  caption="Uniswap V3 NFT LP position" link="https://opensea.io/assets/0xc36442b4a4522e871399cd717abdd847ab11fe88/56656">}}


{{% notice note %}}
*Homework*: implementation of my basic ERC721 NFT available at https://github.com/0xEval/learn-solidity/bandung-erc721
{{% /notice %}}

### References

*ERC20*  
https://eips.ethereum.org/EIPS/eip-20
https://ethereum.org/en/developers/docs/standards/tokens/erc-20/
https://docs.openzeppelin.com/contracts/2.x/erc20

*ERC721*  
https://eips.ethereum.org/EIPS/eip-721
https://ethereum.org/en/developers/docs/standards/tokens/erc-721/  
[What Are NFTs and How Can They Be Used in Decentralized Finance? DEFI Explained](https://www.youtube.com/watch?v=Xdkkux6OxfM)