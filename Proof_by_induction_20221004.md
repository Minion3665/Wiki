# Proof by induction

Imagine the matrix
$A = \begin{bmatrix} 3 & -4 \\ 1 & -1 \end{bmatrix}$

$A^2 = \begin{bmatrix} 5 & -8 \\ 2 & -3 \end{bmatrix}$  
$A^3 = \begin{bmatrix} 7 & -12 \\ 3 & -5 \end{bmatrix}$

A pattern seems to be emerging..., we can hypothesize that the general case is
$\begin{bmatrix} 2n + 1 & -4n \\ n & -2n + 1 \end{bmatrix}$, but how could
we prove this?

Assume that this is true for $n = k$, this means that $\begin{bmatrix} 3 & -4 \\
1 & -1 \end{bmatrix}^k =
\begin{bmatrix} 1 + 2k & -4k \\ k & 1-2k \end{bmatrix}$

Now let's consider $n = k + 1$,  
$\begin{bmatrix} 3 & -4 \\ 1 & -1 \end{bmatrix}^{k + 1} = \begin{bmatrix} 1 + 2k
& -4k \\ k & 1 - 2k \end{bmatrix}
\begin{bmatrix} 3 & -4 \\ 1 & -1 \end{bmatrix}$  
$= \begin{bmatrix} 3(1 + 2k) - 4k & -4(1+2k) + 4k \\ 3k + (1 - 2k) & -4k - 1 +
2k \end{bmatrix}$

- Show the base case
- Suppose it's true when $k = n$, demonstrate that this implies it is true for
  $k = n + 1$
- Show it's true when $k = 0$
- Give a conclusion

$(m - 3)(3m - 2) - m = 3m^2 - 9m - 2m + 6 - m = 3m^2 - 12m + 6 = 
3(m^2 - 4m + 2)$

Assume that $7^{2k - 1} + 3^{2k}$ is divisible by 8  
$7^2 * 7^{2k - 1} + 7^2 * 3^{2k}$

Prove $30|(n^5-n)$ for $n\in\Z, n \ge1$  
Base case:
$n^5 - n = 0$, 0 is divisible by 30  
Inductive step:  
Assume $k^5 -k$ is divisible by 30  
$(k + 1)^5 - (k + 1)$ should also be divisible by 30  
$(k + 1)^5 - (k + 1) =$

## Practice

### Sequences example

A sequence is defined by $u_{n + 1} = 4u_n  - 6$, $u_1 = 3$

Prove by induction that $u_n = 4^{n - 1} + 2$

#### Base case

$u_1 = 4^{0} + 2 = 3$

The base case is true...

#### Inductive step

- For $n = k$
- Assume that $u_k = 4^{k - 1} + 2$
- $u_{k + 1} = 4^k + 2$  
  $= 4 * 4^{k - 1} + 2$  
- $u_k - 2 = 4^{k -1}$
- $u_{k + 1} = 4 *(u_k - 2) + 2$  
  $= 4u_k - 8 + 2$  
  $= 4u_k - 6$

Therefore, if the statement is true for $n = k$ it is also true for $n = k + 1$

#### Conclusion

As the base case is true, and the inductive step shows that for each $k$ that is
true, $k + 1$ is also true, the statement is true for all values of $n$ where $n
\in \Z^+$

Q.E.D.

### Example

$3^n \ge 2n + 1$  
$\forall n \in \Z^+$  

#### Base case

$3^1 = 3$  
$2 * 1 + 1 = 3$  
$3 \ge 3$  

The base case is true, and for our base case $3^k > 1$

#### Inductive step

- For $n = k$  
- Assume that $3^k \ge 2k + 1$  
- $3 * 3^k \ge 2 * k + 2 + 1$  
- So long as $3^k$ is greater than 1, $2 * 3^k \gt 2$
- $\therefore$ as the left hand side has increased by $2 * 3^k$ and the right
  hand side has increased by 2, the left hand side must have increased by more
  than the right hand side if $3^k > 1$

#### Conclusion

Therefore, as the base case has $3^k > 1$ and fulfils the statement, and the
inductive step works so long as $3^k > 1$, the proof works $\forall n \in \Z^+$

Q.E.D.

### Prove $2^n > n^2, \forall n \in \Z > 4$

#### Base case

For $n = 5$:  
$2^5 = 32$  
$5^2 = 25$

#### Inductive step

- For $n = k$
- Assume $2^k > k^2$
- $2^k * 2$ should be $\gt (k + 1)^2$
- $(k + 1)^2 = k^2 + 2k + 1$
- The RHS increases by $2k + 1$
- The LHS increases by $2^k$
- $2^k$ should be $\gt 2k + 1$
- Let's do a short induction proof...

##### Inner induction proof

Prove $2^m > 2m + 1$  
$\forall n \in \Z \ge 3$

###### Inner base case

$2^3 = 8$  
$2 * 3 + 1 = 7$

###### Inner inductive step

- For $m = l$...
- Assume $2^k > $


$f(x, a) = $
