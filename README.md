# MCMC-without-bias

**Authors : BLAZER Alan, BOUVIER Oscar, DUPUY-ZINI Alexandre**

This school project aims at implementing [this paper](https://arxiv.org/abs/1708.03625), by illustrating its use on the PUMP problem explained [here](http://www.openbugs.net/Examples/Pumps.html).
_____
**More explainations are available in french in the notebook.**
_____

### PUMP problem resolution

The objective is to infere the values of 12 parameters fixed by the model using MCMC methods. All parameters had usual posteriors to sample from, but one parameter had an unusual one. Therefor, we used **Gibbs Sampling** to simulate values from the model made of multiple modules, and we had to find another way of sampling from the unusual posterior.

To achieve this, we first used **accept-reject** methods, but they ended being too slow to sample from. **Random Walk Metropolis-Hastings** seemed like a faster choice in that case. However, as this parameter was constrained to be strictly positive, a log-normal kernel was used.

This allowed us to find similar values as the original paper for this problem, obtained by using 15000 iterations, and a 3000 iterations burn-in. This burn-in was fixed in order to delete the bias from the random initialization, and the following iterations are used to get the estimations.

The paper we implemented aims at using maximal coupling in order to use shorter MCMC, while removing the bias by using couplings of Markov chains together with a telescopic sum argument.

### Maximal Coupling

To achieve this objective, we had to study and implement the **maximal coupling** algorithm, which samples from 2 random variables, and creates a couple of values which have a maximum probability of being equal. This will enable us to use 2 coupled MCMC, which have a maximum probability of being equal after being initialized randomly separatly. We applied this algorithm to all the random variables we encountered in the first resolution.

The telescopic sum will be useful in the estimation to remove the bias, for each parameter to infere.

### PUMP problem resolution using unbiased MCMC with couplings

Finally, after completely implementing the solution, we showed that the previous results could be obtained with a much shorter MCMC and no burn-in. Most MCMC had meeting times lower than 50 iterations, meaning that there is no more need to go up to 15000 iterations. We then studied the influence of the parameters in the estimations variance.

The idea is that those very short MCMC can be run simultaneously, instead of hoping to have a stationnary state after a lot of iterations. Those very short MCMC are biased independantly, but this aggregation method enables them to be unbiased collectively.
