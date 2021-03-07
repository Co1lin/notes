# Quantitive Investment & Financial Optimization

It's my notes for the course *Topics on Quantitive Investment and Financial Optimization*. Copyright of the screenshots of slides belongs to the teacher.

## Log Optimal Strategy

![Screen Shot 2021-03-07 at 9.45.37 AM](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%209.45.37%20AM.png)

![Screen Shot 2021-03-07 at 9.46.01 AM](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%209.46.01%20AM.png)

### Log Utility Form

From the mathematics above, we have

![Screen Shot 2021-03-07 at 9.47.35 AM](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%209.47.35%20AM.png)

### Volatility Pumping

2 financial assets. Each of them has different profits and risks. We need to find a proportion to invest on them, to achieve the best return.

#### Stock & Cash

Stock: doubles of halves with 50% of chance

Cash: just remains the value

**Stock does not grow over time (using the log utility form above), and neither does cash.**

However, if we combine them together by 0.5 and 0.5, the assets will grow!!!

![Screen Shot 2021-03-07 at 10.00.08 AM](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%2010.00.08%20AM.png)

(~ 6% per period)

So, what is important is how the **inclusion** of an asset into a portfolio will affect the overall return, but **not its individual return**.

**Assets are valuable as members of portfolios.**

### Dominance of Log Optimal Strategy

The log optimal strategy is the best, from mathematical expectation.

However, it has some defects.

![Screen Shot 2021-03-07 at 10.03.19 AM](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%2010.03.19%20AM.png)

## The Markowitz model
**Choosing a portfolio of risky assets**

### Efficient Frontier

![Markowitz_frontier](Quantitive%20Investment.assets/Markowitz_frontier.jpg)

### The Model

![Screen Shot 2021-03-07 at 11.20.29 AM](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%2011.20.29%20AM.png)

### Linear Property

Any linear combinations of any two points on the efficient frontier **belong to the efficient frontier**!

By having two portfolios, one can generate any portfolios on the efficient frontier.

### Including a Risk-Free Asset

Ref: [Risk-free asset and the capital allocation line](https://en.wikipedia.org/wiki/Modern_portfolio_theory#Risk-free_asset_and_the_capital_allocation_line)

![Screen Shot 2021-03-07 at 11.47.39 AM](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%2011.47.39%20AM.png)

![https://www.math.ust.hk/~maykwok/courses/ma362/Topic2.pdf](Quantitive%20Investment.assets/Screen%20Shot%202021-03-07%20at%2011.52.20%20AM.png)

When a risk-free asset is introduced, the half-line shown in the figure is the new efficient frontier.

Its vertical intercept represents a portfolio with 100% of holdings in the risk-free asset;

the tangency with the parabola represents a portfolio with no risk-free holdings and 100% of assets held in the portfolio occurring at the tangency point;

points between those points are portfolios containing positive amounts of both the risky tangency portfolio and the risk-free asset;

and points on the half-line beyond the tangency point are **leveraged portfolios** involving **negative ($\alpha < 0$)** holdings of the risk-free asset (the latter has been sold short—in other words, the investor has **borrowed** at the risk-free rate) and an amount invested in the tangency portfolio equal to more than 100% of the investor's initial capital.

By the diagram, the introduction of the risk-free asset as a possible component of the portfolio has **improved** the range of risk-expected return combinations available, because **everywhere except at the tangency portfolio the half-line gives a higher expected return** than the hyperbola does at every possible risk level.

