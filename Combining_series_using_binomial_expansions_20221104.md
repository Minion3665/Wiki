# Combining series using binomial expansions

$(1 + x)^n = 1 + nx + \frac{n(n-1)}{2!}x^2 + \frac{n(n-1)(n-2)}{3!}x^3 +
\dots$  

$\sqrt{\frac{1+x}{1-x}} = \sqrt{(1 + x)^1(1+(-x))^{-1}}$  
$= \sqrt{(1 +x + \frac{1-1}{2}x^2 + \frac{-1 + -2 + 1 + 2}{3!}x^3 + \dots)(
1 + x + \frac{2}{2!}x^2+\dots
)}$

$\frac{12x + 5}{(2x + 1)(3x + 1)} = \frac{A}{2x + 1} + \frac{B}{3x + 1}$  
$12x+ 5 = (3x + 1)A + (2x + 1)B$  

When $x = -\frac{1}{3}$:  
$-4 + 5 = (-\frac{2}{3} + 1)B$  
$1 = \frac{1}{3}B$  
$3 = B$  

When $x = -\frac{1}{2}$:  
$-6 + 5 = (-\frac{3}{2}+1)A$  
$-1 = -\frac{1}{2}A$  
$2 = A$

$2(2x +1)^{-1}+3(3x+1)^{-1}$  

$2(1 - 2x + \frac{2}{2!}(2x)^2 + \frac{-1(-2)(-3)}{3!}(2x)^3 + \dots) +
3(1-3x + \frac{2}{2!}(3x)^2 + \frac{-6}{3!}(3x)^3 + \dots)$  
$2(1 - 2x + 4x^2-8x^3 + \dots) +3(1 - 3x+9x^2-27x^3 + \dots)$  
$2 - 4x + 8x^2-16x^3+3-9x+27x^2-81x^3+\dots$  
$5-13x+35x^2-97x^3+\dots$

---

$p(x) = \frac{f(x) + f(-x)}{2}$  
$q(x) = \frac{f(x) - f(-x)}{2}$  
$p(x) + q(x) = \frac{f(x) + f(-x) + f(x) - f(-x)}{2}$  
$p(x) + q(x) = \frac{2f(x)}{2}$  
$p(x) + q(x) = f(x)$  


