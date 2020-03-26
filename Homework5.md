-   Complete HTF exercise Ex. 15.4

> Suppose *x*<sub>*i*</sub>, *i* = 1, ..., *N* are iid (*μ*,
> *σ*<sup>2</sup>). Let $\\bar x\_1^\*$ and $\\bar x\_2^\*$ be two
> bootstrap realizations of the sample mean. Show that the sampling
> correlation corr($\\bar x\_1^\*$,$\\bar x\_2^\*$) = $\\frac{n}{2n-1}$
> ≈ 50%. Along the way, derive var($\\bar x\_1^\*$) and the variance of
> the bagged mean $\\bar x\_{\\text{bag}}$. Here $\\bar x$ is a *linear
> statistic*; bagging produces no reduction in variance for linear
> statistics.

Let *x*<sub>*i*</sub><sup>*k*</sup> be sample *i* from the *k*th
bootstrap.

We know that

*E*(*x*<sub>*i*</sub><sup>*k*</sup>)=*μ*, *V**a**r*(*x*<sub>*i*</sub><sup>*k*</sup>)=*σ*<sup>2</sup>

$$
\\begin{aligned}
Cov(x\_i^k,x\_j^k)&=Cov(x\_i^k,x\_j^m), \\text{ where } i\\neq j, k\\neq m \\\\
&=E((x\_i^k-\\mu)(x\_j^m-\\mu)) \\\\
&=E(x\_i^kx\_j^m)-\\mu E(x\_i^k)-\\mu E(x\_j^m)+\\mu^2\\\\
&=E(\\bar x^2)-\\mu^2\\\\
&=(E(\\bar x))^2+Var(\\bar x)-\\mu^2\\\\
&=\\frac{\\sigma^2}{n}
\\end{aligned}
$$

$$Cov(\\bar x\_1^\*,\\bar x\_2^\*)=\\frac{1}{n^2}Cov(\\sum\_{i=1}^n x\_i^1,\\sum\_{j=1}^n x\_j^2)=\\frac{1}{n^2}n^2\\frac{\\sigma^2}{n}=\\frac{\\sigma^2}{n} $$

$$Var(\\bar x\_1^\*)=Var(\\frac{\\sum\_{i=1}^n x^1\_i}{n})=\\frac{\\sum\_{i=1}^n Var(x\_i^1)+2\\frac{n(n-1)}{2}Cov(x\_1^1,x\_2^1) }{n^2}=\\frac{n\\sigma^2+n(n-1)\\sigma^2/n}{n^2}=\\frac{(2n-1)\\sigma^2}{n^2} $$

$$
\\begin{aligned}
cor(\\bar x\_1^\*,\\bar x\_2^\*)&=\\frac{Cov(\\bar x\_1^\*,\\bar x\_2^\*)}{\\sqrt{Var(\\bar x\_1^\*)Var(\\bar x\_2^\*)}}\\\\
&=\\frac{\\sigma^2/n}{(2n-1)\\sigma^2/n^2}\\\\
&=\\frac{n}{2n-1}
\\end{aligned}
$$

Hence,

$$
\\begin{aligned}
Var(\\bar x\_{\\text{bag}})&=Var(\\frac{\\bar x\_1^\*+\\bar x\_2^\*}{2}) \\\\
&=\\frac{Var(\\bar x\_1^\*)+Var(\\bar x\_2^\*)+2Cov(\\bar x\_1^\*,\\bar x\_2^\*)}{4} \\\\
&=\\frac{(2n-1)\\sigma^2/n^2+(2n-1)\\sigma^2/n^2+2\\sigma^2/n }{4}\\\\
&=\\frac{3n-1}{2n^2}\\sigma^2
\\end{aligned}
$$

Since $Var(\\bar x\_1^\*)&gt;\\frac{\\sigma^2}{n}$, and
$Var(\\bar x\_{\\text{bag}})&gt;\\frac{\\sigma^2}{n}$, we know that
bagging produces no reduction in variance for linear statistics.
