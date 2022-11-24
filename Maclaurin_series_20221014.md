<!-- spell-checker:words Maclaurin,l'Hopital's -->

# Maclaurin series

- Any function can be written as a polynomial

<!-- -->

- $f(x) = a_0 + a_1x + a_2x^2 + \dots + a_\infty x^\infty$  
- We can get the first term by setting $x=0$  
- We can get the second term by differentiating the function...  
  $f'(x) = a_1 + 2a_2x + \dots$  
  ...and then setting $x = 0$  
  $f'(0) = a_1 + 0$
- If we continue doing this, we can see that a more general formula is  
  $a_n = \frac{f^{(n)}(x)}{n!}$

## Use the series of $\sin x$ to find the first 3 non-0 terms in $\sin (x^2)$

For $\sin x$...  
$a_0 = \frac{x}{1}$  
$a_1 = \frac{x^3}{1}$  
$a_2 = \frac{x^5}{2}$  

For $\sin x^2$  
$a_0 = \frac{x^2}{1}$  
$a_1 = \frac{x^6}{1}$  
$a_2 = \frac{x^{10}}{2}$  

## Find Maclaurin series up to the term in $x^3$ for $\ln(\frac{\sqrt{1+2x}}{2-3x})$

$\ln(\frac{\sqrt{1+2x}}{2-3x}) = \ln(\sqrt{1+2x}) + \ln(\frac{1}{2 - 3x})$  
$= \frac{1}{2}\ln(1 + 2x) + \ln((2-3x)^{-1})$  
$= \frac{1}{2}\ln(1 + 2x) - \ln(2 - 3x)$  
$= \frac{1}{2}(2x - \frac{4x^2}{2} + \frac{8x^3}{3}) - \ln2 + \frac{3}{2}x +
\frac{9}{8}x^2$  
$= x - x^2 +\frac{4}{3}x^3-\ln2+\frac{3}{2}x+\frac{9}{8}x^2$  
$= -\ln 2 + \frac{5}{2}x + \frac{1}{8}x^2 + \frac{4}{3}x^3$


