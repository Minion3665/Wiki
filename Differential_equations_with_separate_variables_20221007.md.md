# Differential equations with separate variables

## Solve $\frac{dy}{dx} = (1.2 + 0.4x)y$

$\frac{dy}{dx} = (1.2 + 0.4x)y \implies \frac{1}{y}\frac{dy}{dx} = 1.2 + 0.4x$  
$\int \frac{1}{y}\frac{dy}{dx} \text{ d}x = \int 1.2 + 0.4x \text{ d}x$  
$\int \frac{1}{y} \text{ d}y = \int 1.2 + 0.4x \text{ d}x$  
$\ln y = 1.2x + 0.2x^2 + c$  
$y = e^{1.2x + 0.2x^2 + c}$  
$y = e^{1.2x + 0.2x^2}e^c$  
$y = Ae^{1.2x + 0.2x^2}$

## solve $\frac{dy}{dx} = xy - x$

$\frac{dx}{dy} = xy - x \implies \int \frac{1}{x} \text{ d}x = \int y - 1
\text{ d}y$  
$\ln(x) = \frac{1}{2}y^2 - y + c$  
$x = Ae^{\frac{1}{2}y^2 - y}$

## solve $\frac{d\theta}{dt} = -0.054(\theta - 21)$  

$\frac{d\theta}{dt} = -0.054\theta + 1.134$  
$\int -0.054\theta + 1.134 \text{ d}t =\theta$  
$\frac{d\theta}{dt}*-0.054\theta +1.134t+c = \theta$  
$-0.054\theta(-0.054\theta + 1.134) + 1.134t + c = \theta$

When $t = 0$, $\theta = 94$...  
$(-0.054\theta$

## solve $t\frac{dH}{dt} = H$

$\frac{1}{H}t\frac{dH}{dt} = 1$  
$\frac{1}{H}\frac{dH}{dt} = \frac{1}{t}$  
$\int \frac{1}{H} \text{ d}H = \int \frac{1}{t} \text{ d}t$  
$\ln(H) = \ln(t) + c$  
$H = Ae^{\ln(t)}$

When $t = 1$...  
$H = A \implies A = 2$

When $t = 5$...  
$H = 2e^{\ln5}$  
$H = 10$

## solve $\frac{dN}{dt} = -kN$, where $k$ is a positive constant

$\int \frac{1}{N} \text{ d}N = \int -k \text{ d}t$  
$\ln(N) = -kt + c$  
$N = e^{-kt + c}$  
$N = e^{-kt}e^c$  
$N = Ae^{-kt}$

## solve $x\frac{dy}{dx}+4=y^2$, give your answer in the form $y = f(x)$

$x \frac{dy}{dx} = y^2 - 4$  
$\frac{dy}{dx} = \frac{1}{x}(y^2 - 4)$  
$\frac{1}{y^2 - 4}\frac{dy}{dx} = \frac{1}{x}$  
$\int \frac{1}{y^2 - 4} \text{ d}y = \int \frac{1}{x} \text{ d}x$  
$\int \frac{1}{y^2 - 4} \text{ d}y = \ln(x)$  

