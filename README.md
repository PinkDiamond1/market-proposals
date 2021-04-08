# Market Proposals

Vega allows domain experts to design, launch, and manage markets without needing to write any code or worry about price determination, margin calculation, or liquidity incentivisation.Introduction to the derivatives toolbox is found [https://blog.vega.xyz/vegas-derivatives-toolbox-b098b7de42e](here). 

This is an open source community contribution repository to provide tools for:
- proposing new markets
- evaluating market proposals for governance 
- evaluating market specifications for trading

## How to create a market proposal

Clone this repo to help prepare a new market proposal and:

- share your repo with the community; and/or
- raise a PR on this repo which adds your proposal to the list of [proposals](./proposals).

### Steps for proposing a market:

1. Write a market proposal in the markets proposals section of the [community forums](https://community.vega.xyz/c/market-proposals/22) 
  - Explain context
  - Surface high level definition of the market

1. In [Template Proposals](template-cash-settled-future.ipynb) copy template-cash-settled-future.ipynb and rename (to suit your market)

1. Set out your market in your python notebook, including creating the JSON (last cell)

1. Raise a Git pull request and link to this in your forum post, so the community may discuss and feedback either on the forums or in the PR

1. Once you have incorporated the feedback as you see fit, submit the proposal to the network using the Propose window on Console


## Examples

1. [This Python notebook](./proposals/jpy-usd-monthly-futures.ipynb) is a new market proposal (for JPY/USD futures) that we create on a Testnet Jam call.  


## Resources

1. [Vega docs](https://docs.testnet.vega.xyz/docs/trading-questions/#market-proposals-and-governance) provide an excellent overview of the proposal process and proposal inputs
1. [Early demo of creating a BTC/DAI futures market using APIs](https://www.youtube.com/watch?v=VIqouJx16Ak)
1. [Guide to risk and margins on Vega](https://www.youtube.com/watch?v=Coc3T3INq3k)

## Overview of this repo

1. [Data Tools](./data-tools): tools for downloading data to utilise in setting risk parameters and specifiying settlement data.

1. [Products](./products): definitions of the products available to create markets on Vega

1. [Proposals](./proposals): a list of community and example proposals

1. [Risk Tools](./risk-tools): notebooks and spreadsheets to help with setting the risk model and its parameters 

1. [Price Monitoring Tools](./price-monitoring-tools): notebooks and spreadsheets to help with setting the price monitoring circuit breaker parameters 





