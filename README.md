---
title: "README"
author: "Kiernan Nicholls"
date: "December 14, 2018"
output: 
  html_document: 
    keep_md: yes
---





# predictr

Using R to compare the predictive capabilities of markets and models.

## TL;DR

Prediction markets are able to predict elections with upwards of 90% accuracy.

## Project

Are prediction markets as good as forecasting models in predicting elections?

In recent years, the forecast model has become a staple of political punditry.
Popularized by the data journalism site
[FiveThirtyEight](https://fivethirtyeight.com/), the forecast model is a
statistical tool used to incorperate a number of quantitative inputs in the
output of a probabalistic view of outcomes.

On the eve of the 2016 presidential election, all mainstream forecasts gave
Hillary Clinton overwhelming odds to win the presidency. FiveThirtyEight's
forecast model gave Clinton the lowest odds, [at 71%](https://goo.gl/CLPrUC).
Meanwhile, The New York Times Upshot had her at [85%]((https://goo.gl/QES9vJ)),
PredictWise [89%](https://goo.gl/1Mnf7m), and the HuffPo Pollster's prediction
gave Clinton an infamous [98% chance of winning](https://goo.gl/XJqwyD).

These forecast models represent the best statistical predictions possible. Yet
we now know they were all wrong (some of them more so than others).

Prediction markets are an alternative method to calculate a probabalistic
prediction of election results. Instead of a mathematical model incorperating
inputs, a market has self-interested traders bet on the outcome to determine the
likelihood of all outcomes.

I posit that prediction markets may claim a distinct niche in the field of
political predictions. In theory, prediction markets encompass the data
generated by forecasting models and crowdsource additional unquantifiable
variables. The **efficient-market hypothesis** states that asset prices fully
reflect all available information, not just polling samples or mathematical
factors.

I will be comparing the ability of markets and models to accurately predict
races in the 2018 midterm.

## Polling and Aggrigation

Opinion polling is the most common form of election predicting. By randomly
sampling the overall population of potential voters, pollsters can ask a few
thousand Americans of their voting intentions and determine the division of
votes in the actual election. Sampling errors and systemic errors prevent this
statistical tool from perfectly predicting the elction. By aggrigating a bunch
of polls and averaging their results, sites like
[RealClearPolitics](https://www.realclearpolitics.com/) use the [law of large
numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers) to better calculate
the true opinion of the population.

## Forecasting models

> (Forecasting models) take lots of polls, perform various types of adjustments
to them, and then blend them with other kinds of empirically useful indicators
(what we sometimes call “the fundamentals”) to forecast each race. Then they
account for the uncertainty in the forecast and simulate the election thousands
of times. Our models 
[are probabilistic in nature](https://goo.gl/WYd6qc); 
we do a _lot_ of thinking about these probabilities, and the goal is to develop
probabilistic estimates that hold up well under real-world conditions. For
instance, when we launched the 2018 House forecast, Democrats’ chances of
winning the House were about 7 in 10 — right about what
[Hillary Clinton’s chances were](https://goo.gl/1c4nZj) 
on election night in 2016! So 
[ignore those probabilities at your peril](https://goo.gl/c3Jm2z).

I will be using the _FiveThirtyEight.com_ model to collect forecasting data. In
2016, FiveThirtyEight's prediction was closest to reality. They are one of the
few mainstream forecasters to continue their work into the 2018 midterm
elections.

As FiveThirtyEight [explains on their site](https://goo.gl/cx52rv),

> The House, Senate and gubernatorial models use a broad variety of indicators
in addition to polling. With 435 separate House races every other year — plus
races for each of the 100 Senate seats once every six years and each of the 50
governorships at least once every four years2 — it’s possible to make robust
empirical assessments of which factors really predict congressional and
gubernatorial races well and which ones don’t. Nonetheless, our models default
toward using polling once there’s a lot of high-quality polling in a particular
state or district. In Senate and gubernatorial races, most states have abundant
polling. But this is less true in the House, where districts are polled
sporadically and polling can be an adventure because of small sample sizes and
the demographic peculiarities of each district.

FiveThirtyEight provides three versions of their forecast model that use
different amounts of data beyond their adjusted polling aggregation. In their
own words, "you should think of Classic as the preferred or default version of
FiveThirtyEight’s forecast". Their classic model uses data from three sources:

1. **Polling**: District-by-district polling, adjusted for house effects and
other factors.
2. **CANTOR**: A proprietary system which infers results for districts with
little or no polling from comparable districts that do have polling.
3. **Fundamentals**: Non-polling factors such as fundraising and past election
results that historically help in predicting congressional races.
    * Incumbency
    * State partisanship
    * Incumbent previous margins
    * Generic ballot
    * Fundraising
    * Incumbent voting record
    * Challenger experience
    * Scandals

In the training data (most House races since 1998), the classic model correctly
predicted _96.7%_ of races at the time of election. FiveThirtyEight points out
that the _vast_ majority of races are blowouts, inflating this accuracy
percentage. This makes sense, as the model relies heavily on polling, which
becomes more accurate the closer its done to the election. 100 days before the
election, the classic model was a few percentage points worse at predicting the
races in the training data.

### Forecast data

FiveThirtyEight is kind enough to provide all their daily forecast history as
CSV. For example, the house district data can [downloaded from their site](https://goo.gl/pXYvYJ).
I used the same techniques to reformat, using [`forecast_history.R`](code/forecast_history.R) and saving as [`forecast_history.csv`](data/forecast_history.csv).

The following is a random sample of the data after joining together the predictions for both chambers and doing some slight formatting for consistency's sake.


```
## # A tibble: 89,918 x 8
##    date       last      chamber code  party incumbent voteshare  prob
##    <date>     <chr>     <chr>   <chr> <chr> <lgl>         <dbl> <dbl>
##  1 2018-08-01 Sinema    senate  AZ-99 D     FALSE         0.511 0.738
##  2 2018-08-01 McSally   senate  AZ-99 R     FALSE         0.461 0.262
##  3 2018-08-01 Feinstein senate  CA-99 D     TRUE          0.636 0.999
##  4 2018-08-01 Leon      senate  CA-99 D     FALSE         0.364 0.001
##  5 2018-08-01 Murphy    senate  CT-99 D     TRUE          0.641 0.999
##  6 2018-08-01 Corey     senate  CT-99 R     FALSE         0.324 0.001
##  7 2018-08-01 Carper    senate  DE-99 D     TRUE          0.607 0.989
##  8 2018-08-01 Arlett    senate  DE-99 R     FALSE         0.367 0.011
##  9 2018-08-01 Nelson    senate  FL-99 D     TRUE          0.511 0.616
## 10 2018-08-01 Scott     senate  FL-99 R     FALSE         0.489 0.384
## # ... with 89,908 more rows
```

## Prediction markets

> Prediction markets are exchange-traded markets created for the purpose of
trading the outcome of events. The market prices can indicate what the crowd
thinks the probability of the event is. A prediction market contract trades
between 0 and 100%. It is a binary option that will expire at the price of 0 or
100%. Prediction markets can be thought of as belonging to the more general
concept of crowdsourcing which is specially designed to aggregate information on
particular topics of interest. The main purposes of prediction markets are
eliciting aggregating beliefs over an unknown future outcome. Traders with
different beliefs trade on contracts whose payoffs are related to the unknown
future outcome and the market prices of the contracts are considered as the
aggregated belief.

I will be using the _PredictIt.org_ exchange to collect prediction market data.

### About PredictIt

> PredictIt is a unique and exciting real money site that tests your knowledge
of political events by letting you trade shares on everything from the outcome
of an election to a Supreme Court decision to major world events… PredictIt is
run by [Victoria University](http://www.victoria.ac.nz/) of Wellington, New
Zealand, a not-for-profit university, for educational purposes.

Users of this site can legally trade on the exchange with real currency, winning
money if their prediction comes true and their shares sell for $1. Traders can
also sell their shares at any time, provided there is a corresponding buyer; as
public opinion shifts, “Yes” or “No” shares become more or less valuable,
adjusting the probability in real time as new information is introduced into the
market.

The site is exempt from the usual ban on both online and political betting by
working with researchers to study prediction markets as a political tool. In
October of 2014, the Commodity Futures Trading Commission granted the site a
[No-Action letter](https://goo.gl/PAaquy) allowing them to operate in spite of
such laws. The site hosts markets on nearly every conceivable political event,
electoral or otherwise:

* [Will Donald Trump be president at year-end 2018?](https://goo.gl/E1KvPA)
* [Will the federal government be shut down on February 9?](https://goo.gl/zbTbe3)
* [Will Ted Cruz be re-elected to the U.S. Senate in Texas in 2018?](https://goo.gl/c5sVUU)
* [Will Facebook’s Mark Zuckerberg run for president in 2020?](https://goo.gl/yFA2n3)
* [How many tweets will \@realDonaldTrump post this week?](https://goo.gl/bJk9bj)

### PredictIt markets

There are not markets for every midterm election. Traders are only interested in
betting on markets where they think they have a competitive edge over other
traders. The Cook Political Report lists 334 races as safe for either party, 42
as likely, and only 94 as vulnerable. Of those elections with markets on
PredictIt, only 21 are safe, 15 likely, and 77 vulnerable. Over 80% of
vulnerable elections are being traded on the markets, compared to less than 8%
of safe elections.



|            | Democrat         || Republican         || Total         ||
|------------|----------|--------|------------|--------|-------|--------|
| Rating     | Model    | Market | Model      | Market | Model | Market |
| Safe       | 193      | 12     | 141        | 9      | 334   | 21     |
| Likely     | 15       | 7      | 29         | 8      | 42    | 21     |
| Vulnerable | 15       | 13     | 78         | 64     | 94    | 77     |

The overlap in competitive markets allows us to compare the predictive
capabilities of each tool.

### Market data

PredictIt does not provide provide a collected source of data for on-going
markets, but they do provide a 90-day history for each individual market. They
also provide [an application program interface
(API)](https://www.predictit.org/api/marketdata/all/) to access the most recent
data for every market. I wrote three R scripts to collect, format, and join the
market as needed for analysis: (1) [`market_names.R`](code/market_names.R) to
scrape the API for market information, (2) [`market_data.R`](code.market_data.R)
to scrape the chart information for price history, and (3)
[`join_markets_models.R`](join_markets_models.R)
 

```
## # A tibble: 24,556 x 6
##    date       mid   cid   contract                             price volume
##    <date>     <chr> <chr> <chr>                                <dbl>  <dbl>
##  1 2018-08-10 2918  5264  Will Elizabeth Warren be re-elected?  0.95     56
##  2 2018-08-10 2928  5313  Will Ted Cruz be re-elected?          0.7    1303
##  3 2018-08-10 2940  5368  Will Bernie Sanders be re-elected?    0.95    542
##  4 2018-08-10 2941  5369  Will Joe Manchin be re-elected?       0.75    533
##  5 2018-08-10 2998  5563  Will Joe Donnelly be re-elected?      0.39     12
##  6 2018-08-10 3450  7165  Will Pelosi be re-elected?            0.9      51
##  7 2018-08-10 3455  7182  Will Ryan be re-elected?              0.03      4
##  8 2018-08-10 3480  7266  Will Heidi Heitkamp be re-elected?    0.42     81
##  9 2018-08-10 3484  7270  Will Claire McCaskill be re-elected?  0.47    333
## 10 2018-08-10 3485  7271  Will Tammy Baldwin be re-elected?     0.83      0
## # ... with 24,546 more rows
```

## Data Analysis

### Combined Data

Using the `date`, `code`, and `party` as key variables, we can combine the model
and market data with a left relational join. Using the `tidyr` package tools, we
can turn this into tidy data, where each row is 1 predition and there is a new
variable to indicate the predictive tool used.


```
## # A tibble: 45,962 x 8
##    date       name             code  party incumbent voteshare tool    prob
##    <date>     <chr>            <chr> <chr> <lgl>         <dbl> <chr>  <dbl>
##  1 2018-08-10 Martha McSally   AZ-99 R     FALSE         0.461 model  0.272
##  2 2018-08-10 Martha McSally   AZ-99 R     FALSE         0.461 market 0.02 
##  3 2018-08-10 Nancy Pelosi     CA-12 D     TRUE          0.898 model  1    
##  4 2018-08-10 Nancy Pelosi     CA-12 D     TRUE          0.898 market 0.9  
##  5 2018-08-10 Devin Nunes      CA-22 R     TRUE          0.564 model  0.96 
##  6 2018-08-10 Devin Nunes      CA-22 R     TRUE          0.564 market 0.65 
##  7 2018-08-10 Diane L. Harkey  CA-49 R     FALSE         0.467 model  0.197
##  8 2018-08-10 Diane L. Harkey  CA-49 R     FALSE         0.467 market 0.03 
##  9 2018-08-10 Dianne Feinstein CA-99 D     TRUE          0.636 model  0.999
## 10 2018-08-10 Kevin de Leon    CA-99 R     FALSE         0.364 model  0.001
## # ... with 45,952 more rows
```

### Example Data


```r
ggplot(filter(joined_tidy, code == "MA-99" | code == "IN-99" & party == "D")) +
  geom_line(aes(x = date, 
                y = prob, 
                color = tool),
            size = 2) +
  labs(title = "Probabiulity of Victory Over Time",
       subtitle = "Two Incumbent Democrats, Donnelly (lost) and Warren (won)",
       y = "Probability",
       x = "Date") +
  scale_x_date() + 
  geom_hline(aes(yintercept = 0.50)) +
  facet_grid(~code)
```

![](README_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

