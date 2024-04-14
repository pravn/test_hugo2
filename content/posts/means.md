+++
title = 'PDF Means and Moment Generating Functions'
date = 2024-03-17T20:05:43-04:00
draft = false
+++
This is a post to test out my new setup with hugo. I was not very happy with my setup in wordpress owing to lack of flexibility in writing equations. And when it wanted me to pay for it, I sort of gave up on the platform. Suffice it to say that that is the reason for the considerable delay in posting. 

A fair bit has changed since I had last posted. I have moved to a new country, and work on a nice research problem, with some really smart folks to collaborate with. My research work is in Bird's Eye View fusion in autonomous vehicles, and I will cover ideas from that problem in future posts. That being said, a personal goal is to cover the BDA book by Gelman et al, doing as much justice to it as I can manage in my copious free time. As is well known, we measure our lives in coffee spoonfulls of equations. So someday, when we sit upon the shore fishing, we can say that the lands have been set in order. 

Digressions aside, this time, we look at some very simple ideas concerning probability distributions. We look at two distributions - one continuous and the other discrete, and derive the means as first moments. We look at moment generating functions to help us. 

## Mean as first moment of PDF
For a continuous distribution {{< katex >}}$ f(x)$ {{< /katex >}} we define the mean of the distribution as 

```katex 
$$ \mu = \int x f(x) dx $$
```

For a discrete distribution, it is written as 

```katex
$$ \mu = \sum x f(x) $$
```

## Moment generating functions 
Now we define the moment generating function, which can be used to derive the means and higher order moments. 

The continuous version can be written as 
```katex 
$$ M_X(t) = \int e^{tx} f(x) dx $$
``` 
Likewise the discrete version is as follows:
```katex
$$ M_X(t) = \sum e^{tx}f(x) $$
```
We can differentiate the MGF to obtain moments:

```katex 
$$ \frac{ d M_X(t)}{dt} = \int x e^{tx} f(x) dx $$
```

Setting {{< katex >}} $t=0$ {{< /katex >}} gives us the first moment or the mean 

```katex 
$$ \frac{ d M_X}{dt}|_{t=0} = \int x f(x) dx $$
```

Naturally, higher moments such as variance or kurtosis can also be obtained this way. When we compute all the moments, we have a full description of the PDF.

Let us use these ideas to derive expressions for two common distributions.

## Binomial distribution
The binomial distribution is discrete, written as follows:

```katex 
$$ \begin{align}
f(x) = {n \choose x} p^x (1-p)^{n-x} 
\end{align}
$$
```
The mean for this is 

```katex 
$$ \begin{align}
\mu = \sum_{x=0}^n {n \choose x} x p^x (1-p)^{n-x} 
\end{align}
$$
```

We can unroll the coefficient {{< katex >}} ${n \choose x} $ {{< /katex >}} to simplify the expressions

```katex
$$
\begin{align}
        \mu &= \sum_{x=0}^n {n \choose x} x p^x (1-p)^{n-x} \\
            &= \sum_{x=0}^n \frac{n \cdot (n-1)!}{x \cdot (x-1) ((n-1)-(x-1))!} x p p^{x-1} (1-p)^{(n-1)-(x-1)} \\
            &= np \sum_{x=0}^{n-1} {n-1 \choose x-1} p^{x-1} (1-p)^{(n-1)-(x-1)} \\
            &= np  
\end{align}
$$
```

In the above, we make use of the fact that the binomial distribution sums to 1. 

```katex
$$ \begin{align}
 \sum_{x=0}^n {n \choose x} p^x (1-p)^{n-x} = 1 
\end{align}
$$
```

Next, we use the MGF to derive the means. 
```katex
$$
\begin{align}
    M_X(t) &= \sum_{x=0}^n {n \choose x} p^x (1-p)^{n-x} \\
    &= \sum_{x=0}^n {n \choose x} (pe^t)^x (1-p)^{n-x}\\
    &= (pe^t + 1-p)^n
\end{align}
$$
```

We now invoke the definition of the mean from the MGF. 

```katex
$$ 
\begin{align}
\mu &= \frac{ dM_X}{dt}|_{t=0} \\
&= np (pe^t + 1-p)^{n-1}|_{t=0} \\
&= np
\end{align}
$$
```

## Gamma distribution
Consider the continuous gamma distribution whose mean we would like to compute

```katex
$$
\begin{align}
   f(x) = \frac{1}{\Gamma(\alpha) \beta^\alpha} x^{\alpha-1} e^{-x/\beta}
\end{align}
$$
```

The MGF for this function is 

```katex
$$
\begin{align}
   M_X(t) &= \frac{1}{\Gamma(\alpha) \beta^\alpha} \int_{0}^\infty e^{tx} x^{\alpha-1} e^{-x/\beta} dx \\
   &= \frac{1}{\Gamma(\alpha) \beta^\alpha} \int_{0}^\infty x^{\alpha-1} e^{-x(\frac{1}{\beta} -t)} dx \\
   &= \frac{1}{\Gamma(\alpha) \beta^\alpha} \int_{0}^\infty x^{\alpha-1} e^{-x/(\frac{\beta}{1-\beta t})} dx 
\end{align}
$$
```
Recall that the expression for the gamma function below is a pdf:
```katex
$$
\begin{align}
   f(x) = \frac{1}{\Gamma(a) b^a} \int_0^\infty  x^{a-1} e^{-x/b} dx 
\end{align}
$$
```

It should therefore integrate to {{< katex >}}$ 1$ {{< /katex >}}, so that


```katex
$$
\begin{align}
   \int_0^\infty  x^{a-1} e^{-x/b} dx = \Gamma(a) b^a 
\end{align}
$$
```

Using this in the equation for the moment generating function we get 

```katex
$$
\begin{align}
  \int_{0}^\infty x^{\alpha-1} e^{-x/(\frac{\beta}{1-\beta t})} dx = \Gamma(\alpha) (\frac{\beta}{1-\beta t})^\alpha  
\end{align}
$$
```

The MGF then becomes 


```katex
$$
\begin{align}
  M_X(t) = \frac{1}{\Gamma(\alpha) \beta^\alpha} \int_{0}^\infty x^{\alpha-1} e^{-x/(\frac{\beta}{1-\beta t})} dx = (\frac{1}{1-\beta t})^\alpha  
\end{align}
$$
```

We can now derive the expectation from the MGF:
```katex
$$
\begin{align}
  EX = \frac{d}{dt} M_X(t) = \frac{\alpha \beta}{[(1-\beta t)^{\alpha+1}]|_{t=0}} = \alpha \beta  
\end{align}
$$
```

## Summary
We have derived through some raw machinery, the means of two well known distributions - the binomial and gamma. We used the exercise as a way to demonstrate the potentialities of the moment generating function. I have copied these derivations from what I think is the most essential book on all things statistics - Casella and Berger. In the next few posts, I will post on similar exercises from BDA3. 

## References
1. Statistical Inference - Casella and Berger 
2. Bayesian Data Analysis (BDA3) - Gelman et. al.  