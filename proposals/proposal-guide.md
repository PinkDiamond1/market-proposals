# Guide: Proposals on Testnet

There are two key aspects to proposing a successful market on Vega.

1. Carefully defining the market
1. Articulating the market design choices to the governance community, in order to have the proposal voted in

This notebook covers off both aspects by providing a _Guide_ for each component of the proposal to help with setting the proposal parameters. It then provides an example of how to articulate those choices.


# Market Description and Metadata

The market description should clearly outline the market you are intending to propose and make the case for its usefulness. Think of this as speaking to the governance and trading communities to:

1. Make the case for voting in the market
1. Make the case for trading the market

You may want to reference the current markets on the testnet and explain why this one adds value / is sufficiently different to what else exists. If you simply want to improve a market by proposing an adjustment to a parameter, the settlement asset, data source (etc.) you can propose an amendment to that market (not implemented yet) rather than creating a new market. This is likely to be valued by the governance community, rather than proposing a new market that may fragment liquidity.

# Assets

_Guide to specificing settlement asset_

The settlement asset is what the market will be both margined in and settled in. 

Important notes:
- For [direct cash settled futures](../products/cash-settled-futures.md), this needs to be an asset that represents the quote (e.g. tDAI for USD markets)
- The asset needs to have already been approved on the VEGA bridge and will therefore have both a VEGA asset id and a host chain contractAddress (e.g. Ethereum ERC20 contract address)

Get a [list of all assets on Vega Testnet](https://lb.testnet.vega.xyz/assets)


# Default trading mode

The default trading mode specifies how the market will "normally trade". Currently on Vega testnet, only "continuous trading" is available as a default mode. This trading mode requires a `tick size` to be specified. In this section you should also specify aspects of the market relevant to how it trades, such as `quote name`.

# Risk model and parameters
The risk model and its parameters is one of the most important aspects of the proposal as these are used to *calculate margins (and hence set leverage).* The [Vega docs](https://docs.testnet.vega.xyz/docs/trading-questions/#risk-parameters) describe the inputs and [this video](https://www.youtube.com/watch?v=Coc3T3INq3k) by Dr Siska explains margins on Vega.

The challenge is to set the parameters, particularly those that have the greatest effect, at as close to "real" values as possible so that the market is optimally capital efficient (low margins), whilst also safe for trading (i.e. margins are not being blown through too quickly with traders getting rekt).

## Risk model:
The only available risk model at the moment is the Lognormal Expected Shortfall model. 


## Model independent parameters

Parameters with a mild effect on the margins:
- risk free rate: can set to 0
- mu: can set to 0
- risk aversion lambda: suggest setting to 0.001 

Parameters with a BIG effect on margins:
- tau: suggest setting to 1 hour (0.000114077116130504 years). A shorter time horizon leads to lower margins.
- volatility (Sigma): this represents the expected (annualised) volatility over the time horizon of the market. 

## Methods for deciding the volatility parameters

1. USING HISTORICAL DATA: If you have access to historical spot market data for your market, you can calculate what the actual volatility has been and then infer a forward looking volatility estimate accordingly. Decisions you need to make are:
  - How much historical data is needed?
  - Should you weight more recent volatility outcomes as better indication of the expected forward volatility? (see EWMA, GARCH methods)


1. USING IMPLIED VOLATILITY DATA: If there is a sufficiently liquid options market then you can use the "at the money" implied volatility of an option with a similar expiry to your futures.


1. WHEN NEITHER HISTORICAL SPOT DATA NOR IMPLIED VOLATILITY DATA EXISTS: For futures markets on a new data source (e.g. futures markets corresponding to launch of a new token) then you need to make the case for a volatility level that you choose. For example, you may:
  - Use a similar market to estimate (e.g. similar asset class)
  - Use a similar staged market to estimate (e.g. other new token launches)


Generally speaking, overestimating rather than underestimating volatility is likely to be preferable to the governance / voting community, as higher volatility outcomes than projected will lead to markets that don't function well, whereas lower volatiilty markets will just be less capital efficient.


# Governance parameters

The governance parameters are subject to network minimums. See the full list of Network parameters [here](https://lb.testnet.vega.xyz/network/parameters).

The two that we need to set on the market proposal are:

1. closingDateTime - this should be set to a time in the future when the voting for the proposal will end.
1. enactmentDateTime - this should be set to a time in the future when the market would launch out of opening auction and into the default trading mode.




