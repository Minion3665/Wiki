# Normal distribution

- Most of the data is centered around the mean
- You're very unlikely to be around the outside
- A theoretical normal distribution doesn't stop at the ends

  - In practice it actually doesn't: e.g. you can't get negative height
  - Thankfully, the area under the graph is so small that nobody cares most of
    the time

- The [[probability_density_function.md|pdf]] of a normal distribution is
  impossible to differentiate

The notation for a normal distribution is $X~N(\text{mean}, \text{variance})$

- ~2/3 of the data lies within 1 standard deviations of the mean
- ~95% of the data lies within 2 standard deviations of the mean
- ~99.7% of the data lies within 3 standard deviations of the mean
- By the time you're up to 4 standard deviations, good luck seeing any data

a. $P(2 < X < 11) = 0.5244$  
b. $P(X < 8) = 0.8413$  
c. $P(X > 8) = 0.1586$  

For $X\sim N(\mu, \sigma^2)$, the "Z-score" is the number of standard deviations
away from the mean. It follows a normal distribution $X\sim N(0, 1)$

a. 0.0228
b. 0.0851
c. 0.0214 / 0.0228 = 93.9%


