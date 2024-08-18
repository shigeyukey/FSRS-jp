# Introduction

Root Mean Square Error in Bins (RMSE (bins)) is a metric designed to measure how well FSRS and other spaced repetition algorithms can predict the probability of recall (R).

In our benchmark experiment, we discovered that the RMSE (bins) metric can sometimes be cheated. So, we modified the binning method to prevent algorithms from obtaining artificially low RMSE (bins).

## Old method of calculating RMSE (bins)

​1​​\) ​Group the predicted values of R into bins. For example, let's group some predictions between 0.8 and 0.9 in the bin 1:

    Bin 1 (predictions): [0.81, 0.82, 0.83, 0.84, 0.85, 0.86]

2\) For each bin, record the actual outcome of a review, either 1 or 0. Again = 0. Hard/Good/Easy = 1.

    Bin 1 (actual): [0, 1, 1, 1, 1, 1]

3\) Calculate the average of all predictions within a bin.

    Bin 1 average (predictions) = mean([0.81, 0.82, 0.83, 0.84, 0.85, 0.86]) = 0.835

4\) Calculate the average of all actual outcomes.

    Bin 1 average (actual) = mean([0, 1, 1, 1, 1, 1]) = 0.833

5\) Calculate the squared difference between the average of the predictions and the average of the actual review outcomes.

6\) Repeat steps 2-5 for each bin. The number of bins is arbitrary. The number of reviews in each bin is used for weighting.

7\) Calculate the final value using the following formula: 

$$RMSE(bins)=\sqrt\frac{\sum\limits\_{i=1}^{n} w\_{i} \cdot ({\overline{R}\_{i}}\_{\text{predicted}}-{\overline{R}\_{i}}\_{\text{measured}})^2}{\sum\limits_{i=1}^{n} w\_{i}}$$

where,
- $\overline{R}\_{\text{predicted}}$ is the average of predicted probabilities of recall within a given bin,
$\overline{R}\_{\text{measured}}$ is the average of actual review outcomes within a given bin,
- $w$ is the number of reviews within a given bin, and
- $n$ is the number of bins.


## How it could be cheated

If an algorithm bets a constant number on each review that is precisely equal to the average retention in the dataset, it will achieve an RMSE (bins) of 0. This is an extreme example, but even an algorithm that does not cheat so blatantly can still cheat subtly by reducing the variance of its predictions.

The calibration graph on the left is from an algorithm that doesn't cheat; the calibration graph on the right is from a cheating algorithm.

![311506217-948f32d0-a1f9-4b4e-b0be-0964105bb284](https://github.com/open-spaced-repetition/fsrs4anki/assets/83031600/338d8d81-9389-4d91-9cca-5b9b6551d548)


## New method of calculating RMSE (bins)

The main difference is the binning method. Instead of grouping algorithmic predictions and the review outcomes based on the predicted R, the new method groups them based on three features: the interval length, the number of reviews, and the number of lapses (instances when the user pressed Again). The values of these three features are rounded using formulas designed for this metric. These formulas are arbitrary and can be adjusted. Then, reviews are grouped into bins based on the rounded values. For example, reviews with interval length = 100, n(reviews) = 7, n(lapses) = 0 and reviews with interval length = 101, n(reviews) = 7, n(lapses) = 0 would both fall into the same bin. Previously, the bin into which the value of predicted R would fall was determined by its distribution. Binning is now independent of the distribution of predicted R.

Below is the pseudocode for rounding functions:

```py
delta_t = round(2.48 * power(2.57, floor(log(x)/log(2.57))), 2)

n_reviews = round(1.52 * power(1.58, floor(log(x)/log(1.58))), 0)

n_lapses = round(1.4 * power(1.48, floor(log(x)/log(1.48))), 0) if x != 0 else 0
```
