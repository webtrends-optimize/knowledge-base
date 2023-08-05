# Taguchi/Fisher Method for Design of Experiments (DoE) in MVTs

## Introduction

### Overview of Taguchi/Fisher Method for Design of Experiments (DoE)
The Taguchi/Fisher Method for Design of Experiments (DoE) is a powerful statistical technique that enables efficient and effective experimentation in the context of Multi-Variate Tests (MVTs).

The Taguchi/Fisher Method employs a combination of orthogonal arrays, signal-to-noise ratio analysis, and robust design principles to achieve reliable and meaningful results. By carefully selecting factors (categories of change) and their levels (variations), conducting controlled experiments, and analyzing the data, this method enables practitioners to gain valuable insights and make data-driven decisions.

### Relevance of the Taguci/Fisher Method on Multi-Variate Testing
The Taguchi/Fisher Method offers a structured and efficient approach to conducting Multi-Variate Tests. Unlike traditional A/B testing that focuses on isolated factors, this method allows for the simultaneous evaluation of multiple factors and their interactions, providing a comprehensive understanding of the system under test. By leveraging the statistical rigor and optimization principles of the Taguchi/Fisher Method, software development teams can more easily run MVTs.

### The challenge with traditional MVTs
MVTs come in two flavours – Full Factorial and Fractional Factorial. Consider the following experiment:

- Factor 1: Button Colour
    - Level 1: Red
    - Level 2: Blue
    - Level 3: Green

- Factor 2: Button Size
    - Level 1: Default
    - Level 2: Thinner
    - Level 3: Larger

- Factor 3: Iconography
    - Level 1: None
    - Level 2: Padlock
    - Level 3: Bag

Full Factorial runs every possible combination of Factor-Levels. In this example, that would be 3x3x3 = 27 experiments.

The DoE approach, as part of a Fractional Factorial approach, allows us to test a subset of these combinations and make predictions accordingly.

Using this approach, you will find the need to only run 9 combinations instead of 27, which is far more palettable.

## Understanding Orthagonal Arrays

Orthogonal arrays are a fundamental component of the Taguchi/Fisher Method for Design of Experiments.

They serve as a systematic and efficient way to organize and conduct experiments by reducing the number of required trials. Orthogonal arrays are structured matrices that ensure an equal and balanced representation of the different levels of factors being tested.

In the above example of a 3x3x3 array, the orthagonal array ensures each Level has the same amount of exposure as any other. I.e. of the 9 experiments we run, every Level is shown the same amount of times.

With Orthagonal Arrays, each column in the matrix represents a factor, and each cell within the matrix represents a level of that factor. The arrangement of the levels within the matrix is carefully designed to ensure that every possible combination of factor levels is tested in a balanced and orthogonal manner. This design allows for the identification of the main effects and interaction effects of factors while minimizing the number of experiments needed, saving time and resources. By using orthogonal arrays, practitioners can efficiently explore a large parameter space, gain insights into factor contributions, and optimize the system’s performance.

## The relevance of the Signal-Noise Ratio

The Signal-to-Noise Ratio (SNR) is a critical concept in the Taguchi/Fisher Method for Design of Experiments, as it provides a quantitative measure to assess the performance and variability of the system.

SNR represents the ratio of the signal, which refers to the desired or optimal response, to the noise, which represents the random variations and undesirable influences on the response. By evaluating the SNR in Webtrends Optimize, we are able to determine the extent to which the factors being tested contribute to the variability in the response variable. In our reporting, this surfaces as Factor Influence, reprsented as a simple bar graph depicting the influence of each Factor on overall Conversion Rate.

The objective is to maximize the SNR, indicating a higher signal-to-noise ratio and a more desirable system performance. By focusing on maximizing the SNR, the Taguchi/Fisher Method allows for the identification of the factor settings that lead to improved performance and reduced sensitivity to noise factors. This approach helps ensure that the experiments conducted are robust and effective in optimizing the system’s response, resulting in enhanced product quality and performance.

## To summarise, the benefits of the Taguci / Fisher method

The Taguchi/Fisher Method for Design of Experiments offers several key benefits when applied to Multi-Variate Testing.

Firstly, it enables efficient allocation of resources by reducing the number of experiments required while still providing comprehensive insights. This overcomes what is quite often considered to be the most challenging aspect of running MVTs – traffic.

Secondly, it allows for the simultaneous evaluation of multiple factors, capturing the interactions and dependencies between them. In comparison to sequential A/B testing which is the norm for most programmes, this offers a much more robust method of understanding cause and effect.

Overall, these benefits make the Taguchi/Fisher Method a valuable tool for optimizing the experimentation process and improving software development outcomes.