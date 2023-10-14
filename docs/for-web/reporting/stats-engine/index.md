# Stats Engine

*This document is under construction*

## Introduction

In Webtrends Optimize, our ethos is easily replicable statistics. Our core calculations for most of the metrics you see are those which are used in popular calculators like [https://cxl.com/ab-test-calculator/](https://cxl.com/ab-test-calculator/){target=_blank}.

## Types of metrics 

### Binomial Metrics

The important part of the word binomial is "bi", as in 2. Binomial data in Experimentation is therefore data that has 2 possible outcomes. For us, this is that an event did, or did not happen. 

When considering our traditional goals - purchases, page loads, clicks, etc. - binomial calculations consider whether or not these happened. Statistics on this data consider the likelihood of these actions.

### Non-binomial metrics

Unlike event-level data, where we're capturing a tally of how often things happened, non-binomial data is a numeric data collection. 

Examples include Revenue, Units Per Transaction, etc. Such data is as useful as binomial data - knowing you've made more sales is great, but money earned is as important if not more.

## Statistics for AB/ABn Tests and Targets

AB/ABn Tests and Targets have a single factor, or controlled variable of change. You may have several variations as part of these tests, i.e. an AB/n as opposed to an AB test.

- An AB test for button colour could test Blue vs. Red
- An ABn test for button colour could test Blue vs. Red vs. Black

Either way, there's one controlled variable (colour).

### Binomial calculations for AB/ABn tests and Targets

We employ 2-tailed T-tests for these metrics and these experience types.

Read about it here: [T-tests in Webtrends Optimize](./t-tests)


### Non binomial calcluations for AB/ABn tests and Targets

We employ Wincoxon Rank-Sum / Mann-Whitney U-Tests.  

Road more here: Link TBC

## Statistics for Multi-Variate Tests

Multi-Variate Tests (MVTs) have multiple variables that we're observing. In Webtrends Optimize, we refer to the variables as Factors, and the variations plus control are collectively referred to as Levels.

Taking the previous example of a button colour AB or ABn test, an MVT would consider other Factors or types of change.

For example:

- **Factor 1: Button Colour**
    - Factor 1 Level 1: Control (blue)
    - Factor 1 Level 2: Red
    - Factor 1 Level 3: Green

- **Factor 2: Button Size**
    - Factor 2 Level 1: Control (medium)
    - Factor 2 Level 2: Bigger
    - Factor 2 Level 3: Biggest

- **Factor 3: Button Icon**
    - Factor 3 Level 1: Control (no icon)
    - Factor 3 Level 2: Bag Icon
    - Factor 3 Level 3: Padlock Icon

The **Test Array** is what we refer to as the count of variations per factor. In the above example, the test array would be 3x3x3.

The calculations for MVTs differ strongly from what you'll see from a single-factor experiment.

Instead of making 1 vs 1 comparisons and looking for a single winner, MVTs look to find a combination of Factor-Levels that work the best togehter. And as such, different calculations are required.

### Design of experiments for Fractional Factorial MVTs

We employ Taguci/Fisher Design of Experiments. 

Read more here: [Taguci-Fisher Design of Experiments for MVTs](./taguchi-fisher-design-of-experiments-mvt)

### Predicted Optimal 

*Detail tbc*

Road more here: Link TBC

### Factor Influence 

*Detail tbc*

Road more here: Link TBC

### Non-binomial calculations for MVTs

*Detail tbc*

Road more here: Link TBC

## To summarise

*Detail tbc*