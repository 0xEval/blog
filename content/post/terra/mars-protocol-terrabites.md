---
title: Notes on the Terra Bites Podcast about Mars Protocol w/ @ZeMariaMacedo (Head of Delphi Labs)
date: Mon Sep  6 17:58:12 +08 2021
categories:
  - "Terra"
---

## Relationship Delphi Digital <> TerraForm Labs

Pete:

"How did you end up building on Terra ecosystem?"

José:
- Interest in algorithmic stablecoins
- Terra well developed and under the radar
- Delphi Labs active contribution = help building missing core primitives (money market, AMMs)
- Not maxis (also look into Solana), but most efforts focused on Terra at the moment

## What is Mars protocol? in comparison to Anchor and money markets like Aave/Compound

- Mars = on-chain, non custodial, community-governed bank
- Attracts liquidity and provide lending while managing insolvency risks

## Mars compared with Anchor

Anchor:
- Anchor = savings as a service
- Solely focused on providing a fixed-rate (~20%) on UST deposits
- Only accept yield-bearing collateral (PoS assets) in order to achieve fixed savings rate
- Anchor ~= specialized Mars, closest equivalent to crypto risk-free stable savings account
 
 Mars:
- Mars = decentralized banking protocol.
- Rates fluctuate and are determined by the pool(s) utilization rate (borrowing demand)
- Unlocking leverage on non-yield bearing assets (e.g: BTC)

## Mars compared with Aave/Compound

- Existing Terra assets (e.g: mAssets) will be there as collateral options
- Onboard assets from other chains with IBC & Wormhole (Col. 5)
- Three main differences compared to ETH money markets:

### 1) Uncollateralized lending:

TradFi markets:

- Loans are capital inefficient (low LTV)
- Exchanges are custodial and can liquidate you
- Small addressible markets since only depositors can borrow

> Same as if Airbnb would only let hosts stay at each others' houses

Mars design:

- Allows governance to extend uncollateralized credit lines to smart contracts
- Extends to a largely untapped market --> non-depositors borrowers
- Similar to offering of existing ETH protocols like Alpha Homora & Iron Bank
- Sounds risky but ~= bank extending credits to businesses based on credit history, expansion plans, assess whether they can pay back
- Mars "only" has to deal with smart contract risk. Audit code, review liquidation logic, assess overall risk level --> decide how much credit it can allow
- Embrace crypto trust minimization instead of re-creating TradFi in crypto
- Delphi Labs working on smart contract risk assessment framework
- Long-term any protocol can open credit line on Mars

*Example (leverage while staying long $MIR):*

![[mars_mir_example.png]]


### 2) Token economics

Revenues:

- Mars generates fees on the borrowing side
- No performance fees on Mars directly (but Dapps built on Mars might have some)

Governance:

- In Cream/Compound, governance tokens holders participating in votes do not bear the consequence of their action
- In Mars, governors (the Martian Council) need to have skin in the game == $XMARS staked
- $XMARS stakers suffer first in case of bad assets introduced to the system

### 3) Dynamic interest-rate model

Traditional MM:

- Set optimal utilization rate depending on asset
- 100% rate = illiquid, withdrawals impossible
- Target optimal utilization lower than 100% to stay liquid
- Manipulate % interest rate to keep it in control
- Can't react to external market condition (e.g: people are willing to pay a high interest rate if the %APR is sufficiently high)

Mars / Oiler Network (Ethereum):

- Block-by-block dynamic system that continuously adjust depending on utilization rate & optimal target rate
- Based on control theory (active contribution from physics PHD academic)

# Solvency issue scenario

Maker & Aave:

- Maker = mint $MKR to backstop the protocol in case of $DAI insolvency (e.g: March 2020)
- Aave = diversified safety module backstops the protocol
- Governors must have skin in the game

Mars:

- Above solution has a flaw
- Negative reflexivity in case of shortfall event.
- Example with AAVE: 
 
	 Anticipate dsafetselling pressure from safety module -> people sell AAVE in advance -> safety module has to sell more -> protocol mint more tokens -> increased selling pressure -> repeat

- Tranche-based system instead:

	Tranche 1 Insurance Funds (aUST) - from protocol-generated fees. First one to be wiped out in case of shortfall event

	Tranche 2 Martian Council (MARS) - protocol governance token holders

	Tranche 3 (community decision) - issue a debt token like Yearn / mint tokens / do something else

![[mars_solvency_tranches.png]]

# Launch & future plans

Expected launch:

- Waiting for Columbus 5 update
- A month or two from audits (as of early September)
- No information about token release for now
- Mars' future will be determined by the community as more assets come into the protocol

Future plans:

- Co-incubated by Delphi Labs (risk framework), IDEO (design), other contributors (smart contract dev)
- Big goal = create innovative governance structure to coordinate all these contributors
- DAOs are the most interest part of crypto but are very innefficient at the moment
- Inspiration from Yearn governance 2.0

# Show notes:

Read the Mars Red Paper - [https://mars-protocol.medium.com/mars...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbjZqZWJnOFc3N2FOUzNJN1F2NUJVUDJfcGdFUXxBQ3Jtc0ttcWxfSjU3cEhLTERZRnFxaWJtdVdrWUlCbEE2M2ZFQkFrNGFaZVNkMFd5bmVERzdaa0RoLXVnbmdINlhLYWdiZ01ERFowOEZGUEE0ZVFTbEFtbzJGSTd0WlZqSXUzTUJXNlQ5c0pNc0g2Z2dQRHM1Yw&q=https%3A%2F%2Fmars-protocol.medium.com%2Fmars-protocol-litepaper-1-0-60d1b024405a)
