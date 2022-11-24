# Using hyperbolic identities to solve equations

[toc]

## The identities we'll be using

- $sinh(2x) = 2sinh(x) cosh(x)$
- $cosh2x = cosh^2x + sinh^2x$
- $cosh2x = 2cosh^2x - 1$
- $cosh2x = 2sinh^2x + 1$
- $sech^2x = 1 - tanh^2x$
- $cosech^2x = coth^2x-1$

## Ex8C

### 1

Note: *Mark schemes for questions 1a-1c just say "proof"*

#### a

$sinh2x = 2sinh(x)cosh(x)$  
$\frac{e^{2x}-e^{-2x}}{2} = 2(\frac{e^x-e^{-x}}{2})(\frac{e^x+e^{-x}}{2})$  
$e^{2x}-e^{-2x} = 4(\frac{(e^x-e^{-x})(e^x+e^{-x})}{4})$  
$e^{2x}-e^{-2x} = (e^x-e^{-x})(e^x+e^{-x})$  
$e^{2x}-e^{-2x} = ({e^x}^2-{e^{-x}}^2)$  
$e^{2x}-e^{-2x} = e^{2x}-{e^{-2x}}$

#### b

$cosh2x = 2cosh^2x-1$  
$\frac{e^{2x} + e^{-2x}}{2} = 2(\frac{e^{x}+e^{-x}}{2})^2-1$  
$\frac{e^{2x} + e^{-2x}}{2} = 2(\frac{(e^{x}+e^{-x})(e^{x}+e^{-x})}{4})-1$  
$\frac{e^{2x} + e^{-2x}}{2} = \frac{(e^{x}+e^{-x})(e^{x}+e^{-x})}{2}-1$  
$e^{2x} + e^{-2x} = (e^{x}+e^{-x})(e^{x}+e^{-x})-2$  
$e^{2x} + e^{-2x} = e^{2x}+1+e^{-2x}+1-2$  
$e^{2x} + e^{-2x} = e^{2x}+e^{0}+e^{-2x}+e^{0}-2$  
$e^{2x} + e^{-2x} = e^{2x}+e^{-2x}$

#### c

$cosech^2x=coth^2x-1$  
$\frac{1}{sinh^2x}=\frac{1}{tanh^2x}-1$  
$\frac{1}{sinh^2x}=\frac{cosh^2x}{sinh^2x}-1$  
$1=cosh^2x-sinh^2x$  
$1=\frac{(e^x+e^{-x})^2}{4}-\frac{(e^x-e^{-x})^2}{4}$  
$4=(e^x+e^{-x})^2-(e^x-e^{-x})^2$  
$4=(e^{2x}+e^{-2x}+2)-(e^2x+e^{-2x}-2)$  
$4=2--2$  
$4=4$

### 2

$sinh2x = 5sinh(x)$  
$2sinh(x)cosh(x)=5sinh(x)$  
Where $sinh(x) \not = 0...$  
$2cosh(x)=5$  
$e^x+e^{-x}=5$  
$e^x+e^{-x}=5$  
$e^x+(e^x)^{-1}=5$  
$e^x+\frac{1}{e^x}=5$  
$(e^x)^2+1=5e^x$  
$y:e^x$  
$y^2-5y+1=0$  
$y = \frac{5\pm\sqrt{21}}{2}$  
$\sqrt{21}<5$ so both solutions will be possible  
$e^x = \frac{5\pm\sqrt{21}}{2}$  
$x = \ln\frac{5\pm\sqrt{21}}{2}$  
$x \in \{1.57, -1.57\}$

> Missed x = 0 which is also possible as $sinh0=0$

### 3

$cosh2x=sinh(x) + 4$  
$2sinh^2x+1=sinh(x)+4$  
$2sinh^2x=sinh(x)+3$  
$2sinh^2x-sinh(x)-3=0$  
$y:sinh(x)$  
$2y^2-y-3=0$  
$y \in \{-1, \frac{3}{2}\}$  
$sinh(x) \in \{-1, \frac{3}{2}\}$  
$x \in \{-0.881, 1.19\}$

> Correct

### 4

$cosh2x-5cosh(x)-2=0$  
$2cosh^2x-1-5cosh(x)-2=0$  
$2cosh^2x-5cosh(x)-3=0$  
$y:cosh(x)$  
$2y^2-5y-3=0$  
$y \in \{3, -\frac{1}{2}\}$  
Negative arcosh isn't real so...  
$x = \ln(3 + \sqrt{3^2-1})$  
$x = \ln(3 + \sqrt{8})$

> Not fully simplified, $\sqrt8=\sqrt4*\sqrt2=2\sqrt2$

### 5

$2\sinh 2x=9\tanh x$  
$2\sinh 2x=9 \frac{\sinh x}{\cosh x}$  
$4sinh(x)cosh(x)=9 \frac{sinh x}{e^x+e^{-x}}$  
$4cosh(x)=9 \frac{1}{e^x+e^{-x}}$  
$4cosh^2(x)=9$  
$cosh^2(x)=\frac{9}{4}$  
$cosh(x)=\pm\frac{3}{2}$  
Negative arcosh isn't real, so reject $-\frac{3}{2}$
$cosh(x)=\frac{3}{2}$  
$x=\ln(\frac{3}{2} + \sqrt{x^2 - 1})$

> Missed $x=0$ again, I need to remember that this is possible

### 6

$2sech x = e^x$  
$2 * \frac{1}{\cosh x} = e^x$  
$2 * \frac{2}{e^x+e^{-x}} = e^x$  
$\frac{4}{e^x+e^{-x}} = e^x$  
$4 = e^x * e^x + e^{-x} * e^x$  
$4 = e^{2x} + 1$  
$3 = e^{2x}$  
$2x = \ln {3}$  
$x = \frac{1}{2}\ln 3$
$x = \ln 3^\frac{1}{2}$

> Correct as $\sqrt3=3^\frac{1}{2}$
