# `eosio.forum` account creation and contract deployment

## Motivation

Since the launch of the EOS mainnet, users have been awaiting the first referendum. This proposal looks to deploy to production a community-developed forum smart contract that will be able to facilitate the hosting of proposals and votes on-chain.

For more background information, you can read prior blog posts:

https://www.eoscanada.com/en/eos-referendum-beta-announcement

https://www.eoscanada.com/en/eosio.forum-referendum-contract-update

https://www.eoscanada.com/en/a-proposed-referendum-tool-for-the-eos-mainnet-eosio.forum

Credits:
- Thanks to [EOS Rio](https://web.eosrio.io/) for the template for multisig proposal write-ups.
- Thanks to [Dan Larimer](https://github.com/bytemaster) for the original sample contract that inspired the final version
- Thanks to the other Block Producer teams who have helped with the project: [EOS Nation](https://www.eosnation.io/), [EOS42](https://eos42.io/), [EOS Tribe](https://eostribe.io/), [GenerEOS](https://www.genereos.io/), [Greymass](https://greymass.com/)
- Thanks to community feedback we have received, notably from: @Bootl3r, @mountain_cloud, and @thomasbcox

## Proposal review

Using `eosc` or `cleos` we can perform a detailed review of the multisig proposal.

### Walkthrough

1. Block Producers create a new account named `eosio.forum`, created by `eosio` with an authority structure so that `eosio` retains control over the official community forum.
2. Block Producers do not need to delegate CPU or bandwidth to the `eosio.forum` account itself, but if the need arises this can be done by anyone without a multisig proposal.
3. Block Producers authorize the purchase of enough RAM (approximately 300 KB) to store the contract and ABI.
4. Block Producers deploy the contract (the latest version as published on https://github.com/eoscanada/eosio.forum)
5. Block Producers deploy the associated ABI, which includes the Ricardian Contracts.


### 2 proposals

#### First proposal : Create Account, Buy RAM

```
$ eosc multisig review eoscanadaops forumstep1

Proposer: eoscanadaops
Proposal name: forumstep1


---------------------------------------------------------------------
----------------------- TRANSACTION HEADER --------------------------
---------------------------------------------------------------------

Expiration: 2019-01-21 12:49:09 +0000 UTC (in 833h18m52.208738s, analysis time: 2018-12-17 19:30:16.791262 +0000 UTC)
Expiration: 2019-01-21 12:49:09 +0000 UTC
Reference block number: 45094
Reference block prefix: 12ed3db6
Maximum net usage words (of 8 bytes, 0 = unlimited): 0
Maximum CPU usage in milliseconds (0 = unlimited): 0
Number of seconds to delay transaction (cancellable during that time): 0

---------------------------------------------------------------------
------------------------------ ACTIONS ------------------------------
---------------------------------------------------------------------

Context-free actions: 0

Actions: 2
1. Action eosio::newaccount, authorized by: eosio@active
Create a new account named "eosio.forum", created by "eosio" with the following authority structure:
{
  "active": {
    "threshold": 1,
    "accounts": [
      {
        "permission": {
          "actor": "eosio",
          "permission": "active"
        },
        "weight": 1
      }
    ]
  },
  "owner": {
    "threshold": 1,
    "accounts": [
      {
        "permission": {
          "actor": "eosio",
          "permission": "active"
        },
        "weight": 1
      }
    ]
  }
}


2. Action eosio::buyrambytes, authorized by: eosio@active
Account "eosio" is buying RAM for receiver "eosio.forum", for 307200 bytes (whatever the market value)
```

#### Second proposal : Set Code, Set ABI

**WARNING: This one can only be pushed to the network after the first proposal has been executed**
**It is not possible to proactively create this proposal since we have determined (after a first attempt), that setcode / setabi will perform a lower level check which will fail if done before newaccount has been succesfully completed**

```
$ eosc multisig review eoscanadaops forumstep2

...
```
