# Integration 2 assessment

## Question 1

### a

$\int e^{4x} \text{ d}x = \frac{1}{4}e^{4x} + c$

### b

$\int e^{4x}(2x + 1) \text{ d}x$  
$= \int 2xe^{4x} + e^{4x} \text{ d}x$  
$= \frac{1}{4}e^{4x}2x - \int e^{4x}2 \text{ d}x + \frac{1}{4}e^{4x}$  
$= \frac{1}{2}xe^{4x} - \frac{1}{2}e^{4x} + \frac{1}{4}e^{4x}$  
$= \frac{1}{2}xe^{4x} - \frac{1}{4}e^{4x}$

### c

$\int \frac{1 + \ln x}{x} \text{ d}x$  
$= \int \frac{u}{x} \text{ d}x$  
$\frac{du}{dx} = \frac{1}{x}$  
$du = \frac{1}{x}dx$

$= \int u \text{ d}u$  
$= \frac{u^2}{2} + c$  
$= \frac{(1 + \ln x)^2}{2} + c$

## Question 2

### i

$\int x\sin2x \text{ d}x = x\int \sin2x \text{ d}x - \int -\frac{1}{2}\cos2x
\text{ d}x+c$  
$= -x \frac{1}{2}\cos2x +\frac{1}{4}\sin2x + c$

### ii

$\int^{\frac{\pi}{2}}_{0}x(3+\sin2x) \text{ d}x = \int^{\frac{\pi}{2}}_{0}3x +
x\sin2x \text{ d}x$  
$= \int^{\frac{\pi}{2}}_{0}3x \text{ d}x +
[-x\frac{1}{2}\cos2x+\frac{1}{4}\sin2x]^\frac{\pi}{2}_0$  
$= [\frac{3}{2}x^2-x \frac{1}{2}\cos2x+\frac{1}{4}\sin2x]^\frac{\pi}{2}_0$  
$= \frac{3\pi^2}{8} - \frac{\pi}{4} \cos\pi + \frac{1}{4}\sin\pi
+0-0+0$  
$= \frac{3\pi^2}{8} + \frac{\pi}{4}$  
$= \frac{3\pi^2}{8} + \frac{2\pi}{8}$  
$= \frac{3\pi^2 + 2\pi}{8}$  
$= \frac{\pi(3\pi + 2)}{8}$

## Question 3

### a

$y = x^{-2}\ln x$  
$\frac{dy}{dx} = -2x^{-3}\ln x + x^{-2} * \frac{1}{x}$  
$= -2x^{-3}\ln x +x^{-3}$  
$= x^{-3}(1 - 2\ln x)$  
$= \frac{1 - 2\ln x}{x^3}$

### b

$\int x^{-2}\ln x \text{ d}x = -\frac{1}{x}\ln x - \int -\frac{1}{x}*
\frac{1}{x} \text{ d}x + c$  
$= - \frac{1}{x}\ln x + \int \frac{1}{x^2} \text{ d}x + c$  
$= -\frac{1}{x}\ln x - \frac{1}{x} + c$  
$= -\frac{1}{x}(\ln x + 1) + c$

### c

#### i

$\frac{1-2\ln x}{x^3} = 0$  
$1 - 2\ln x = 0$  
$2\ln x = 1$  
$\ln x = \frac{1}{2}$  
$e^{\frac{1}{2}} = x$

#### ii

$\int^{5}_{1}x^{-2}\ln x \text{ d}x = (-\frac{1}{5}(\ln5+1)) + (\ln1 + 1)$  
$= -\frac{1}{5}\ln 5 - \frac{1}{5} + 1$  
$= \frac{4}{5} -\frac{1}{5}\ln 5$  
$= \frac{1}{5}(4 - \ln5)$

## Question 4

### a

#### i

$f(x) = x^4+2x$  
$f'(x) = 4x^3 + 2$

#### ii

The top is $\frac{1}{2}$ times the derivative of the bottom...

$\int \frac{2x^3 + 1}{x^4 + 2x} \text{ d}x = \int \frac{1}{2} *
\frac{4x^3 + 2}{x^4 + 2x} \text{ d}x$  
$= \frac{1}{2} \ln(x^4+2x) + c$

### b

#### i

$u = 2x + 1$  
$\frac{du}{dx} = 2$  
$dx = \frac{1}{2}du$  
$2x = u - 1$  
$x = \frac{1}{2}u - \frac{1}{2}$  
$\int x\sqrt{2x + 1} \text{ d}x = \int \frac{1}{2}x\sqrt u \text{ d}u$  
$=\int \frac{1}{4}(u - 1)\sqrt u \text{ d}u$  
$= \frac{1}{4}\int u^{\frac{3}{2}} - u^{\frac{1}{2}} \text{ d}u$

#### ii

When $x = 4$...  
$u = 2 * 4 + 1 = 9$  
When $x = 0$...  
$u = 0 + 1 = 1$

$\frac{1}{4}\int^{9}_{1} u^{\frac{3}{2}} - u^{\frac{1}{2}} \text{ d}u$  
$= \frac{1}{4}[\frac{2}{5}u^{\frac{5}{2}}-\frac{2}{3}u^{\frac{3}{2}}]^9_1$  
$= \frac{1}{4}(\frac{2}{5}*9^{\frac{5}{2}}-\frac{2}{3}*9^{\frac{3}{2}} -
\frac{2}{5}+ \frac{2}{3})$  
$= 19.86... = 19.9 \text{ to 3 s.f.}$

## Question 5

### a

$\frac{3x - 5}{(x + 3)(2x - 1)} = \frac{A}{x + 3} + \frac{B}{2x - 1}$  
$3x - 5 = A(2x - 1) + B(x + 3)$

When $x = \frac{1}{2}$...  
$\frac{3}{2}-5=B(\frac{1}{2} + 3)$  
$-\frac{7}{2} = \frac{7}{2}B$  
$B = -1$

When $x = -3$...  
$-9-5 = A(-6-1)$  
$-14 = -7A$  
$A = 2$

$\frac{3x - 5}{(x + 3)(2x - 1)} = \frac{2}{x + 3} - \frac{1}{2x - 1}$

### b

$\int \frac{3x - 5 }{(x + 3)(2x - 1)} \text{ d}x = \int \frac{2}{x + 3} -
\frac{1}{2x - 1} \text{ d}x$  
$= 2\ln(x + 3) - \ln(2x - 1) + c$

## Question 6

### a

$4\sin^2x = 4 - 4\cos^2x$  
$\cos2x = 2\cos^2x - 1$  
$4 - 4\cos^2x = -2(2\cos^2x - 1) + 2$  
$= 2 - 2\cos2x$  

### b

$\int^{\frac{\pi}{12}}_{0} 2 - 2\cos2x \text{ d}x$  
$= [2x -\sin2x]^\frac{\pi}{12}_0$  
$= 0.0236 \text{ to 3s.f.}$


