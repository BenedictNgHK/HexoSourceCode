---
title: Types of Error
author: Benedict Ng
categories:
	- [Numerical Analysis]
mathjax: true
---
Generally speaking, there are 4 types of error:

- Modeling Error
To abstract (simplify) a mathematical model from practical
problems, there exists an error between the model and the
actual problem.

- [Measurement Error](https://en.wikipedia.org/wiki/Observational_error)
The values of the parameters in the model are obtained
through measurement, and the observations find errors.

- [Truncation Error](https://en.wikipedia.org/wiki/Truncation_error)
Using numerical methods to obtain the approximate solution of
the model, there exists an error between the approximate solution
and the exact solution.

- [Roundoff Error](https://en.wikipedia.org/wiki/Round-off_error) 
Machine word length is limited, resulting in errors in
representing data in computers.

In this course, we mainly focus on the latter 2 types of error.

## Definition of Variables

| Sign      | Significance                              |
| :-:       | :-:                                       |
| $x$       | Exact Value                               |
| $x^*$     | Estimated Value                           |
| $p$       | Digits to be preserved after rounding off |
| $R_n$     | Truncation Error                          |
| $E$       | Roundoff Error                            |
| $e$       | Error                                     |
| $\epsilon$| Absolute Error                            |
| $e_r$     | Relative Error                            |
## Truncation Error

Formula: $R_n = x - x^*$
Example :
Suppose that we want to calculate the value of $e^x$ with first 4 terms of Taylor Expansion at point 0.
$x = 1 + \frac{x}{1!} + \frac{x^2}{2!} + ... + \frac{x^n}{n!} = \Sigma_{n = 0}^{n \to \infty}\frac{x^n}{n!}$
$x^* = 1 + \frac{x}{1!} + \frac{x^2}{2!} + \frac{x^3}{3!}$
$R_n = x^* - x = - \Sigma_{n = 4}^{n \to \infty}\frac{x^n}{n!}$

## Roundoff Error

Formula: $E = x - x*$ **Please note: the order is reversed.**
There are two types of Roundoff Error, but the formula is the same. Assume $x = \frac{2}{3}$ and $p=4$
Example:

### Round-by-chop

Only preserve the value before $p$ and abandon rest digits.
$$
x^* = 0.6666\space E = x^* - x = -0.000066666......
$$

### Round-to-nearest

Rounded the estimated value at digit $p$. If the number at digit $p$ is greater or equal to 5, abandon it; otherwise add the $p-1$ digit with value 1.
$$
x^* = 0.6667\space E = x^* - x = 0.0000333333......
$$

### Total Error

Total Error = Truncation Error + Roundoff Error

### Absolute Error & Error

$\epsilon(x) = |x^* - x|$
$e(x) = x^* - x$

### Relative Error

$$
e_r(x) = \frac{e}{x} = \frac{x^* - x}{x}
$$
