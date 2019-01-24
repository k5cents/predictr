*predictr*
==========

Using R to compare the predictive capabilities of markets and models.

Project Background
------------------

In recent years, the forecast model has become a staple of political
punditry. Popularized by the data journalism site
[FiveThirtyEight](https://fivethirtyeight.com/), the forecast model is a
statistical tool used to incorperate a number of quantitative inputs and
output a probabalistic view of all possible outcomes.

On the eve of the 2016 presidential election, all mainstream forecasts
gave Hillary Clinton overwhelming odds to win the presidency.
FiveThirtyEight's forecast model gave Clinton the lowest odds, [at
71%](https://goo.gl/CLPrUC). Meanwhile, The New York Times Upshot
calculated [85%]((https://goo.gl/QES9vJ)) and the HuffPo Pollster's
prediction published an infamous [98% chance of a Clinton
victory](https://goo.gl/XJqwyD).

Is there a better alternative to forecasting models?

Markets are an alternative method used to ascertain a probabalistic
prediction of election results. Instead of a mathematical model
incorperating quantitative inputs, a market has self-interested traders
bet on the outcomes to determine the likelihood of each.

I posit that prediction markets may claim a distinct niche in the field
of political forecasting. In theory, prediction markets encompass the
data generated by forecasting models and crowdsource additional
unquantifiable variables.

This project compares the accuracy of markets and models in their
ability to predict the winner of many 2018 midterm congressional
elections.

Predictive Methods
------------------

### Polling and Aggrigation

Opinion polling is the most common form of election predicting. By
[randomly sampling](https://en.wikipedia.org/wiki/Sampling_(statistics))
the overall population of potential voters, pollsters can ask a few
thousand Americans of their voting intentions and determine the division
of votes in the actual election. Sampling errors and systemic errors
prevent this statistical tool from perfectly predicting the elction. By
aggrigating a bunch of polls and averaging their results, sites like
[RealClearPolitics](https://www.realclearpolitics.com/) take advantage
of the [law of large
numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers) to better
calculate the true opinion of the population.

### Forecasting Models

In the word's of Nate Silver from FiveThirtyEight:

> (Forecasting models) take lots of polls, perform various types of
> adjustments to them, and then blend them with other kinds of
> empirically useful indicators (what we sometimes call “the
> fundamentals”) to forecast each race. Then they account for the
> uncertainty in the forecast and simulate the election thousands of
> times.

I will be using the FiveThirtyEight model to collect forecasting data.
In 2016, FiveThirtyEight's prediction was closest to reality. They are
one of the few mainstream forecasters to continue their work into the
2018 midterm elections.

The exact process of the FiveThirtyEight is proprietary, so we can't
know exaclty what data is being incorperated in what ways. In the
"classic" version of their model, three types of quantifiable data are
used:

1.  **Polling**: District-by-district polling, adjusted for house
    effects and other factors.
2.  **C.A.N.T.O.R.**: A proprietary system which infers results for
    districts with little or no polling from comparable districts where
    polling has been done.
3.  **Fundamentals**: Non-polling factors that historically help in
    predicting congressional races:
    -   Incumbency
    -   State partisanship
    -   Incumbent previous margins
    -   Generic ballot
    -   Fundraising
    -   Incumbent voting record
    -   Challenger experience
    -   Scandals

In the training data (most House races since 1998), the classic model
correctly predicted *96.7%* of races at the time of election.
FiveThirtyEight points out that the *vast* majority of races are
blowouts, inflating this accuracy percentage.

### Prediction Markets

Prediction Data
---------------

### FiveThirtyEight Model

### PredictIt Markets

Project Findings
----------------

Conclusion
----------

Sources
-------
