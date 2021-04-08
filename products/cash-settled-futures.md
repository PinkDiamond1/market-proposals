# Built-in Product: Cash Settled Futures (CSF)

This built-in product provides "direct" futures (i.e. opposite of inverse futures) that are cash-settled, i.e. they are margined and settled in a single asset.

Background reading: https://www.cmegroup.com/education/courses/introduction-to-futures.html

Futures are a simple "delta one" product and the first product supported by Vega.


## 1. Product parameters

1. `trading_termination_trigger (Data Source)`: triggers the market to move to `trading terminated` status ahead of settlement at expiry (required to ensure no trading can occur after the settlement result may be known by participants). (This would usally be a date/time based trigger but may also use an oracle.)
1. `settlement_data (Data Source: number)`: this data is used by the product to calculate the final settlement cashflows. The receipt of this data triggers this calculation and therefore also moving the product to the `settled` status.
1. `settlement_asset (Settlement Asset)`: this is used to specify the single asset that an instrument using this product settles in.


## 2. Settlement assets

1. Returns `[cash_settled_future.settlement_asset]`


## 3. Valuation function

```javascript
// Futures are quoted in directly terms of price 
cash_settled_future.value(quote) {
	return quote
}
```

## 4. Lifecycle triggers

### 4.1 Mark to Market Settlement

```
settlement_amount( party ) =  party.prev_open_volume * (cash_settled_future.value(current_price) - cash_settled_future.value(prev_mark_price)) + SUM(from i=1 to new_trades.length)( new_trade(i).volume(party) * (cash_settled_future.value(current_price) - new_trade(i).price ) )
```


### 4.2 Termination of trading

```javascript
cash_settled_future.trading_termination_trigger(event) {
	setMarketStatus(TRADING_TERMINATED)
}
```


### 4.3 Final settlement ("expiry")

```javascript
cash_settled_future.settlement_data(event) {

	// Suspend the market if Vega receives settlement data before trading termination (this would require investigation and governance action)
	if market.status != TRADING_TERMINATED {
		setMarketStatus(SUSPENDED)
		return
	}

	final_cashflow = cash_settled_future.value(event.data) - cash_settled_future.value(market.mark_price)) 
	settle(cash_settled_future.settlement_asset, final_cashflow)
	setMarkPrice(event.data)
	setMarketStatus(SETTLED)
}
```