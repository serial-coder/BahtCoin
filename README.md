<p align="center"><a href="#" target="_blank" rel="noopener noreferrer"><img width="500" src="images/RTHB-icon.png"></a></p>

# Thai Baht Coin backed by BTC
This project is the proof of concept for securely issuing Thai Baht Stable Coin named RTHB, which is backed by Bitcoin (BTC).

## Introduction
In the fiat currency world, the unit of money is typically backed by gold. Meanwhile, Bitcoin is like a new gold standard in the cryptocurrency. That is why we decided to do this project in order to prove that we can issue a stable coin in the unit of Thai Baht (we call RTHB) which is backed by Bitcoin. This project is just a proof of concept. There are several issues we do not cover or prove in this demonstration such as regulation proofs or practical use scenario proofs.

## How it works?
In Bitcoin ecosystem, there is an interesting sidechain platform called [RSK project (rootstock)](https://www.rsk.co/). RSK platform enables us develop EVM(Ethereum Virtual Machine) based smart contract in order to interact with BTC (they call RBTC in RSK platform). With the smart contract feature enabled, we can put some special business logics against Bitcoin ecosystem easily.

Our RTHB is an ERC20-compatible token with extra following features.
1. Anyone can invoke the function **issue()** along with the collateral RBTC and then get RTHB back. By default, the amount of RTHB would be issued as the proportion 1:1.5 (~66.67%) of the collateralized RBTC (this proportion is adjustable).
2. RTHB holders can freely transfer any amount of RTHB to anyone.
3. An owner of RTHB contract, who did the transaction (1), can switch back his/her fund to RBTC by way of invoking the function **claim()** along with the same amount of RTHB as specified in the contract.
4. In case the RBTC price drops which makes RBTC price on a specific contract less than or equal to 130% of the collateralized RBTC, the contract is open for the public to take over. Thus, anyone can call to the function **publicTakeover()** together with the amount of RTHB calculated by the equation: RTHB >= targetContract.RBTC * currentRate, to take over the contract and take away the collateralized RBTC.

## Example scenarios of RTHB
### Scenario 1 - RBTC price is stable

<img width="500" src="images/RTHB-Scenario-Price-Stable3.png">

1) Alice sends 1 RBTC at a current rate 1,500 THB/BTC to RTHB Smart Contract (calling **issue()**).
2) Alice receives 1,000 RTHB back (at 1.5 RBTC:1 RTHB ratio).
3) Alice decides to return 1,000 RTHB to get her own RBTC back (calling **claim()**).
4) Fortunately the rate is still the same as when the RBTC was deposited, so the RTHB Smart Contract would return 1 RBTC back to Alice.

### Scenario 2 - RBTC price is increasing

<img width="500" src="images/RTHB-Scenario-Price-is-Increasing.png">

- This scenario is similar to the first scenario. Since RBTC price is increasing, Alice is able to return 1,000 RTHB to get her own RBTC back at the rate 2,500 THB/BTC.

### Scenario 3 - RBTC price is decreasing

<img width="500" src="images/RTHB-Scenario-Price-is-Decreasing.png">

1) Alice sends 1 RBTC at a current rate 1,500 THB/BTC to RTHB Smart Contract (calling **issue()**).
2) Alice receives 1,000 RTHB back (at 1.5 RBTC:1 RTHB ratio).

- A month later, RBTC price is dropping to 1,200 THB/BTC. Thus, RTHB Smart Contract forces sell of Alice contract in question (when the rate ratio is lower than 1.3 RBTC:1 RTHB).

3) Bob takes over Alice contract by providing 1,200 RTHB (with some discount*) (calling **publicTakeover()**).
4) Bob takes away 1 RBTC and Alice loses her contract. However, Alice still holds 1,000 RTHB.

discount*: the system can offer some discount for incentive purpose. For instance, when a rate is 1,200 THB/BTC, the system can offer a sale at 1,190 THB/BTC.

## Features
- Issuing new RTHB by providing RBTC (at 1.5 RBTC:1 RTHB ratio).
- Oracle price feeding.
- Force sell for unhealthy contracts (below 1.3 RBTC:1 RTHB ratio).
- List of all contracts.

## To-do
- Implement some missing ERC20 functions.
