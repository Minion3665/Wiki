# U6DM Revision Paper 2

## 4

### a

$\int x\sin x \text{ d}x = x \int \sin x \text{ d}x - \int \frac{d}{dx}(x)\int
\sin x\text{ d}x \text{ d}x$  
$= -x \cos x - \int \int \sin x \text{ d}x \text{ d}x$  
$= -x \cos x - \int -\cos x \text{ d}x$  
$= -x \cos x + \sin x + c$  
$= \sin x - x \cos x + c$

### b

$u = x^2 + 5$  
$\frac{du}{dx} = 2x$  
$x^2 = u - 5$  
$x = \sqrt{u - 5}$  
$\int x\sqrt{x^2 + 5} \text{ d}x = \int 2x^2\sqrt{u} \text{ d}u$  
$= \int 2(u - 5)\sqrt u \text{ d}u$  
$= \int 2u^\frac{3}{2} - 10u^\frac{1}{2} \text{ d}u$  
$= \frac{4}{5}u^\frac{5}{2} - \frac{20}{3}u^\frac{3}{2} + c$

## 6

### a

$\int xe^{5x} \text{ d}x$  
$= \frac{1}{5}e^{5x}x - \int \int e^{5x} \text{ d}x \text{ d}x$  
$= \frac{1}{5}e^{5x}x - \int \frac{1}{5}e^{5x} \text{ d}x$  
$= \frac{1}{5}e^{5x}x - \frac{1}{25}e^{5x} + c$

### b

$u = \sqrt x$  
$\frac{du}{dx} = \frac{1}{2}x^{-\frac{1}{2}}$  
$= \frac{1}{2} * \frac{1}{\sqrt x}$  
$= \frac{1}{2\sqrt x}$

$\int \frac{1}{\sqrt x(1 + \sqrt x)} \text{ d}x = \int \frac{1}{2\sqrt x}
\frac{1}{\sqrt x(1 + u)} \text{ d}u$  
$\int \frac{1}{2x(1 + u)} \text{ d}x$

## 2

### a

#### i

$\frac{7}{50}$

#### ii

$\frac{22}{50} = \frac{11}{25}$

#### iii

$\frac{14}{25}$

#### iv

$\frac{6}{23}$

#### v

$\frac{25}{46}$

### b

$\frac{22 * 21 * 20 * 19}{50 ^ 4} = \frac{4389}{156250}$

## 1

### a

$y(2.0) = -1$  
$y(2.1) = 0.161$

As there is a change of sign between these values and the curve is continuous in
this range, there must be a a solution to $y = 0$ between these 2 values

### b

$x^3 = x + 7$  
$x = \srqt[3]{x + 7}$

### c

$x_2 = 2.08$  
$x_3 = 2.09$  
$x_4 = 2.09$

## 9

### a

#### Prove for the base case

$\begin{bmatrix} 3 & 0 \\ 6 & 1 \end{bmatrix}^1$  
$= \begin{bmatrix} 3^1 & 0 \\ 3(3^n - 1) & 1 \end{bmatrix}$  
$= \begin{bmatrix} 3 & 0 \\ 6 & 1 \end{bmatrix}$

It's true

#### Prove that if it's true for k it's true for k+1

$$
