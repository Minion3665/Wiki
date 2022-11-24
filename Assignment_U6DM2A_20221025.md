# Assignment U6DM2A

## Q1

### a

Considering downwards velocity only...:  
$\frac{ds}{dt} = 9.8$  
$s = 9.8t$  
$d = 4.9t^2$  
$2.45 = 4.9t^2$  
$0.5 = t^2$  
$\sqrt{0.5} = t = 0.707$ (to 3s.f.)

### b

It's travelling at 20m/s rightwards for the entire time until it hits the ground
$0.707 * 20 = 14.14214m$

### c

When the ball hits the ground...  
Horizontal speed = $20$m/s  
Vertical speed = $9.8 * 0.707$ = $6.929646$m/s  
Gradient = $\frac{\text{change in y}}{\text{change in x}}$  
Gradient = $\frac{20}{6.929646}$ = $2.88615$  
Angle = $\arctan 2.88615$ = $1.237259$ = $1.24 radians$ (to 3s.f.)

## Q2

$s = 4\pi r^2$  
$\frac{ds}{dr} = 8\pi r$  
$\frac{dr}{dt} = 12$  
$\frac{ds}{dt} = \frac{ds}{dr} * \frac{dr}{dt}$  
$\frac{ds}{dt} = 8\pi r * 12 = 96\pi r$  
$\frac{ds}{dt}(150) = 14400\pi$

## Q3

### a

(from part b)  
$y = \frac{x^2}{9} - \frac{9}{x^2}$

Use the quotient rule...  
$\frac{dy}{dx} = \frac{18x}{81} - \frac{18x}{x^4}$  
$\frac{dy}{dx} = \frac{2x}{9} - \frac{18}{x^3}$

When $t = 0$...  
$x = 3e^0$  
$x = 3$
$\frac{dy}{dx}(3) = \frac{6}{9} - \frac{18}{27}$  
$= \frac{2}{3} - \frac{2}{3}$  
$= 0$  
$y = e^0 - e^0 = 0$

Tangent equation...  
$y = 0$

### b

$x = 3e^t$  
$\frac{x}{3} = e^t$  
$\log_e \frac{x}{3} = t$

$y = e^{2\log_e \frac{x}{3}} - e^{-2\log_e \frac{x}{3}}$  
$y = e^{\log_e \frac{x^2}{3^2}} - e^{\log_e{\frac{x^{-2}}{3^{-2}}}}$  
$y = \frac{x^2}{3^2} - \frac{x^{-2}}{3^{-2}}$  
$y = \frac{x^2}{3^2} - \frac{\frac{1}{x^2}}{\frac{1}{3^2}}$  
$y = \frac{x^2}{3^2} - \frac{3^2}{x^2}$  
$y = \frac{x^2}{9} - \frac{9}{x^2}$

## Q4

$3\sqrt{x}\frac{dx}{dt} = 8\sin2t$  
$\int 3\sqrt{x}dx = \int 8\sin2t dt$  
$2x^\frac{3}{2} = -16\cos2t + c$  
$x^\frac{3}{2} = -8\cos2t + d$  

When x = 0, t = 0...  
$0 = -8 + d$  
$d = 8$  
$x^\frac{3}{2} = -8\cos2t + 8$  
$x = (-8\cos2t + 8)^\frac{2}{3}$  

$\frac{dx}{dt}$ should be 0  
$8\sin 2t = 0$  
$\sin 2t = 0$  

This is true either every $\frac{\pi}{2}$  
So let's check some... as it's trig I'll only need to check 2  
When t = 0, x = 0 (by definition)
When t = $\frac{\pi}{2}$, x = 16  

Max height = 16m = 1600cm

## Q5

$\int_{\frac{1}{2}\pi}^{0}(3\cos\theta)(2\sin\theta)' \text{ d}\theta$  
$= \int_{\frac{1}{2}\pi}^{0}(3\cos\theta)(-2\cos\theta) \text{ d}\theta$  
$= \int_{\frac{1}{2}\pi}^{0}-6\cos^2\theta \text{ d}\theta$  
$= \int_{\frac{1}{2}\pi}^{0}-3(2\cos^2\theta -1 +1) \text{ d}\theta$  
$= \int_{\frac{1}{2}\pi}^{0}-3(\cos2\theta+1) \text{ d}\theta$  
$= \int_{\frac{1}{2}\pi}^{0}-3\cos2\theta -3 \text{ d}\theta$  
$= [-\frac{3}{2}\sin2\theta-3\theta]^0_{\frac{1}{2}\pi}$  
$= \frac{3}{2}\sin\pi + \frac{3}{2}\pi$  
$= 0 + \frac{3}{2}\pi$  
$= \frac{3}{2}\pi$

## Q6

### i

When $t = 0$...  
$a = (9\hat{\pmb{i}})ms^{-2}$

When $t = 3$...  
$a = (9\hat{\pmb{i}} - 12\hat{\pmb{j}})ms^{-2}$

### ii

$F = Ma$  
$|a| = \sqrt{9^2 + 12^2} = \sqrt{81 + 144} = \sqrt{225} = 15$

$F = 4 * 15 = 60N$

### iii

$v = 4\hat{\pmb{i}} + \int^{t}_{1}9\hat{\pmb{i}} \text{ d}t + 2\hat{\pmb{j}} +
\int^{t}_{1}-4t\hat{\pmb{j}} \text{ d}t$  
$v = 4\hat{\pmb{i}} + 9\hat{\pmb{i}}(t - 1) + 2\hat{\pmb{j}} +(-2t^2 -2)
\hat{\pmb{j}}$  
$v = (-5\hat{\pmb{i}} + 9\hat{\pmb{i}}t -2t^2\hat{\pmb{j}})ms^{-2}$

## Q7

### a

$x \in \mathbb{R} \cap [1, \infty)$

### b

$y = \frac{\sqrt{2x - 2} - x * \frac{d}{dx}((2x - 2)^\frac{1}{2})}{2x - 2}$  
$\frac{dy}{dx} = \frac{(2x - 2)^\frac{1}{2} - x(2x-2)^{-\frac{1}{2}}}{2x - 2}$  
$\frac{dy}{dx} = \frac{2x - 2 - x}{(2x - 2)^{\frac{3}{2}}}$  
$\frac{dy}{dx} = \frac{x - 2}{(2x - 2)^{\frac{3}{2}}}$

### c

$\frac{d^2y}{dx^2} = \frac{(2x - 2)^\frac{3}{2}-(x - 2)(3(2x -
2)^\frac{1}{2})}{(2x-2)^{3}}$  

A point of inflection is when $\frac{d^2y}{dx^2} = 0$  
For $\frac{d^2y}{dx^2}$ to be 0, the top must be 0  

$(2x - 2)^\frac{3}{2}-3(x - 2)\sqrt{2x - 2} = 0$  
$(2x - 2) - 3(x - 2) = 0$  
$2x - 2 - 3x + 6 = 0$  
$-x + 4 = 0$  
$x = 4$  

There is a single point of inflection, it is where $x = 4$

### d

x is convex when $\frac{d^2y}{dx^2}$ is positive  
Check at $x = 3$  
$(6 - 2) - 3(3 - 2) = 4 - 9 + 6 = 1$, so x is convex in the range $[1, 4)$

End of test
