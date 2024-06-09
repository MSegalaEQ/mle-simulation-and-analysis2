# Maximum Likelihood Estimator: Variance

Referring back to the [previous post](https://github.com/MSegalaEQ/mle-simulation-and-analysis), it is established that the maximum likelihood estimation (MLE) approach provides estimations centered around the original parameter value, but how good indeed is this approach is not obvious. Due to the fact that the access to a large number of measurements from the same dataset is unrealistic, in practice, the parameter estimation process is often unique an with limited data.

![MLE expected value approximation with simulated data](https://github.com/MSegalaEQ/mle-simulation-and-analysis/raw/main/multiple-sample-result.png)

Examining the example in this figure, despite a higher likelihood for $`θ_{MLE}`$ to be 5, it may be near 2, 8, or any other value. In other words, with only an unique estimation, accurate result is not guaranteed. This is primarily because likelihood is not about finding the true parameter values - values that generate the samples of observed data - but rather which parameter values are more consistent to represent the given sample. In a bad day, a sample might lead to a poor approximation of the parameters.

In estimation theory and statistics, the Cramér–Rao bound is a theoretical result found in literature that establishes the lower limit of variance for unbiased estimators, given a fixed sample size. According to this theorem, this limit is equal to the inverse of the Fisher Information ($I$):

$$
Var(θ) = \frac{1}{I(θ)}
$$

A simplified demonstration is shown ahead.

## Theoretical Proof

Between two random variables $W$ and $V$, the expression
$$|\text{Corr}(W, V)| \leq 1$$
in addition to 
$$\text{Corr}(W, V) = \frac{\text{Cov}(W, V)}{\sqrt{\text{Var}(W) \cdot \text{Var}(V)}}$$
provides the result:

$$
\text{Cov}^2(W, V) \leq \text{Var}(W) \cdot \text{Var}(V)
$$

Therefore:

$$
\text{Var}(W) \geq \frac{\text{Cov}^2(W, V)}{\text{Var}(V)}
$$

Using the definition of covariance:

$$
\text{Cov}(W, V) = \mathbb{E}(W.V) - \mathbb{E}(W)\mathbb{E}(V)
$$

For a given parameter $W = \hat{\theta}$ and the score function $V = {\partial \ell}/{\partial \theta}$:

$$
\text{Var}\left( \frac{\partial \ell}{\partial \theta} \right) = \mathbb{E}\left[ \left( \frac{\partial \ell}{\partial \theta} \right)^2 \right] - \left( \mathbb{E}\left( \frac{\partial \ell}{\partial \theta} \right) \right)^2
$$

The expected value for the score function is equal zero for any statistical model, so $\left[ \mathbb{E}\left( {\partial \ell}/{\partial \theta} \right) \right]^2 = 0$, hence:

$$
\text{Var}\left( \frac{\partial \ell}{\partial \theta} \right) = \mathbb{E}\left[ \left( \frac{\partial \ell}{\partial \theta} \right)^2 \right] = I(\theta)
$$

And

$$
\text{Cov}\left(\hat{\theta}, \frac{\partial \ell}{\partial \theta}\right) = \mathbb{E}\left(\hat{\theta}. \frac{\partial \ell}{\partial \theta}\right) - \mathbb{E}(\hat{\theta}) \mathbb{E}\left(\frac{\partial \ell}{\partial \theta}\right)
$$

$$
\text{Cov}\left(\hat{\theta}, \frac{\partial \ell}{\partial \theta}\right) = \mathbb{E}\left(\hat{\theta} \frac{\partial \ell}{\partial \theta}\right)
$$

Finally:

$$
Var(\hat{\theta}) \geq \frac{{Cov}^2(\hat{\theta}, \frac{\partial \ell}{\partial \theta})}{I(\theta)} = \frac{\mathbb{E}\left(\hat{\theta} \frac{\partial \ell}{\partial \theta}\right)^2}{I(\theta)}
$$

As $\theta = E(\hat{\theta})$:

$$
\theta = E(\hat{\theta}) = \int \dots \int \hat{\theta}(y) f(y; \theta) \, dy
$$

Taking the derivative of this equation, the left side returns 1 and the right side returns:

$$
\frac{\partial}{\partial \theta} \int \dots \int \hat{\theta}(y) f(y; \theta) \, dy = \int \dots \int \hat{\theta}(y) \frac{\partial f(y; \theta)}{\partial \theta} \, dy
$$

$$
= \int \dots \int \hat{\theta}(y) \frac{\partial f(y; \theta)}{\partial \theta} \cdot f(y; \theta) \, dy
$$

$$
\int \dots \int \hat{\theta}(y) \frac{\partial \log f(y; \theta)}{\partial \theta} \cdot f(y; \theta) \, dy
$$

$$
\mathbb{E}\left( \hat{\theta}(Y) \frac{\partial \log f(Y; \theta)}{\partial \theta} \right) = \text{Cov}\left( \hat{\theta}(Y), \frac{\partial \log f(Y; \theta)}{\partial \theta} \right)
$$

$$
\text{Cov}\left( \hat{\theta}(Y), \frac{\partial \log f(Y; \theta)}{\partial \theta} \right) = 1
$$

Therefore:

$$
Var(\hat{\theta}) \geq \frac{1}{I(\theta)}
$$

## Final Considerations

Maximun likelihood method not only yields estimations centered around the true value, but also has its own tools to assess parameter variance. It is of great value to know whether attained values are sufficiently accurate. For unbiased parameters, the Cramér–Rao bound theorem offers complementary insight into MLE, demonstrating that, among the unbiased parameters, variance possesses a lower bound and the closer the estimated variance is to the lower bound, the more favorable it is within a given statistical model.
