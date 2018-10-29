# predictr

Using R to compare the predictive capabilities of markets and models.

 * [Project](#project)
 * [Prediction markets](#prediction-markets)
   + [About PredictIt](#about)
   + [PredictIt markets](#predictit-markets)
   + [Market data](#market-data)
 * [Forecast models](#forecast-models)
   + [About FiveThirtyEight](#about-fivethirtyeight)
   + [FiveThirtyEight forecast](#fivethirtyeight-forecast)
   + [Forecast data](#forecast-data)

## Project

In recent years, the forecast model has become a staple of political punditry. Popularized by the digital journalism site [FiveThirtyEight](https://fivethirtyeight.com/), the forecast model is a statistical tool used to aggregate and balance data to calculate the probability of an electoral outcome.

On the eve of the 2016 presidential election, all mainstream forecasts gave Hillary Clinton overwhelming odds to win the presidency. FiveThirtyEight's forecast model gave Clinton the lowest odds, [at 70%](https://goo.gl/CLPrUC). Meanwhile, The New York Times Upshot had her at [85%]((https://goo.gl/QES9vJ)), PredictWise [89%](https://goo.gl/1Mnf7m), and the HuffPo Pollster's prediction gave Clinton an infamous [98% chance of winning](https://goo.gl/XJqwyD).

These forecast models represent the best statistical predictions possible. Yet we now know they were all wrong (some of them more than others).

I posit that prediction markets may claim a distinct niche in the field of political predictions. In theory, prediction markets encompass the data generated by forecasting models and crowdsource additional unquantifiable variables. The **efficient-market hypothesis** states that asset prices fully reflect all available information, not just polling samples or mathematical factors.

I will be comparing the ability of markets and models to accurately predict races in the 2018 midterm.

## Prediction markets

> Prediction markets are exchange-traded markets created for the purpose of trading the outcome of events. The [market prices](https://en.wikipedia.org/wiki/Market_price "Market price") can indicate what the crowd thinks the [probability](https://en.wikipedia.org/wiki/Probability "Probability") of the [event](https://en.wikipedia.org/wiki/Event_(probability_theory) "Event (probability theory)") is. A prediction market contract trades between 0 and 100%. It is a [binary option](https://en.wikipedia.org/wiki/Binary_option "Binary option") that will expire at the price of 0 or 100%. Prediction markets can be thought of as belonging to the more general concept of crowdsourcing which is specially designed to aggregate information on particular topics of interest. The main purposes of prediction markets are eliciting aggregating beliefs over an unknown future outcome. Traders with different beliefs trade on contracts whose payoffs are related to the unknown future outcome and the market prices of the contracts are considered as the aggregated belief. _[source: [Wikipedia](https://en.wikipedia.org/wiki/Prediction_market)]_

I will be using the _PredictIt.org_ exchange to collect prediction market data.

### About PredictIt

> PredictIt is a unique and exciting real money site that tests your knowledge of political events by letting you trade shares on everything from the outcome of an election to a Supreme Court decision to major world events… PredictIt is run by [Victoria University](http://www.victoria.ac.nz/) of Wellington, New Zealand, a not-for-profit university, for educational purposes. _[source: [PredictIt](https://www.predictit.org/support/what-is-predictit)]_

Users of this site can legally trade on the exchange with real currency, winning money if their prediction comes true and their shares sell for $1. Traders can also sell their shares at any time, provided there is a corresponding buyer; as public opinion shifts, “Yes” or “No” shares become more or less valuable, adjusting the probability in real time as new information is introduced into the market.

The site is exempt from the usual ban on both online and political betting by working with researchers to study prediction markets as a political tool. In October of 2014, the Commodity Futures Trading Commission granted the site a No-Action letter allowing them to operate in spite of such laws. The site hosts markets on nearly every conceivable political event, electoral or otherwise:

* [Will Donald Trump be president at year-end 2018?](https://www.predictit.org/markets/detail/2939/Will-Donald-Trump-be-president-at-year-end-2018)
* [Will the federal government be shut down on February 9?](https://www.predictit.org/markets/detail/4078/Will-the-federal-government-be-shut-down-on-February-9)
* [Will Ted Cruz be re-elected to the U.S. Senate in Texas in 2018?](https://www.predictit.org/markets/detail/2928/Will-Ted-Cruz-be-re-elected-to-the-US-Senate-in-Texas-in-2018)
* [Will Facebook’s Mark Zuckerberg run for president in 2020?](https://www.predictit.org/markets/detail/2992/Will-Facebook%27s-Mark-Zuckerberg-run-for-president-in-2020)
* [How many tweets will @realDonaldTrump post from noon Oct. 10 to noon Oct. 17?](https://www.predictit.org/markets/detail/4956/How-many-tweets-will-@realDonaldTrump-post-from-noon-Oct-17-to-noon-Oct-24)

### PredictIt markets
There are not markets for every midterm election. However, there _are_ markets for over 100 of the most contested congressional general elections. As of October 19th, the Cook Political Report classifies 105 House races as "competitive".

| Rating | Democrat | Republican | Total |
|--|--|--|--|
| Solid | 182 | 145 | 327 _(75%)_ |
| Likely | 10| 50 | 60 _(14%)_ |
| Toss-up | 3| 45 | 48 _(11%)_ |

These competitive races aren't all represented with markets, but there is significant overlap. Enough to establish a comparison between our different predictive tools.

### Market data

PredictIt does not provide provide a collected source of data for on-going markets, but they do provide a 90-day history for each individual market. They also provide [an application program interface (API)](https://www.predictit.org/api/marketdata/all/) to access the most recent data for every market. I was able to write a short R script to combine these two sources and centralize the relevant market history. This data is scraped, collected, and formatted with [`market_history.R`](code/market_history.R) and is saved as [`market_history.csv`](data/market_history.csv).

    > sample_n(market_history, size = 10)
    # A tibble: 10 x 6
       date       name        id volume price question
       <date>     <chr>    <int>  <int> <dbl> <chr>
     1 2018-09-16 PA-17     4271      4  0.87 Which party will win PA-17?
     2 2018-10-15 NJ-02     3862      0  0.05 Which party will win NJ-02?
     3 2018-07-29 NH-01     3767      0  0.25 Which party will win NH-01?
     4 2018-08-12 MS-03     4023      0  0.1  Which party will win MS-03?
     5 2018-08-19 Donnelly  2998    217  0.42 Will Joe Donnelly be re-elected?
     6 2018-08-30 Manchin   2941    460  0.74 Will Joe Manchin be re-elected?
     7 2018-08-29 FL-27     3738    281  0.86 Which party will win FL-27?
     8 2018-08-19 Nelson    2999      0  0.5  Will Bill Nelson be re-elected?
     9 2018-10-11 SC-04     4106      0  0.1  Which party will win SC-04?
    10 2018-08-28 SC-04     4106      0  0.98 Which party will win SC-04?

## Forecast models

> (Forecasting models) take lots of polls, perform various types of adjustments to them, and then blend them with other kinds of empirically useful indicators (what we sometimes call “the fundamentals”) to forecast each race. Then they account for the uncertainty in the forecast and simulate the election thousands of times. Our models [are probabilistic in nature](https://fivethirtyeight.com/features/a-users-guide-to-fivethirtyeights-2016-general-election-forecast/); we do a _lot_ of thinking about these probabilities, and the goal is to develop probabilistic estimates that hold up well under real-world conditions. For instance, when we launched the 2018 House forecast, Democrats’ chances of winning the House were about 7 in 10 — right about what [Hillary Clinton’s chances were](https://fivethirtyeight.com/features/why-fivethirtyeight-gave-trump-a-better-chance-than-almost-anyone-else/) on election night in 2016! So [ignore those probabilities at your peril](https://fivethirtyeight.com/features/the-media-has-a-probability-problem/). _[source: [FiveThirtyEight](https://fivethirtyeight.com/methodology/how-fivethirtyeights-house-and-senate-models-work/)]_

I will be using the _FiveThirtyEight.com_ model to collect forecasting data. In 2016, FiveThirtyEight's prediction was closest to reality. They are one of the few mainstream forecasters to continue their work into the 2018 midterm elections.

### About FiveThirtyEight

Nate Silver started FiveThirtyEight in 2008 to cover the presidential election. Silver specifically sought to answer the simple question of which democratic primary candidate would fare best against the presumptive Republican nominee. As Silver put it, “someone could look like a genius simply by doing some fairly basic research into what really has predictive power in a political campaign.” The first FiveThirtyEight model was more basic, simply aggregating polls and weighing them using past accuracy. Silver built his forecast model using three guiding principles:

1. **Think probabilistically**. FiveThirtyEight aims to express the full range of possible outcomes and the probability of each.
2. **Today’s forecast is the first forecast of the rest of your life**. The prediction should change to be as good as possible, regardless of past data or ideas.
3. **Look for consensus**. Evidence suggests that aggregates or group forecasts are more accurate than individual ones.

The blog was brought under the New York Times in 2010, ESPN in 2014, and ABC in 2018. When the site was bought by ESPN, they expanded from 2 full time staff to 20. The site covers politics, sports, science & health, economics, and culture.

In the 2012 presidential general election, FiveThirtyEight was able to accurately predict the winner of 50/50 states. In 2016, the presidential forecast model gave Hillary Clinton a 71% chance of winning, significant lower than the likes of HuffPo or the New York Times.

### FiveThirtyEight forecast

As FiveThirtyEight [explains on their site](https://fivethirtyeight.com/methodology/how-fivethirtyeights-house-and-senate-models-work/),

> The House, Senate and gubernatorial models use a broad variety of indicators in addition to polling. With 435 separate House races every other year — plus races for each of the 100 Senate seats once every six years and each of the 50 governorships at least once every four years2 — it’s possible to make robust empirical assessments of which factors really predict congressional and gubernatorial races well and which ones don’t. Nonetheless, our models default toward using polling once there’s a lot of high-quality polling in a particular state or district. In Senate and gubernatorial races, most states have abundant polling. But this is less true in the House, where districts are polled sporadically and polling can be an adventure because of small sample sizes and the demographic peculiarities of each district.

FiveThirtyEight provides three versions of their forecast model that use more or less additional data beyond their adjusted polling aggregation. In their own words, "you should think of Classic as the preferred or default version of FiveThirtyEight’s forecast". Their classic model uses data from three sources:

1. Polling: District-by-district polling, adjusted for house effects and other factors.
2. CANTOR: A proprietary system which infers results for districts with little or no polling from comparable districts that do have polling.
3. Fundamentals: Non-polling factors such as fundraising and past election results that historically help in predicting congressional races.

In the training data (most House races since 1998), the classic model only miscalled 3.3% of races at the time of election. This makes sense, as the model relies heavily on polling, which becomes more accurate the closer its done to the election. 100 days before the election, the classic model was a few percentage points worse at predicting the races in the training data.

### Forecast data

FiveThirtyEight is kind enough to provide all their daily forecast history as CSV. For example, the house district data can [downloaded from their site](https://projects.fivethirtyeight.com/congress-model-2018/house_district_forecast.csv). I used the same techniques to reformat, using [`forecast_history.R`](code/forecast_history.R) and saving as [`forecast_history.csv`](data/forecast_history.csv).

    > sample_n(forecast_history, size = 10)
    # A tibble: 10 x 9
       date       candidate         chamber state district party incumbent voteshare probability
       <date>     <chr>             <chr>   <chr> <chr>    <chr> <lgl>         <dbl>    <dbl>
     1 2018-09-26 Will Hurd         house   TX    23       R     TRUE          0.49  0.519
     2 2018-08-03 Kendra Horn       house   OK    05       D     FALSE         0.473 0.244
     3 2018-10-04 David Young       house   IA    03       R     TRUE          0.477 0.468
     4 2018-08-02 Liz Matory        house   MD    02       R     FALSE         0.301 0.000300
     5 2018-09-26 Martin Heinrich   senate  NM    00       D     TRUE          0.523 0.993
     6 2018-08-04 Nancy Soderberg   house   FL    06       D     FALSE         0.474 0.259
     7 2018-08-09 Michael T. McCaul house   TX    10       R     TRUE          0.569 0.977
     8 2018-10-09 William Tanoos    house   IN    08       D     FALSE         0.389 0.0055
     9 2018-09-26 Mike Braun        senate  IN    00       R     FALSE         0.463 0.241
    10 2018-08-25 John P. Sarbanes  house   MD    03       D     TRUE          0.671 1.000
