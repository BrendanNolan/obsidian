# Derivatives

A **derivative** is a financial instrument whose value is derived from the value of some underlying
asset, such as a stock, bond, commodity, or index. An example of a derivative is an employee stock
option - this is not itself an asset but its value is derived from the value of the underlying
asset, namely company stocks.

## Types of Derivatives

- A **forward** is an agreement to buy or sell an asset at a specified future date for a specified
  price.
- A **future** is similar to a forward but is standardised and traded on an exchange.
- An **option** gives the holder the right, but not the obligation, to buy (call) or sell (put) the
  underlying asset at a specified strike price, either on a specific date (European) or at any time
  up to expiry (American).

### Symmetry of Payoffs

Plotting a derivative's profit/loss at expiry against the underlying's price gives a curve whose
shape reflects the contract's structure:

- **Forwards and futures** have **linear, symmetric** payoffs. Both parties are obligated to
  transact at the agreed price, so the P&L is just the difference between the spot and the agreed
  price — a straight line, with gains and losses behaving identically in magnitude.
- **Options** have **asymmetric, kinked** payoffs. The holder only exercises when favourable, which
  introduces a `max(., 0)` into the payoff and produces a bend (kink) at the strike: flat on the
  unfavourable side, linear on the favourable side.

The general principle: **obligation produces linear payoffs; optionality produces kinked payoffs.**

# Long and Short Positions

A **long** position is one where you gain from a rise in the value of the underlying asset.
A **short** position is one where you gain from a fall in the value of the underlying asset.

## Examples of Short Positions

| Instrument     | Upfront cost/credit | Loss if you're wrong |
| -------------- | ------------------- | -------------------- |
| Buy put        | Pay premium         | Capped at premium    |
| Sell call      | Receive premium     | Unlimited            |
| Sell futures   | None (margin only)  | Unlimited            |

## Examples of Long Positions

| Instrument     | Upfront cost/credit | Loss if you're wrong       |
| -------------- | ------------------- | -------------------------- |
| Buy call       | Pay premium         | Capped at premium          |
| Sell put       | Receive premium     | Capped at strike − premium |
| Buy futures    | None (margin only)  | Capped at contract value   |


