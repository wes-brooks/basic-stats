# Mathematical statistics
Now, let's look at some of the math that allows us to use manipulate random variables in order to reason about their distributions. Beginning with some important identities that are used constantly when doing algebra with random variables. In particular, we will see how these identities relate to the mean. That's because the mean has some special properties: you've seen how we can calculate intervals based on known distributions that contain samples with a specified frequency. But we need to know the distribution. It turns out that the distribution of the sample mean approaches the Normal distribution as the sample size increases, for almost any independent data. That allows us to create intervals and reason about the distribution of real data, even though the data's distribution is unknown.


## Identities
The following hold for random variables $X_1, X_2, \dots$ that are independent:

 - $E(aX_1) = aE(X_1)$
 
 - $E(X_1 + X_2) = E(X_1) + E(X_2)$
 
 - $var(aX) = a^2 var(X_1)$
 
 - $var(X_1 + X_2) = var(X_1) + var(X_2)$
 
 In the case of calculating the mean, $a$ is $1/n$. So if the $X_1, X_2, \dots$ all share the same mean $\mu$ and the same variance $\sigma^2$, then the expectation identities say that the expectation of the mean is
 $$E[ \frac{1}{n} (X_1 + X_2 + \cdots + X_n) ] = \frac{1}{n} [ E(X_1) + E(X_2) + \cdots + E(X_n) ] \\ = \frac{1}{n} [n \times \mu ] \\
 =\mu$$
 
 And the variance identities say that the variance of the mean is
 $$var[ \frac{1}{n} (X_1 + X_2 + \cdots + X_n) ] = \frac{1}{n^2} [ var(X_1) +var(X_2) + \cdots + var(X_n) ] \\
 = \frac{1}{n^2} [n \times \sigma^2 ] \\
 =\sigma^2 / n$$
 
 So the mean of a sample of independent random variables has the same expectation as the population but its variance is much smaller. As a result, increasing sample size leads to greater precision in estimating the population mean. Let's take a look at some examples:
 


```r
# create a random variable X with mean -3 and standard deviation 3
X = function(n=20, mean=-3, sd=3) { rnorm(n, mean=mean, sd=sd) }

# show the histogram of some samples of X as well as the 0.12 and 0.88 quantiles:
x = X()
hist( x )
abline( v=quantile( x, c(0.12, 0.88) ), lty=2 )
```

<img src="02_mathematical-statistics_files/figure-html/unnamed-chunk-1-1.png" width="672" />

```r
# generate 50 samples of X and calculate their means:
mean_x = numeric( 50 )
for (i in 1:50) {
  mean_x[[ i ]] = mean( X() )
}

# plot the histogram of mean_x as well as the 0.12 and 0.88 quantiles
hist( mean_x )
abline( v=quantile( mean_x, c(0.12, 0.88) ), lty=2 )
```

<img src="02_mathematical-statistics_files/figure-html/unnamed-chunk-1-2.png" width="672" />


## Law of large numbers
The law of large numbers says that if the individual measurements are independent, then the mean of a sample tends toward the mean of the population as the sample size gets larger. This is what we'd expect, since we showed the rate at which the variance of the sample mean gets smaller is $1/n$.


```r
nn = c(1, 2, 4, 8, 12, 20, 33, 45, 66, 100)
means = sapply( nn, function(n) mean( rnorm(n) ) )
plot(nn, means, bty='n', ylab = "sample mean")
abline(h=0, lty=2)
```

<img src="02_mathematical-statistics_files/figure-html/unnamed-chunk-2-1.png" width="672" />


## Central limit theorem
The most important mathematical result in statistics, the Central Limit Theorem says that if you take (almost) any sample of random numbers and calculate its mean, the distribution of the mean tends toward a normal distribution. We illustrate the "tending toward" with an arrow and it indicates that the distribution of a sample mean is only *approximately* Normal. But if the original samples were from a Normal distribution then the sample mean has an *exactly* Normal distribution. From here, I'll start writing the mean of a random variable $X$ as $\bar{X}$ and the mean of a sample $x$ as $\bar{x}$.

$$ \bar{X} \rightarrow N(\mu, \frac{\sigma^2}{n}) $$
And because of the identities we learned before, you can write this as 
$$\frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \rightarrow N(0, 1) $$

In fact, the This is significant because we can use the standard normal functions on the right, and the data on the left, to start answering questions like, "what is the 95% confidence interval for the population mean?" 

