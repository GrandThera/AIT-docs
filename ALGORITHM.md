# Algorithm Notes

AIT Simple_RegimeDetection combines unsupervised regime discovery with a
Gaussian Markov Switching model. K-Means is used to choose or initialize the
number of states, while the final probabilities come from a time-aware hidden
Markov model.

## Input

The analyzer expects a sequence of positive prices ordered in time:

```python
prices = [100.0, 101.2, 102.4, 101.9]
```

No external market data format is required. This keeps the first release easy to
test and easy to embed in notebooks, services, or backtesting pipelines.

## Feature Engineering

For each rolling window, the library computes:

- `mean_return`: average log return in the window.
- `volatility`: sample standard deviation of log returns.
- `trend_strength`: linear price slope normalized by average price.
- `max_drawdown`: worst peak-to-trough loss in the window.
- `downside_volatility`: volatility contributed by negative returns.
- `skewness`: asymmetry of returns.
- `kurtosis`: excess tail weight of returns.
- `autocorrelation`: lag-one return persistence.
- `range_position`: final price position inside the rolling high-low range.
- `ewma_volatility`: exponentially weighted realized volatility.

These features are intentionally compact. They capture direction, risk, and
trend persistence without depending on a technical-analysis package.

## Scaling

The model standardizes features before clustering:

```text
z = (x - mean) / standard_deviation
```

This prevents volatility or trend magnitude from dominating the clustering
because of unit scale alone.

## K-Means Initialization

The repository includes a deterministic K-Means implementation:

1. Sort observations by aggregate feature value.
2. Initialize centroids across that sorted distribution.
3. Assign each observation to the closest centroid.
4. Recompute centroids.
5. Repeat until centroid movement is below tolerance or the iteration limit is
   reached.

The implementation stores final assignments and inertia for inspection. These
assignments initialize the Markov model's means, variances, initial state
probabilities, and transition matrix.

## Automatic Regime Count

When `n_regimes="auto"`, the analyzer fits K-Means for each value in
`auto_range` and selects the best count using silhouette score:

```text
s = (b - a) / max(a, b)
```

Where:

- `a` is the average distance to observations in the same cluster.
- `b` is the average distance to the nearest different cluster.

The selected K becomes the number of hidden states in the Markov Switching
model. The score by regime count is exposed in `result.selection_scores`.

The analyzer can also choose the state count by fitting the full Markov model
for each candidate K and minimizing AIC, BIC, or ICL. BIC is the default because
it usually discourages unnecessary extra regimes more aggressively than AIC.

## Markov Switching Model

The final model is a hidden Markov model with diagonal Gaussian emissions. Each
regime has:

- an initial probability;
- a row in the transition matrix;
- a Gaussian mean vector;
- a Gaussian variance vector.

The transition matrix captures regime persistence:

```text
P(S_t = j | S_{t-1} = i)
```

This is the main reason the model is more useful than raw clustering for market
regimes: it allows the previous regime to influence the next regime estimate.

## Baum-Welch Estimation

After K-Means initialization, the model runs Baum-Welch:

1. Forward pass estimates filtered state probabilities.
2. Backward pass incorporates future observations.
3. The expectation step computes state and transition responsibilities.
4. The maximization step updates initial probabilities, transition
   probabilities, means, and variances.
5. Iteration stops when log-likelihood improvement is below tolerance.

## Probability Output

For each observation, the analyzer returns smoothed state probabilities from the
forward-backward pass. These probabilities use both the observation at that time
and the temporal structure learned through the transition matrix.

The result also exposes filtered probabilities, which only use information up
to time `t`. Filtered probabilities are more appropriate for live monitoring,
while smoothed probabilities are more appropriate for historical diagnostics.

## Viterbi Path and Uncertainty

The Viterbi algorithm returns the single most likely sequence of hidden regimes.
The dashboard uses probabilities for confidence and the Viterbi path for stable
diagnostics.

Each row also includes a normalized entropy score called `uncertainty`. Low
uncertainty means one regime dominates; high uncertainty means the model is
less decisive.

## Labels

For three regimes, clusters are labeled by directional strength:

- lowest return plus trend score: `bear`
- middle score: `sideways`
- highest score: `bull`

For any other number of regimes, labels are returned as `regime_0`,
`regime_1`, and so on.

## Roadmap

The next technical step is to add model diagnostics, information criteria, and
out-of-sample scoring for comparing different feature sets and state counts.
