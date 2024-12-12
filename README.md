# Casino Cheating Detection Project

## Overview
The goal of this project is to develop a system for detecting cheating players in a casino based on the number of wins during 100 games. Using hypothesis testing, the system establishes a decision rule that balances the probability of detecting cheaters and minimizing false accusations of honest players.

## Project Details

### Problem Description
The casino requires a decision rule that:
- Detects cheating players with a probability of 80% (as close as possible to 80%, but not below).
- Ensures fair players are flagged as cheaters only occasionally.

Cheating players will be identified, removed from the casino, and stored in a database. The casino’s image processing system will prevent flagged players from re-entering.

### Constraints and Trade-offs
- Each player plays 100 independent games.
- The decision rule uses hypothesis testing to classify players as fair or cheating.
- Players’ probabilities of winning:
  - Normal players: 1/37
  - Cheaters: 1/20

### Hypothesis Testing
1. **Distribution:**
   - Normal players: Binomial distribution \(X \sim Binom(100, \frac{1}{37})\).
   - Cheaters: Binomial distribution \(X \sim Binom(100, \frac{1}{20})\).
2. **Null Hypothesis (H0):** The player is fair.
3. **Alternative Hypothesis (H1):** The player is cheating.
4. **Power of the Test:** The 80% requirement corresponds to the power of the test, which is \(1 - \beta\) where \(\beta\) is the probability of a Type II error.
5. **Type I Error (\(\alpha\)):** The probability of incorrectly flagging an honest player as a cheater.

### Decision Rule
- Likelihood ratios are calculated between the null and alternative hypotheses.
- A power test determines the threshold for detecting cheaters.
- The system adjusts the decision rule threshold to balance Type I and Type II errors.

## Implementation
The project was implemented in R, leveraging statistical and plotting libraries. The key components include:

1. **Hypothesis Calculation:**
   ```r
   hypotesis <- function(n, prob){
     k <- seq(1, n)
     return(dbinom(k, n, prob))
   }
   ```

2. **Likelihood Ratio Calculation:**
   ```r
   likelihoodRatio <- function(H0, H1){
     likelihoodRatio <- H0 / H1
     plot(seq(1, length(likelihoodRatio)), likelihoodRatio, type = "p", col = 4)
     lines(seq(1, length(likelihoodRatio)), likelihoodRatio, col = 2)
     return(likelihoodRatio)
   }
   ```

3. **Power Test and Rule Adjustment:**
   ```r
   rule <- function(prob, maxC, likelihoodRatio, H1){
     c <- seq(0, maxC, by=0.01)
     for (i in 1:length(c)) {
       if (powerTest(c[i], likelihoodRatio, H1) > prob) {
         return(c[i])
       }
     }
   }
   ```

4. **Error Type Calculations:**
   - Type I Error (\(\alpha\))
   - Type II Error (\(\beta\))

### Outputs
- Decision Rule: \(X \geq\) threshold value.
- Probability of Type I Error: \(\alpha\).
- Adjusted decision rules for various error trade-offs.

## Adjustments
After initial testing, the decision rule was adjusted to reduce Type I errors by lowering the probability of detecting cheaters. This adjustment improved fairness for honest players but decreased the detection rate for cheaters.

## Conclusion
The system provides a flexible decision rule to detect cheaters while maintaining control over error rates. Adjustments to thresholds allow the casino to balance the trade-offs between Type I and Type II errors effectively.

