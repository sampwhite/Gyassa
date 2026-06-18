---
layout: default
title: Evaluating Rating Systems
---

# Evaluating Rating Systems

*Aug 30 2011*

In my prior blog I suggested that head to head comparisons should be used to create numeric "chess-like" ratings using
the Elo Rating Model as a theoretical basis for the computations of ratings for various goods and services that are
evaluated. But this brings up a question. How do you determine whether it is truly better?

In the multiplayer game simulation I wrote, it is quite easy to determine how well the rating solution is performing.
The software knows the "true skill" of each player and can tell you precisely how well the differences in numeric
ratings of two opponents reflect their true skill differences.

But in a real game, you do not know the actual skill of the players involved so you need to find some other way to
determine how well the rating system is working. If this were a simple problem in sampling a random variable, then
typically one would calculate the Variance of a computed estimation as compared to the observed values by taking the
difference between the estimated value and an observed value, squaring it and then taking the mean of these squares. If
we were to do this for a rating system, we would take the rating difference between the two players (or goods or
services being compared) and use that to predict the percentage chance of the first player winning. That would be the
estimated value for the result of the match. The match would then have a result of 1.0 if the first player won, 0.0 if
the second player won, and 0.5 if there was a draw. We would then take the estimated value for the result of the match
and subtract the actual result (1.0, 0.0, or 0.5), square it and take the average of all the squares computed this way.
But this turns out not to work well. The rating difference for the players is only an estimated difference in ratings,
their true difference can only be known if the true skill values of the players are known. This estimation of the rating
difference does behave like a standard statistical random variable and can use standard variance computations for
judging accuracy if we had a way of sampling the actual rating differences with a standard probability variable. But the
estimated value for the winning percentage is a complicated function of the rating difference which means the random
variable for the winning percentage with respect to the rating difference does **not** follow a standard probability
distribution. How do we fix this?

It turns out that Jeff Sonas has already done this work and you can find the formula at
http://www.kaggle.com/c/ChessRatings2/Details/Evaluation. The formula for a variance term coming from a single resolved
match (the equivalent of one of the squares in the standard variance computation).

$$-[Y \cdot \log_{10}(E) + (1-Y) \cdot \log_{10}(1-E)]$$

where $\log_{10}$ is the logarithm base 10, $E$ is the estimated winning percentage, and $Y$ is 1.0, 0.0, or 0.5
depending on who actually won the match. The actual variance value is the mean of the variance terms computed for all
the matches played.

I go a little further than this in my use of this variance computation. I compute the best possible variance assuming
that for each match, the result is one that minimizes the variance term. So if the first player actually won the match,
but the variance term would be smaller if the opponent won, then the computation for best possible variance would use
that value instead. To compute an absolute variance value I take the variance value on actual results as proposed by
Jeff Sonas and subtract this best possible value. In the simulations I have performed, a value of 0.1 or less indicates
a well functioning rating system.

So how do we use this formula for other rating systems besides games? Take the example of ranking the best 100 films of
the last decade. Have a group of experts be consulted as the basis for creating this ranking. You evaluate the ranking
by randomly selecting two films from the best 100 films and asking one of the experts to judge which is better. You do
this many times with all the experts getting an equal number of head-to-head films to judge. The current ranking
predicts which film should be judged better and a percentage chance that this view will be confirmed by the expert can
be estimated by how far apart the film is on the top 100 list (maybe each difference in 1 can be declared to be 10 Elo
rating points, this computation can be varied until the variance computation is minimized) and then the chess evaluation
variance as described above can be used to rate the quality of the top 100 list.

For those who don't like an idea unless it can make money, I believe there is some untapped potential here as well. The
same approach could be used to evaluate stock pickers (predict which of two stocks will do better), rating agencies for
bonds (predict which of two bonds has a higher chance of default), and formulas for evaluating derivatives (which of two
derivatives is more accurately priced). Having a single metric that judges the quality of contributions of various
financial experts could have great monetary value.
