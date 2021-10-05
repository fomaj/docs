---
sidebar_position: 5
---

# Contracts

There are three main contracts in the fomaj system.

- Fomaj Contract
- FMJToken Contract
- PrizePool Contract

## FomajContract

The starting point for this contract comes from Pancakeswaps prediction contract (MIT Licenced).
https://bscscan.com/address/0x18b2a687610328590bc8f2e5fedde3b582a49cda


There were A LOT of changes made on top of it (https://www.diffchecker.com/7EFKmDYE).

Some of them are:

#### Staking requirments
Users need to stake `X` amount of `FMJ` tokens to the contract to be able to elegible to participate in the market. 

### Reclaim Funds
Users can reclaim the amount they bet, no matter the outcome of the prediction. 

### Using FMJ Tokens
Pancake contract uses BNB to bet on markets. Fomaj uses FMJ tokens. 

### Bet on FLAT outcome
Uses can bet on BULL/BEAR and FLAT outcome. 

## FMJToken Contract

This is a typical ERC20 contract with an addition of transaction fees that goes to PrizePool contract.  

## PrizePool contract

A simple storage contract to store the prizes that's meant to distribute to winners