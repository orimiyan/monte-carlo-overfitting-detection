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

## Target Metrics for Out-of-Sample Validation

This project will evaluate whether a strategy is ready for further use by measuring whether its risk and performance characteristics remain stable during the out-of-sample / forward-test period.

The purpose of these metrics is not only to determine whether the strategy remains profitable, but also to assess whether it continues to satisfy risk constraints when exposed to unseen data. If the forward-test Monte Carlo results are significantly weaker than the backtest Monte Carlo results, this may indicate that the strategy is overfitted or not robust enough for live deployment.

The key question is:

Can the strategy continue to pass the same risk rules and maintain a similar performance distribution outside the original backtest sample?

The main metrics targeted in this project include:

1. **Out-of-sample pass probability**  
   Measures the percentage of Monte Carlo paths that pass all defined risk rules during the forward-test period.  
   This answers whether the strategy still survives the required constraints on unseen data.

2. **Out-of-sample failure probability**  
   Measures how often simulated paths fail due to rule breaches such as maximum drawdown, daily loss limits, or account failure thresholds.  
   This answers how frequently the strategy becomes invalid under forward-test conditions.

3. **Maximum drawdown distribution**  
   Measures the range of maximum drawdowns across simulated paths, including median, mean, and tail drawdown levels.  
   This answers whether the strategy’s downside risk becomes materially worse out-of-sample.

4. **Daily loss breach probability**  
   Measures the probability of breaching daily loss limits across Monte Carlo paths.  
   This answers whether the strategy remains compatible with strict day-to-day risk limits.

5. **Risk of ruin**  
   Measures the percentage of simulated paths that hit a predefined failure or capital loss threshold.  
   This answers whether the strategy has an unacceptable probability of severe loss.

6. **Profit target achievement probability**  
   Measures how often the strategy reaches its target return before breaching risk limits.  
   This answers whether the strategy is not only surviving, but also producing enough return to justify use.

7. **Final return distribution**  
   Measures the spread of final simulated account outcomes across the forward-test Monte Carlo runs.  
   This answers whether the strategy’s return profile remains similar to the backtest distribution.

8. **Risk-adjusted return stability**  
   Measures whether performance remains stable after accounting for risk, using measures such as Sharpe ratio, return-to-drawdown ratio, or profit factor.  
   This answers whether returns are being achieved efficiently or through excessive risk.

9. **Trade-level stability**  
   Measures whether win rate, average win, average loss, payoff ratio, and profit factor remain consistent across backtest and forward-test periods.  
   This answers whether the strategy’s underlying edge is still present on unseen data.

10. **Loss clustering**  
    Measures the frequency and severity of consecutive losses or clustered drawdowns.  
    This answers whether the strategy is vulnerable to sequences of losses that could cause failure even if average returns appear acceptable.

11. **Backtest-to-forward-test degradation**  
    Measures how much key metrics deteriorate between the backtest Monte Carlo results and the forward-test Monte Carlo results.  
    This answers whether the strategy’s performance has weakened materially outside the original sample.

12. **Statistical comparison tests**  
    Compares the backtest and forward-test Monte Carlo distributions using statistical methods such as t-tests, non-parametric tests, confidence intervals, or distribution distance measures.  
    This answers whether differences between the two periods are likely to be meaningful rather than random variation.

Together, these metrics create a validation framework for deciding whether a strategy should progress toward live trading. A strategy should only be considered for further use if it can pass the relevant risk rules out-of-sample and maintain a risk profile that is broadly consistent with the backtest results.
