# tz-share

A personal Chrome extension that sizes day trades on the TradeZero web app by risk: given a stop price, it computes how many shares keep the loss at the stop within a fixed budget, and loads that number into the order ticket.

## Account

**Equity**:
Total account value: cash plus open positions marked to market. TradeZero labels it "Account Value".
_Avoid_: Account, balance, account value

**Buying power**:
The dollar amount TradeZero says can be committed to new positions, including any margin multiplier.
_Avoid_: BP, margin

## Sizing

**Side**:
Long or short, chosen explicitly by the trader; never inferred from the stop.
_Avoid_: Direction

**Risk budget**:
The dollars the trader accepts losing if the stop is hit; set either as a percent of Equity or as a fixed dollar amount (the **Risk mode**).
_Avoid_: R, risk, R-value (when meaning dollars)

**Stop price**:
The price at which the trader will exit a losing trade, typed manually, already including any slippage allowance the trader wants.
_Avoid_: Stop, stoploss, SL

**Entry price**:
The price the trade is expected to fill at: the Order ticket's limit price if one is set, otherwise the ask for a long or the bid for a short.
_Avoid_: Current price, last

**Stop distance**:
The per-share loss if stopped out: Entry price minus Stop price for a long, Stop price minus Entry price for a short. Must be positive for a valid sizing.

**Raw size**:
Risk budget divided by Stop distance, before any cap or rounding.

**Position cap**:
The maximum dollars a single sizing may commit: the lesser of Buying power and the trader's configured max-position amount (default: Equity).
_Avoid_: Clamp, limit

**Lot size**:
The share increment sizes are floored to (default 1).

**Size**:
The final share count: Raw size limited by Position cap / Entry price, floored to the Lot size.
_Avoid_: Shares, qty, quantity (when meaning the computed value)

## TradeZero page

**Order ticket**:
The TradeZero UI where symbol, side, order type, limit price and quantity are entered before an order is sent.
_Avoid_: Order entry, order form

**Quantity field**:
The Order ticket input holding the share count; the only thing on the page the extension writes to.

## Behaviour

**Auto mode**:
The Size is written into the Quantity field whenever any input to it changes.
_Avoid_: Live mode

**Manual mode**:
The Size is previewed continuously but only written into the Quantity field on explicit request.

**Freeze**:
A state in which Auto mode stops writing to the Quantity field, entered while the pointer is over the order-sending controls.