### But isn't $\sigma$ unknown, too?
I've presented calculations that use the parameter $\sigma$. But in reality, $\sigma$ is just as unknown as $\mu$. Is it safe to replace $\sigma$ in the calculations by the standard deviation that we calculate from the sample using the `sd()` function? Not quite. If the $S$, the sample standard deviation (this is what you get from R's `sd()` function) is used for $\sigma$ in the CLT, then you need to include the uncertainty of estimating $\sigma$ by $S$ in the uncertainty of the distribution. As a result, the distribution will be slightly less clustered at the central point and have "heavy tails". This distribution is called the "t-distribution", and its discovery was a triumph of early modern statistics.

$$\frac{\bar{X} - \mu}{S/\sqrt{n}} \rightarrow t_{n-1} $$

Here the $n-1$ subscript denotes the "degrees of freedom" of the t-distribution. 



```r
# plot the densities of a standard normal distribution, and of a t-distribution with five degrees of freedom:
t = seq(-4, 4, length.out=1001)
  
plot( x=t, y=dnorm(t), ylim=c(0, 0.5), bty='n', lty=2, type='l', ylab="density" )
par( new=TRUE )
plot( x=t, y=dt(t, df=5), ylim=c(0, 0.5), xaxt='n', yaxt='n', xlab='', ylab='', bty='n', col='red', type='l' )
```

<img src="02_mathematical-statistics_files/figure-html/unnamed-chunk-3-1.png" width="672" />

Once again, if the original samples were from a Normal distribution, then the distribution of the mean is exactly a $t$ distribution, but otherwie the $t$ distribution is an approximation that gets better as the sample size increases.

What's more important than memorizing the precise formula is to remember that for (almost) any data where the samples are independent, the mean comes from some distribution that is more and more like the Normal distribution as the sample size increases. Let's take a look at an example using the uniform distribution.


```r
# generate 20 samples from a uniform distribution and plot their histogram
N = 20
u = runif( N )
hist( u )
```

<img src="02_mathematical-statistics_files/figure-html/unnamed-chunk-4-1.png" width="672" />

```r
# generate 100 repeated samples of the same size, calculate the mean of each one, and plot the histogram of the means.
B = 100
uu = numeric( B )
for ( i in 1:B ) {
  uu[[i]] = mean( runif(N) )
}

hist(uu)
```

<img src="02_mathematical-statistics_files/figure-html/unnamed-chunk-4-2.png" width="672" />

```r
# what happens as B and N get larger and smaller? Do they play different roles?
```

## Events and intervals
Answering realistic statistical questions is often a matter of evaluating the probability of some event. 

Given a t-distribution with eight degrees of freedom, the probability of sampling a number randomly from the distribution which is less than -1 is found from the distribution function:


```r
pt(-1, df=8)
```

```
## [1] 0.1732968
```

For that same distribution, if you want to know the value which is exceeded by 12% of samples, then you would use the quantile function:


```r
qt( 1 - 0.12, df=8 )
```

```
## [1] 1.26934
```

Again using the same distribution, you can evaluate the interval where 90% of samples are within the interval. 


```r
qt( c(0.05, 0.95), df=8 )
```

```
## [1] -1.859548  1.859548
```

But if that $t_8$ random variable is the result of a calculation like $\frac{\bar{X} - \mu}{S/\sqrt{n}}$, then the event that it is in the interval (-1.86, 1.86) is the event

$$ -1.86 < \frac{\bar{X} - \mu}{S/\sqrt{n}} \le 1.86 $$

Here, $\bar{X}$ and $S$ are random, while $n$ is the known sample size and $\mu$ is the unknown but fixed paramter that we want to put bounds around. Just rearranging terms we can get:

$$ \bar{X} - 1.86 \times S / \sqrt{n} \le \mu < \bar{X} + 1.86 \times S / \sqrt{n} $$
since this event occurs 90% of the time, we call the interval a 90% confidence interval. To change the confidence level, you would evaluate different quantiles of the t-distribution. If the degrees of freedom are not eight, then you would use the correct number for your data.

### Questions to ponder
 - Can you see a way to make the confidence interval smaller other than changing the confidence level? Why would that be helpful?
 - In the formula for a confidence interval, what is random and what is fixed? What would change if you collected new data by repeating the experiment? Does this suggest something about what the 90% means for our 90% confidence interval?



