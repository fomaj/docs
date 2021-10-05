---
sidebar_position: 2
---

# How it all works

In fomaj a user can predict if CKB/USD will go up/down or stay flat in a given period of time. 

If the user predicts correctly, they win rewards.

If the user predict incorrectly, they don't lose anything.

The system consists of rounds. A user have a chance to participate in each round. 

## FMJ Token
Fomaj has a token. `$FMJ`. This is currently an `ERC20` token on polyjuice L2. But in future this will be an `SUDT` token. 

This acts as both a utility and a governance token.

Users `Bet` on the market using these tokens. They also get rewards as these tokens..

## Staking requirement
There is no lose when a prediction is incorrect. This opens up bad players to spam. Staking requimrnet is how this is miniminzed. 

To participate in prediction rounds, initially users will have to stake `J` amount of `FMJ` tokens to the contract. This will be locked for `K` amount of time. In that time users can partcipate in any number of rounds. 

Currently these are set to

`J` = `100,000`

`K` = 6 months.

These are not finalzied values. And can be changed after the hackathon.

The more users decide to join FOMAJ to earn risk-free rewards, the more tokens they will have to buy and lock. This will increase the value of token in the long run. 

## Rewards

### Where does the reward come from?

There are two main ways to tackle this. Right now, fomaj follows the *second approach* listed bellow.

#### Minting
This is an approach that `pancakeswap-farms` follows. That is miniting. Pancakeswap mint `CAKE` tokens and gives the users who stake on farms. This makes the `CAKE` token inflationary. But they make it deflationary by buying back and burning tokens.

#### Transaction Fees
This is the approach `FOMAJ` follows.
Let's do an example. 

If *Alice* is sending *Bob* `100` `FMJ`, *BOB* will only get `95` FMJ. The remaining `5` tokens is the transaction fees. This goes to the prize pool. Whatever amount left in a prize pool at a given period of time is set as the prize for users participating in prediction rounds.

The more people participate to get free rewards, they will have to buy more `$FMJ`tokens to stake. This increase the prize pool and also the value of `FMJ` tokens. 

### How is reward calculated in each round?

After evaluating all the variables, I came up with the following formula. This also is subject to change after the hackathon.


![Formula](/img/formula.png)

## Timestamps

A round has three timestamps. 

- Start timestamp `t1` - This is the begining of a round. 
- Lock timestamp `t2` - Where betting is locked
- Close timestamp `t3` - Where winners are determined

`t2 - t1` is called an `INTERVAL`. Users can bet only in this timefram. 

`t3 - t1`` = `INTERVAL` * `CLOSE_TIME_MULTIPLIER`

The amount of `FMJ` tokens in the prize pool in `t3-t1` will be the amount that will be distributed to the winners.

The `INTERVAL` is currently set to `5` minutes.
The `CLOSE_TIME_MULTIPLIER` is currently set to `4`. 

But these valeus are not finalized yet.


