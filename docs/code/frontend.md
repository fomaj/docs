---
sidebar_position: 5
---

# Frontend

Frontend is a `NextJs` application. If you would like to deploy it locally, follow the instructions bellow. 

> If you have not, please check [contracts page](/code/contracts) before you continue. You will need contract addresses from that section to run the frontend locally.

> The interface design was inspired by `PlotX` market.  But their front-end is closed source. None of the code was copied from them. It was merely  a "design" inspiration. 

### Clone the repository 

```
git clone git@github.com:fomaj/frontend.git

cd frontend
```

### Install dependencies

```
yarn install
```

### Change contract addresses

Change the contract addresses shown in `constants/contracts.ts` to the contract you deployed. 

https://github.com/fomaj/frontend/blob/master/constants/contracts.ts

### Build and serve

```
yarn build

yarn start
```