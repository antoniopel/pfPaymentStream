# Testing
- Since this project uses Chainlink oracles testing the project requires an infura/alchemy/geth node
- To set your node add the `GETH_NODE` env variable in `.env`
- Provided node will be used to spawn a local mainnet fork

        npm install
        npx hardhat test


# Additions to the original design specs:
- `_addToken(address tokenAddress, address oracleAddress)`
	- adds the corresponding ChainLink oracle address
- `delegatePausable(uint streamId, address delegate) onlyPayerOrDelegated(streamId)`
	- Delegates pausable capability to `address delegate`
- `claimable (uint streamId)`
	- returns accrued amount in USD (18 decimals) for `streamId`
- `claimableToken (uint streamId)`
	- returns accrued amount in `token` value using Chainlink' oracle
- `getStreamsCount()`
	- return total no. of streams: useful for looping through `streams` using `getStream(id)`

# Design specs
[Original design specs](https://docs.google.com/document/d/17xmWzQTd_gW2GGcn-mgoRBR6kHcjV--LJhkv-frAcls/edit#heading=h.cljfvymqw9x2)
# Outcomes

-   A payment stream is a per-Ethereum-block drip stream of abstract USD to a payee address.
    
-   A payer may create, start, or stop payment streams to a payee address.
    
-   Each payment stream is associated with a token <XYZ>, and has received an ERC20 approval from the funding address.
    
-   Payees may Claim the unlocked, unclaimed USD balance of the payment stream.
    
-   A Claim will consult an on-chain price oracle USDC/XYZ, calculate the amount of XYZ coins to be paid, and transfer that amount of XYZ to the payee.
    

# Implementation

## createStream(payee address, payee USD amount, payer token, funding address, end time)

Create a new payment stream from payer to payee. Funds may be from any account; funding account must approve this contract debiting this amount.

## pauseStream(stream ID)

Stop the drip.

## unpauseStream(stream ID)

Restart the drip.

## setPayee(stream ID, address)

Change stream payee address.

## setFundingRate(stream ID, USD amount, end time)

Change stream funding rate.

## setFundingAddress(stream ID, address)

Change stream funding address.

## claim(stream ID)

Payee receives nnnn.nn XYZ tokens, derived from a calculation of accumulated-but-not-claimed USD balance. The amount of XYZ tokens is calculated from an on-chain XYZ/USDC price oracle at claim time.