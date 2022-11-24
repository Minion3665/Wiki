# Improper integrals

- An improper integral is one that starts at $-\infty$, goes to $\infty$, or the
  integrand doesn't exist somewhere within the range
- The limit may or may not exist
- It's important to use the proper notation inside an exam...

## The proper notation, by example

Integrating $\int^{\infty}_{0}e^{-3x} \text{ d}x$

$\int^{\infty}_{0}e^{-3x} \text{ d}x$  
$= \lim_{p \to \infty} \int^{p}_{0}e^{-3x} \text{ d}x$  
$= \lim_{p \to \infty} [-\frac{1}{3}e^{-3x}]^p_0$

## Practice again

$\int^{\infty}_{0}xe^{-x} \text{ d}x$  
$= \lim_{n \to \infty} \int^{n}_{0}xe^{-x} \text{ d}x$  
$= \lim_{n \to \infty} -xe^{-x} + \int^{n}_{0}e^{-x} \text{ d}x$

$\int x^3\ln x \text{ d}x = \int x^3 \text{ d}x\ln x - \int x^3 \int \ln x
\text{ d}x \text{ d}x$  

$\sqrt 0 = 0$, therefore the integral is not defined at $x = 0$

$\lim_{n \to 0} \int^{4}_{n}\frac{1}{x^\frac{1}{2}} \text{ d}x$  
$\lim_{n \to 0} \int^{4}_{n}x^{-\frac{1}{2}} \text{ d}x$  
$\lim_{n \to 0} [2x^{\frac{1}{2}}]^4_n$  
