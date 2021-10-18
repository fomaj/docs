---
sidebar_position: 4
---

# Contracts

> Please read [How it all works](/how-it-works) before reading this section.

## Contract Details
There are three main contracts in the fomaj system.

- Fomaj Contract
- FMJToken Contract
- PrizePool Contract

### FomajContract
`0x887111c0C8A60c7085A2004c0843DeCa4344aaF7`

https://github.com/fomaj/contracts/blob/master/contracts/core/Fomaj.sol

The starting point for this contract comes from Pancakeswaps prediction contract (MIT Licenced).
https://bscscan.com/address/0x18b2a687610328590bc8f2e5fedde3b582a49cda


There were A LOT of changes made on top of it (It turned out to be a completly different contract)

Some of them are:

- __Staking requirments__: 
Users need to stake `X` amount of `FMJ` tokens to the contract to be able to elegible to participate in the market. 
- __Reclaim Funds__:
Users can reclaim the amount they bet, no matter the outcome of the prediction. 
- __Using FMJ Tokens__:
Pancake contract uses BNB to bet on markets. Fomaj uses FMJ tokens. 
- __Bet on FLAT outcome__:
Uses can bet on BULL/BEAR and FLAT outcome. 

### FMJToken Contract
`0xaE666767443D0cAa30F0a785E8070878e1fb68a0`

https://github.com/fomaj/contracts/blob/master/contracts/core/FMJToken.sol

This is a typical ERC20 contract with an addition of transaction fees that goes to PrizePool contract.  

### PrizePool contract
`0xBE10F2D8DFd27A43d2653e47612D93B04a51DB2D`

https://github.com/fomaj/contracts/blob/master/contracts/core/FomajPrizePool.sol

A simple storage contract to store the prizes that's meant to distribute to winners

---

In addition to the above these contracts are used to demostrate the system. 
- Oracle contracts
- Test token faucet

### Oracle contracts
`0x9346980F92b46F5166a8e2d63507C6E4041cE8b4`

https://github.com/fomaj/contracts/tree/master/contracts/oracle

Fomaj requires an oracle to determine the price of CKB/USD. Since Chainlink is not available officially, a "dummy" chainlink contract was deployed. 

This is a `fork` of oracle-contracts provided by Nervos dev-rel team:

https://github.com/Kuzirashi/chainlink-nervos

### Faucet contract
`0xa0816B9FA3daED791090228b50C5cbDce2110687`

https://github.com/fomaj/contracts/blob/master/contracts/faucet/FMJFaucet.sol

A facuet contract to send `$FMJ` token for testing purposes. 

## Deploying contracts
This section explains how to deploy the the above contracts locally. 

### Clone the repository

```
git clone git@github.com:fomaj/contracts.git

cd contracts
```

### Install the dependencies

```
yarn install
```

### Setup environment variables

Rename `.env.example` to `.env` and fill the required enviroment variables

```
mv .env.example .env
```

```
# core -----------------------------------------------
CORE_PRIVATE_KEY="Your eth account private key, must have a Godwoken L2 account"

# These are different variable of fomaj system.
INTERVAL_SECONDS="900"
BET_PRICE_RANGE="5"
BUFFER_SECONDS="30"
MIN_BET="10000"
CLOSE_TIME_MULTIPLIER="4"
MIN_STAKE="100000"
MIN_PRIZE_AMOUNT="10000"
TOKEN_TX_PERCENTAGE=50 
FAUCET_TOKEN_AMOUNT=500000

# these values need to be filled after deployment to execute the fomaj system
FOMAJ_ADDRESS=""
PRIZE_POOL_ADDRESS=""
TOKEN_ADDRESS=""
FAUCET_ADDRESS=""

# oracle -----------------------------------------------
ORACLE_UPDATER_PRIVATE_KEY="A seperate private key to keep oracle updated, must have a Godwoken L2 account"
MESSARI_API_KEY="messari api key to get CKB/USD price"
CKB_USD_AGG="Fill this after deploying oracle"


NERVOS_PROVIDER_URL="https://godwoken-testnet-web3-rpc.ckbapp.dev"

```

### Compile the contracts.

```
yarn compile
```
 This will generate the required type defenitions for scripts in the `scripts` directory. Now we are ready to deploy. 

### Deploy oracle

```
yarn deployOracle
```
In the console the CKB/USD aggregator address will be printed. 

```
Aggregator CKB / USD deployed at: 0xa49fDC14b72Aa28eFd9A44143CbAbc0955E2CA39
```

Update `CKB_USD_AGG` in `.env` file with this value.

### Keep oracle up to date
Until we have a proper chainlink in polyjuice, the price of CKB/USD needs to be updated regurlarly.

A script in `scripts/oracle/update.ts` is used for this. 

Open a different terminal and run. 

```
yarn updateOracle
```

If you had setup `MESSARI_API_KEY` in the `.env` file properly, this script will update the oracle every few minutes.

### Deploying main contracts

This command likely takes some time to complete. The script we are about to execute `scripts/core/deploy.ts` does the following thigns

1. Deploy main Fomaj contract
2. Deploy prize pool contract
3. Deploy faucet contract
4. Deploy FMJ token contract
3. Set token of prizepool
4. Set token of faucet
5. Exclude main contract from transaction fees
6. Kickoff fomaj initial round

After each task, there will be a `30` second wait time. 

Once completion, you will see an output like the following in the console. 

```
Using RPC: https://godwoken-testnet-web3-rpc.ckbapp.dev
Deploying contract: Fomaj
Contract deployed: 0x829D6610433e41a55973F71104a74e5D3b3f84B2
Waiting 30 seconds.
Deploying contract: PrizePool
Contract deployed: 0x7F51Fe436De99B221844fC72AE9d795980ef9394
Waiting 30 seconds.
Deploying contract: Faucet
Contract deployed: 0x0A398e56593dDE11F54b882ad908f9B32C788c17
Waiting 30 seconds.
Deploying contract: FMJToken
Contract deployed: 0x07fc06e9Aa5FFB6b86a660a9B9095E3cE38406AF
Waiting 30 seconds.
Setting prize pool
Prize pool set.
Waiting 30 seconds.
Changing faucet token address
Token address set.
Waiting 30 seconds.
Exluding Fomaj contract from fees
Excluded
Waiting 30 seconds
Execuing kickoff
Kickoff success!
Done in 300.57s.
```

You have successfully deployed the contracts. 

Copy the contract addresses shown and replace them in `.env` file. 

### Executing contracts

The genesis round need to be started first. Then each round needs to be `executed` manually. This is done via `scripts/core/execute.ts` script. Runing the following command (and keep it running) will do all the required works. 

```
yarn execute
```

This is a limiation that you can find in `pancakeswap-prediction` as well. An operator account starts and excutes rounds. 

This limitation can be solved with a governance contract. By allowing anyone to call the governance contract which calls the `Fomaj` contract to close/start rounds. This is not currently implemented. Partially because of time contraints of hackathon and partially because  `block.timestamp` of polyjuice needs to be clearly understood :).  Fomaj will look into implementing this once `Polyjuice` is launched on mainnet.  