# Monte Carlo Overfitting Detection

This project extends the Monte Carlo backtest robustness framework by comparing simulated strategy performance across both backtest and forward-test periods.

The purpose of this project is to evaluate whether a trading strategy that appears robust during historical testing continues to behave consistently when exposed to unseen market data.

A strategy should not be considered ready for live trading only because it performs well in a backtest. It should also demonstrate that its risk and return characteristics remain stable during forward testing. This project treats forward testing as an out-of-sample validation stage before committing capital.

## Project Objective

The objective is to identify potential overfitting by comparing Monte Carlo results from:

1. The original backtest period
2. The forward-test / out-of-sample period

If the strategy performs well during the backtest but deteriorates significantly during forward testing, this may suggest that the strategy was fitted too closely to historical market conditions.

The goal is not only to check whether the strategy remains profitable, but whether it continues to pass risk constraints under unseen data.

## Why This Matters

A strategy can appear profitable in a backtest while still being fragile. This can happen when the strategy depends too heavily on a favourable historical sequence of trades, specific market conditions, or parameter choices that do not generalise.

Monte Carlo simulation helps test this by generating many alternative trade sequences from both the backtest and forward-test results.

By comparing the distributions from both periods, this framework can assess whether the strategy's performance remains statistically consistent or whether it shows signs of overfitting.

## Methodology

The framework applies Monte Carlo simulation separately to the backtest and forward-test trade results.

For each period, the simulation generates thousands of possible equity paths by resampling trade-level returns. Each simulated path is then evaluated against predefined trading and risk rules.

The results from the two periods are compared to identify whether the forward-test performance remains within an acceptable range of the backtest performance.

## Risk Rules Tested

Each simulated path is evaluated against rules such as:

- Maximum total drawdown
- Maximum daily loss
- Profit target achievement
- Account failure threshold
- Minimum trading period requirements
- Risk-of-ruin conditions

A strategy must continue to satisfy these constraints during the forward-test period before being considered suitable for live or funded trading conditions.

## Overfitting Detection

Overfitting is assessed by comparing whether the statistical properties of the strategy remain stable between the backtest and forward-test periods.

Key comparisons include:

- Backtest pass probability vs forward-test pass probability
- Backtest failure probability vs forward-test failure probability
- Backtest drawdown distribution vs forward-test drawdown distribution
- Backtest return distribution vs forward-test return distribution
- Risk-of-ruin estimates across both periods
- Profit target achievement probability
- Frequency of drawdown rule breaches
- Stability of win rate, average win, average loss, and payoff ratio

A strategy may show signs of overfitting if the forward-test Monte Carlo results are materially weaker than the backtest Monte Carlo results.

For example, warning signs may include:

- A sharp drop in pass probability
- A higher probability of breaching drawdown limits
- Larger or more frequent drawdowns
- Lower expected return across simulated paths
- Increased risk of ruin
- Reduced payoff ratio
- A forward-test distribution that is significantly worse than the backtest distribution

## Statistical Comparison

This project will also explore statistical methods for comparing the backtest and forward-test results.

Possible statistical tests and measures include:

- Difference in mean simulated returns
- Difference in maximum drawdown distributions
- Difference in failure probabilities
- T-tests for comparing average simulated returns
- Non-parametric tests for comparing distribution differences
- Confidence intervals around key Monte Carlo metrics
- Distribution distance measures between backtest and forward-test outcomes

The aim is to move beyond visual comparison and use statistical evidence to assess whether the forward-test results are meaningfully different from the backtest results.

If the forward-test results are statistically weaker, this may indicate that the original backtest was overfitted or that the strategy is not robust to unseen market conditions.

## Decision Framework

The output of this project is intended to support a decision on whether the strategy should progress toward live trading.

The strategy may be considered for further development if:

- It passes the same risk rules in the forward-test period
- The forward-test Monte Carlo distribution remains close to the backtest distribution
- Drawdown behaviour remains stable
- Risk-of-ruin remains acceptably low
- Performance does not rely on one favourable sequence of trades

The strategy should be rejected, revised, or tested further if:

- Forward-test failure probability increases significantly
- Drawdowns become materially larger
- Profit target achievement probability falls sharply
- Statistical tests suggest a significant deterioration
- The strategy fails to generalise to unseen data

## Project Direction

This repository represents the next stage before live trading.

The first stage is to test whether a strategy is robust within the backtest sample. The second stage is to test whether that robustness continues during forward testing. This project focuses on the second stage.

The long-term objective is to build a validation pipeline that follows this structure:

```text
Backtest results
        ↓
Monte Carlo robustness testing
        ↓
Forward-test Monte Carlo comparison
        ↓
Overfitting detection
        ↓
Decision on live deployment
