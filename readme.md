# Next Order Please!

Trading's order management system (Saugatuck River) maintains a central limit order book. The system (which only keeps track of one single stock) sequentially processes orders containing the following information: quantity of stock, order price, and whether the order is a buy (bid) or a sell (ask). The system then clears orders by matching possible buyers to sellers.

[https://www.wikiwand.com/en/Central_limit_order_book](https://www.wikiwand.com/en/Central_limit_order_book)

For example, given data in the format `<BID/ASK>,<QUANTITY>,<PRICE>`

```
ASK,7,16062
ASK,12,16037
BID,5,16037
BID,10,16009
```

You should parse the orders one by one and clear them after each transaction.

The first order is an `ASK` order for `7` stocks at price `16062` cents. That means the person who submitted this order wants to sell 7 stocks at \$160.62. Since there are no `BID` (buy) orders on the system currently, nothing happens. This results in the following state for the order book:

```
ASK,7,16062
```

The second order is another `ASK` order for `12` stocks at price `16037` cents. This means the person who submitted this order wants to sell 12 stocks at \$160.37. Again, sine there are no `BID` (buy) orders on the system currently, nothing happens. However, this does move the best `ASK` price (the lowest price someone is willing to sell the stock) to \$160.37. The state of the order book is now:

```
ASK,7,16062
ASK,12,16037
```

The third order is a `BID` order for `5` stocks at prie `16037`. This means the person who submitted this order wants to buy 5 stocks at \$160.37. This creates a transaction that depletes (partially) the `ASK` order at `16037`. Ths `16037` ask order gets prioritized because the seller is willing to sell for a lower price.

Hence the state of the order book goes from:

```
ASK,7,16062
ASK,12,16037
BID,5,16037
```

to

```
ASK,7,16062
ASK,7,16037
```

Then, a fourth order gets submitted. It is a `BID` order for `10` stocks at `16009`. This depletes the `16037` ask order, then partially depletes the `16062` order.

Hence the order book goes from:

```
ASK,7,16062
ASK,7,16037
BID,10,16009
```

to:

```
ASK,4,16062
```

One day, the trading system crashed. You're asked to reconstruct the state of the order book. The day's transactions are in `output.csv`.

Given `output.csv`. Each row represents a single order submitted to a limit order book in the format `direction,quantity,price`. You're allowed to assume that price is in cents and that the minimum increment is one cent.

Find the final state of the order book in the following format `"%d%d%d%d", bestBid.quantity, bestBid.price, bestAsk.quantity, bestAsk.price`.
