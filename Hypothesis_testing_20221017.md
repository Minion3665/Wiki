# Hypothesis testing

## Starter

$X\sim B(30, \frac{1}{5})$

### a

$0.0355$

### b

$1.24 * 10^{-3}$

### c

## Q1

| n   | $P(X \le n)$ |
| --- | ------------ |
| 12  | 0.868        |
| 13  | 0.942        |
| 14  | 0.979        |
| 15  | 0.994        |
| 16  | 0.999        |
| 17  | 0.9997       |
| 18  | 0.9999       |
| 19  | 0.9999       |
| 20  | 1            |

## Q4

$H_0: P = 0.7$  
$H_1: P \gt 0.7$

For us to accept the null Hypothesis, the seeds should follow a binomial
distribution of $X\sim B(20, 0.7)$

Test statistic = 18

$P(X \ge 18) = 0.0355$  
$0.0355 \lt 0.05$

Therefore, we reject the null hypothesis.

There is sufficient evidence at the 5% level of significance to accept that the
new seeds germinate better than the previous seeds.

## Q7

$H_0: p = 0.15$  
$H_1: p \not = 0.15$

Assume $H_0$ is true...  
$X\sim B(40, 0.15)$  
The mean is $40 * 0.15 = 6$  
$2 \lt 6$, so it must be on the left-hand tail  
$P(X \le 2) = 0.04860$  
$0.04860 \gt \frac{0.05}{2}$, $\therefore$ we accept $H_0$

There is not significant evidence at the 5% level of significance that the
probability of scoring a 5 is different to 0.15

## Q8

### a

$H_0: p = 0.5$  
$H_1: p \not = 0.5$

Assume $H_0$ is true...  
$X\sim B(10, 0.5)$  
The mean is $10 * 0.5 = 5$  
$7 \gt 5$, so it must be on the right-hand tail  
$P(X \ge 7) = 0.1718$  
$0.1718 > \frac{0.10}{2}$, $\therefore$ we accept $H_0$

There is not significant evidence at the 10% level of significance that Suzanne's
new racket has made a difference to the probability of her winning a match

### b

$H_0: p = 0.5$  
$H_1: p \gt 0.5$  

Assume $H_0$ is true...  
$X\sim B(20, 0.5)$  
$P(X\ge c)$ must be $\le 0.1$  

$P(X\ge13) = 0.1316$  
$P(X\ge14)= 0.05766$  

The critical value is 14

$\therefore$ Suzanne must win 14 games in order for her to conclude that the
squash racket has improved her performance

## Q9

$H_0: p=0.3$  
$H_1: p\ne 0.3$  

Assume $H_0$ is true...  
$X\sim B(15, 0.3)$

## Q10

It increases it. The probability of incorrectly rejecting $H_0$ is equal to the
significance level

## Q11

1%
