-   Review HTF sections 7.10 and 7.11

-   Consider a 1-nearest neighbor classifier applied to a two-class
    classification problem, where the marginal probability associated
    with either class is one half, and where the distribution of a
    univariate predictor is standard normal, independent of class (i.e.,
    not a very good predictor). Do the following:

> Show that the expected prediction error (EPE; HTF expression 7.3) is
> equal to 0.5.

$$
\\begin{aligned}
Err&=E\[L(Y,\\hat f(X))\]\\\\
&=Pr\\{Y\\neq \\hat f(X)\\}L(Y,\\hat f(X)|Y\\neq \\hat f(X))+ Pr\\{Y\\neq \\hat f(X)\\}L(Y,\\hat f(X)|Y\\neq \\hat f(X))  \\\\
&=\\frac{1}{2}\*1+\\frac{1}{2}\*0\\\\
&=\\frac{1}{2}
\\end{aligned}
$$

> Show that $E\_z\[\\hat{\\text{Err}}\_{\\text{boot}}\]$ (expectation of
> HTF expression 7.54) is approximately equal to 0.184, where *z*
> represents the training sample of *N* class and predictor pairs. Thus,
> demonstrate that the bootstrap estimate of EPE is optimistic.

$$
\\begin{aligned}
E\_z\[\\hat{\\text{Err}}\_{\\text{boot}}\]&=Pr\\{\\text{observation } i\\in \\text{bootstrap sample } b\\}\\hat{\\text{Err}}\_{\\text{boot}}|\\text{observation } i\\in \\text{bootstrap sample} +\\\\
&Pr\\{\\text{observation } i\\notin \\text{bootstrap sample } b\\}\\hat{\\text{Err}}\_{\\text{boot}}|\\text{observation } i\\notin \\text{bootstrap sample}\\\\
&=(1-(1-\\frac{1}{N})^N)\*0+(1-(1-(1-\\frac{1}{N})^N))\*0.5\\\\
&\\approx e^{-1}\*0.5\\\\
&=0.184
\\end{aligned}
$$

Since $E\_z\[\\hat{\\text{Err}}\_{\\text{boot}}\]$ is 0.184, which is
below 0.5, we can see that the bootstrap estimate of EPE is optimistic.

> Compute or approximate $E\_z\[\\hat{\\text{Err}}^{(1)}\]$ (expectation
> of HTF expression 7.56).

$$
\\begin{aligned}
E\_z\[\\hat{\\text{Err}}^{(1)}\]&=E(\\frac{1}{N}\\sum\_{i=1}^N\\frac{1}{|C^{-i}|}\\sum\_{b\\in C^{-i}}L(y\_i,\\hat f^{\*b}(x\_i) ))\\\\
&=\\frac{1}{N}\\sum\_{i=1}^N\\frac{1}{|C^{-i}|}E\[\\sum\_{b\\in C^{-i}}L(y\_i,\\hat f^{\*b}(x\_i) )\]\\\\
&=\\frac{1}{N}\\sum\_{i=1}^N\\frac{1}{|C^{-i}|}\\frac{1}{2} |C^{-i}|\\\\
&=\\frac{1}{2}
\\end{aligned}
$$

> Compute or approximate $E\_z\[\\hat{\\text{Err}}^{(0.632)}\]$
> (expectation of HTF expression 7.57)

$$E\_z\[\\hat{\\text{Err}}^{(0.632)}\]=0.368 \*E\[ \\overline{\\text{err}}\]+0.632 \* E\[ \\hat{\\text{Err}}^{(1)}\]=0.368\*0+0.632\*0.5=0.316 $$
