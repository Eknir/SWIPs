---
Title: Honey to money oracle
Author: Aron Fischer <aron@ethswarm.org>, Rinke Hendriksen <rinke@ethswarm.org>, Vojtech Simetka <vojtech@iovlabs.org>
Discussions-to: URL will be provided
Status: Draft
Type: Standards track
Category: Core
Created: 31-07-2019
---

## Simple summary 
SWARM needs a mechanism to allow fluctuation of the exchange rate between honey (SWARMs internal accounting unit) and the currency used to pay (default: Ether). We propose an on-chain price oracle that returns an exchange rate + validUntil tuple.  

## Abstract 
The ability to update prices is required in order to:
* Facilitate experimentation with the absolute price of honey (e.g. start at a low price and gradually increase);
* Buffer exchange rate fluctuations of the currency used for payments;
* Lower the price of SWARM in the long-run as the costs of operating a SWARM-node decrease.
* Coordinate price changes across the swarm network.

We propose an on-chain price oracle that returns the current exchange rate when queried. The price-oracle will initially be managed by SWARM developers and stakeholders.

## Motivation
Nodes keep track of the balances with each other nodes using an imaginary currency 'honey' as the accounting unit. Honey is not a currency in which nodes can settle their balances with each other, so there is a requirement to facilitate converting between honey and a payment currency (currently Ether). Furthermore, it is desirable that this conversion rate can fluctuate since:
* The SWARM developers plan to initially set the price of honey artificially low and progressively increase this price as the network matures, until the price is sufficiently high to incentivize nodes to offer their services to the network while being low enough not to scare away potential users of SWARM. 
* We expect the price of the payment (crypto-)currency to fluctuate in value against the (fiat) costs of operating a SWARM node. As the real benefits (i.e. the worth of your compensation) of operating a SWARM node shouldn't fluctuate, the honey price should adapt too to buffer the exchange rate fluctuations of the payment currency. (recall: "accounting is in honey, costs are in fiat, payment is in crypto".)
* The cost of operating a SWARM node is expected to decrease over time, the price of honey should decrease as well to reflect this. 

Updating the honey price should be atomic,network-wide operation: either all nodes upgrade or none. 
Currently price changes are hard coded in the SWARM source code, meaning only nodes running the same release will agree on price; in order to assure that all peers agree on pricing updated nodes will refuse to connect to older nodes. While this assures price homogeneity among connected peers, it causes a network split and re-merging during every upgrade. 
This SWIP proposes an on-chain price oracle to enable an atomic update process of honey prices without requiring nodes to update their source-code. 
## Specification
* The SWARM source will reference the address of a honeyToMoney price oracle
* The price oracle implements the HoneyToMoney interface (to-be specified) 
* The HoneyToMoney contract will be initially owned by an M/N multi-sig of SWARM developers and stakeholders.
* Before sending a payment (denominated in currency), a node will query the honeyToMoney price oracle to get the most up-to-date price conversion. 
* Upon receiving of payment, nodes will request the honey amount of this payment by looking up the quoted value of the oracle at the time in which the payment was received. This might cause payment imbalances between nodes, as the oracle might be queried one or several blocks before the transaction was sent. To avoid this situation, we propose that nodes include a payload with the transaction, specifying the (very recent) time at which the oracle was queried.

## Rationale
To be defined

## Backwards Compatibility 
This SWIP is backward compatible as long as the price oracle quotes the same prices as is currently hard-coded in non-upgraded SWARM nodes. It is up to the owner of the MsgToHoney oracle to ensure that he does not update prices too much while not all nodes run are on the new SWARM. Currently, the SWARM is not running with a live test net for settling prices, so we expect no problems with this SWIP if it is implemented before the SWARM will go live with price-incentivization. 
Test Cases
Not currently available
## Implementations 
Not currently available
## Copyright Waiver
 Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
