# News Sentiment & Stocks: Quantifying Optimism During a Pandemic 
## 2021 Senior Thesis
<br>
This repository contains all the code and datasets written and created during my undergraduate thesis. Including, but not limited to, news data collection from the New York Times and Guardian APIs, Stock price retrieval from yahoo finance, VADER sentiment analysis of headlines, ML stock classification model. See README file for more details. 

## Overview

This study aims to investigate whether narratives proliferated by large media outlets track strongly with price developments in financial markets. During the year 2020, a massive reflation in equity prices followed their sharp decline in response to the COVID-19 pandemic. There have been two narratives largely associated with this rebound in asset prices-- one of extreme monetary and fiscal stimulus, another of an effective vaccine that would ultimately replace lock-downs as a combative measure against the disease. While both narratives are incredibly nuanced and display an extreme level of complexity, they still are propagated by an underlying, mass psychology that is increasingly quantifiable using various computational approaches. Because the transmission of ideas is remarkably similar to that of viral diseases, news headlines will be used as a proxy to quantitatively evaluate the degree of these narratives' dissemination (Shiller 2017). To better understand how these ideas were transmitted on a global scale, and because the sheer volume of headlines published each day far exceeds the amount any human could read on their own, this paper will rely on Natural Language Processing Library VADER to evaluate each narrative's dissemination. 

Similar approaches to sentiment analysis are wildly popular both in academic research and private enterprise. Natural Language Processing's popularity has only been magnified by the increasingly-accessible, digital stores of historical headlines and social media posts. Moreover, in 2018, a study estimated that nearly 70% of trading on US exchanges was algorithmic (Hilbert 2020). This number is only expected to have grown in the last three years. Automated, artificial processing of financial news is now a viable alternative approach to manually reading news articles. Social media has also transformed the way people consume news and, generally, information. According to another study conducted in 2018, 55% of surveyed adults over 30 reported getting their news from social media, which was twice as many as the number reported in 2010 (Kohut 2012). In consideration of these trends, it is crucial to establish i) how well popular NLP libraries extract sentiment from news headlines ii) And secondly How the two narratives compete as explanations for the rebound in stock prices. This will be done by employing economic and computational models to analyze thousands of filtered headlines.

## Data
### News Data

Headlines from the New York Times and Guardian were selected as the main data source in this project for several reasons. First and foremost, both publishers provide public API's with both high call limits and very extensive online documentation. While more robust, subscription API's are available and often used by large commercial enterprises for similar purposes, both the New York Times and Guardian are reliable world class new sources with millions of readers worldwide. Their readers reflect a finance-savvy, college educated subset of the population who is probabilistic more likely to be involved in financial markets. Over 56% of The New York Times' readers and 65% of Guardian readers both hold a college degree or higher, which does not include those currently pursuing a degree (Kohut 2012). 38% of The New York Times' readers earn at least $75,000 per year while Guardian readers were found to be 24% more likely to hold a mortgage and 32% more likely to have stocks and shares than the average UK citizen. It was also reported that 68% of Guardian readers are over the age of 35. Aside from these demographics being slightly skewed towards being more financially well-off and older, the gender breakdown is very balanced. 52% of New York Times' readers are male while 48% are female. Similarly, around 56% of Guardian readers are male while 44% are female. These statistics suggest that the subscribers to these news sources may be more inclined to follow international, financial news. Given these demographic characteristics, it's reasonable to conclude each source may cater to its educated base by publishing news stories in line with the common narratives propagated among these groups. 

Financial news headlines are an ideal medium to study the transmission of economic narratives due to their concise delivery of complicated economic developments encoded in simple phrases and keywords. Headlines are also a unique target for sentiment analysis and classification because they tend to adhere to professional standards and are fairly uniform in size and term usage. Unlike tweets, headlines are carefully crafted and controlled to match their networks' presiding opinions. The resulting strings of words tend to be succinct, while still reflecting an aggregate of professional viewpoints from editors and journalists, whereas tweets, or other social media posts, tend to solely reflect their respective author's. Tweets are also far more susceptible to taking extreme or radical viewpoints that do not align with any prevailing paradigm. Perhaps an even more-compelling reason for the use of headlines, however, is their tendency to embody a degree of sensationalism. Because publishers rely on total impressions for their revenue, headlines are often designed to be eye-catching, perceived as relevant, and align with a prevailing ideology that suits their patronage. This results in frequent usage of buzzwords and other time-relevant phrases that quickly catch the eye of prospective readers. As a direct result, headlines exhibit extremely strong semantic patterns when grouped by topics and charted over time-- thereby facilitating the perfect environment for an in depth analysis of economic narratives. 

To ensure a sufficient sample size was acquired, for both vaccine and stimulus-related headlines, API requests were made collecting historical data over a 15 month period beginning on January 1st 2020 and ending on March 5th, 2021. March 5th was chosen as the cut-off date because it represents a significant inflection point in both vaccine development and monetary stimulus measures. On March 5th Moderna announced its pursuit of a COVID-19 vaccine. Shortly thereafter, on March 15th, 2020 in response to the pandemic-devastated financial markets, the US Federal Reserve announced emergency large-scale easing programs aimed at preserving the fragile flow of credit in the US economy. This included cutting the Federal Funds Rate to zero, quantitative easing, relaxing certain capital requirements, and  several other measures to resuscitate a contracting economy. Early March marked a potential starting date for the reflation narratives, and will be further referred to as such. 

API's, or application programming interfaces, are fantastic ways to collect both historical and real-time data. The high institutional demand, however, creates a very lucrative market for them, which can present challenges to unfunded developers. Initially, despite most of the later analysis being completed using Python 3.9.2,  R 4.0.5 was used to facilitate all API calls using the jsonlite package. The data retrieved via API calls was filtered via built-in query endpoints to subset headlines only relating to vaccines or stimulus news. Each query endpoint returned articles so long as they contain the specified query keyword in the article's body or headline. Vaccine and stimulus headlines were gathered separately to facilitate a later comparison between the two sets of headlines. Although both API's used in this project are free, to prevent the abuse of their interfaces, the New York Times sets a limit of 4,000 requests per day, whereas the Guardian limits requests 5,000 per day. In order to avoid exceeding these call limits, data was methodically collected over 8 days. Each API conveniently returns data in chronological order, thus permitting the continuous retrieval of headlines over several days. 13,780 headlines were gathered in total between January 1st, 2020 and March 5th, 2021. 5,931 of these headlines were  stimulus-related, while 7,849 belonged to the vaccine-relevant data-set. Each resulting data set contained article headlines and the date and time they were published. Among the two groups of headlines, there were strong semantic patterns that reflected the target themes. **Table 1** displays five randomly selected headlines from each data-set. 

Vaccine-related Headlines| Stimulus-related Headlines
-------------------------|---------------------------       
US pharma company raises vaccine hopes but more trials are vital, say experts | Unemployment Claims Remain High as Millions Still Struggle to Find Work
AstraZeneca, Under Fire for Vaccine Safety, Releases Trial Blueprints | Stimulus Deal Falters as McConnell Signals Republican Resistance
China Approves Covid-19 Vaccine as It Moves to Inoculate Millions | Congress Passes Huge Coronavirus Relief Bill
New Pfizer Results: Coronavirus Vaccine Is Safe and 95% Effective | Put the Money Printer on Autopilot
Russia says data on Sputnik Covid vaccine shows 95% efficacy | At Long Last, a Stimulus Nears

### Financial Data 
While the larger reflation narrative has been widely attributed to extreme monetary stimulus and the rapid development of a COVID-19 vaccine. Certain sectors in the economy have rebounded asymmetrically well compared to others such as small businesses or other contact-based businesses worst affected by the nation-wide lock-downs. To better understand these asymmetries, it is important to understand the degree to which these narratives played in determining market outcomes in each sector. Thus four stock indices were selected and examined, each chosen to represent different sectors of the economy. The following were chosen:
    
* NASDAQ: Has a historical record of tracking with the US economy since it was founded in 1971. 
* S&P 500: Contains 500 stocks, chosen based on market size, liquidity, and industry, which together represent leading industries within the U.S. economy. 
* Russell 2000 : Reflects 2,000 of the smallest publicly-traded U.S. companies based on market capitalization.
* NASDAQ Biotech Index:  Contains NASDAQ-listed biotech and pharmaceutical companies.

Historical stock data was downloaded from Yahoo Finance for the same date ranges as headlines (March 5th, 2019 - March 5th 2021). The data included date, open price, high, low, closing price, adjusted closing price and trading volume. Only date, opening price, adjusted price, and volume were saved for analysis, while high, low and regular closing price were ignored. The adjusted closing price was used instead of the regular close as a more accurate pricing strategy. The adjusted close is essentially the closing price after all cash/stock dividends and stock splits have been taken into account. Because all assets used were indices comprised of hundreds of individual stocks, it was crucial to use the adjusted close as the true price of the index after these dividends and splits had been factored out. 

## Data Filtering

### Dates

The scope of this study was limited to observing longer-term price developments in context of the larger, macro-economic picture, and thus drastically less concerned about capturing the high-frequency, intraday price movements following news releases. The dynamics of news releases and high-frequency price responses have, however, been covered extensively in the literature, particularly by economists Busse and Green in the early 2000's (Busse 2002, Green 2004). Both studies found that asset prices respond in a matter of seconds to critical news releases. For this project, however, daily frequencies were sufficient to observe the amassed reaction to news releases across several indices and macroeconomic implications. However, initially, because markets are not open on weekends or during national holidays, news and stock data did not have the same frequencies. This was because all four indices are based in the United States and do not trade on weekends or national holidays. News data on the other hand, flows unrestricted, everyday, 24 hours a day. In the data obtained, thousands of articles were either published outside of trading hours or on days on which financial markets were closed (i.e National holidays or weekends). It would be foolish to discount any news simply for being published outside of trading hours. Thus a new date was computed for each article that corresponded to the next trading day-- following Busse's conclusion that price responds almost instantaneously to news (Busse 2002). This new date was only used if an article was published outside of regular trading hours. So if a headline was published stating Astrazeneca vaccine trials were halted on a Tuesday evening at 8pm for instance, that headline would be assigned a new date corresponding to the following day, with the assumption that it would impact markets after open the next morning. For headlines published on Saturday or Sunday, their date was adjusted to match the following Monday's date. The same method was applied to the holidays: New Year's Day, MLK Jr. Day, Presidents Day, Memorial Day, Independence Day, Labor Day, Thanksgiving Day, and Christmas. Several other steps were taken before the data was merged by the new date  to the corresponding financial data. While R was used extensively in the scraping and cleaning of the data, python was used during the remainder of the project because of it's powerful Natural Language Processing libraries.  

### Sentiment Analysis

Sentiment analysis has exploded over the past ten years in popularity, and is now frequently cited in the literature. What is meant by sentiment? Before proceeding, it is important to explain what sentiment is and how it could relate to prices. Investor sentiment can broadly range from and expectation of belief about the future price and risks of an asset that in not justified by the current set of facts \cite{baker2007investor}. When it comes to text-sentiment libraries, there are countless, highly-sophisticated libraries to choose from. Because headlines tend to be short in length, the sentiment analysis tool VADER (Valence Aware Dictionary and sEntiment Reasoner) was used to score the rating of each headline.  VADER is one of the more powerful Natural Language Processing libraries, and is trained on millions of short isolated texts. The library is also very commonly implemented in algorithmic trading strategies for its speed and versatility\cite{guidi2017extracting}. VADER is designed to measure both the intensity and sentiment of short, concise sentences. The mathematical derivations of sentiment scores will, however, be further discussed in the time-series analysis section of this methodology. It is most commonly used in analyzing tweets and online reviews, both of which tend to be comparable in length to news headlines. This characteristic is often cited as a reason why VADER also performs well on news headlines. And similar applications have been well-documented in the literature\cite{guidi2017extracting, schumaker2012evaluating}. While a large fraction of the literature on the subject favors automated,  machine learning models, there is still considerable insight that can be gained from using simpler, descriptive approaches to sentiment analysis. These techniques are elegantly laid out by Guidi in his 2017 paper on tweets and stock price prediction \cite{guidi2017extracting}.

VADER measures the intensity and polarity of texts. It provides three polarity scores: A positive, negative, and neutral score where neutral is the complement to the sum of negative and positive scores. Each score is constrained between -4 and 4. VADER then computes a compound score which is the normalized sum of its positive and negative scores, shown in equation \ref{eq:VADERnormalization}, where x is the sum of valence scores. 
\begin{equation}
    x = VADER_{positive} + VADER_{negative}   
\end{equation}
The default normalization constant $\alpha = 15$ was used in this experiment, as recommended by the package. Thus the normalization equation applied in this experiment is shown in equation \ref{eq:VADERnormalization}.    
\begin{equation}
    x = \frac{x}{\sqrt{x^{2} + 15}}
    \label{eq:VADERnormalization}
\end{equation}
Table \ref{tab:VADER} shows VADER's compound scoring system in action on a sample of 8 headlines. While VADER extracts some of the relevant sentiment, it is immediately apparent VADER has a limited comprehension of the complicated pandemic context. However, VADER does do an exceptional job identifying the intensity and polarity of vaccine headlines.
\begin{table}[]
    \centering
    \begin{tabular}{lr}
    \toprule
    \textbf{Neutral:}\\
    \midrule
     Coronavirus Variant Is Indeed More Transmissible, New Study Suggests \\
     Compound Rating: 0.0\\
     
     Becoming a Part of the Story \\
     Compound Rating: 0.0 \\
     
    \toprule
    \textbf{Positive:} \\
    \midrule
     New Pfizer Results: Coronavirus Vaccine Is Safe and 95\% Effective \\
     Compound Rating: 0.718 \\
     
     The surge in coronavirus cases is helping speed up the development of vaccines. \\
     Compound Rating: 0.296 \\
     
     A day of hope, and warning, as a vaccine is rolled out in the U.S. \\
     Compound Rating: 0.128 \\
     \toprule
    \textbf{Negative:} \\
    \midrule
     How Bad Will the Coronavirus Outbreak Get? Here Are 6 Key Factors \\
     Compound Rating: -0.542 \\
     
     America and the Virus: 'A Colossal Failure of Leadership' \\
     Compound Rating: -0.511 \\
     
     Russia Is Slow to Administer Virus Vaccine Despite Kremlin's Approval \\
     Compound Rating: -0.372 \\
    \end{tabular}
    \caption{VADER Compound Sentiment Scores}
    \label{tab:VADER}
\end{table}

Although using the compound normalized sum of polarities is a common and easy way to obtain a snapshot of the overall sentiment in a given time period, it would be insufficient to discard the individual negative and positive ratings. This is for the simple reason that what might be good news for one index is not necessarily good for another. The set of stock indices chosen requires we consider the partial effects of both negative and positive news in addition to the normalized sum of the two. That said, neutral scores are also relevant to this experiment, perhaps best understood as a measure of no news. By considering each of VADER's negative, positive, compound, and neutral scores different hypotheses can be tested. For instance, is no news good news? Is negative vaccine news worse for biotech stocks? Does positive sentiment around monetary and fiscal stimulus lead to price increases? 

## Textual Analysis

Removing stop words and lemmatization are two popular methods for extracting the significant words in text. To get an understanding of the text data and the common trends between narratives, the text data was first filtered. Stop words were removed and stemming was conducted to consolidate important terms that occurred most frequently in the data. Stop-words are the often-meaningless, yet frequent words that show up in text data (i.e. the, he, is, and etc.). Lemmatization reduces the amount of unique words by extracting the root term. For example, the two terms ``vaccines" and ``vaccinate" both refer to the same root ``vaccin" but without first stemming will be counted as two separate terms. The Lancaster stemming method, included in the Natural Language Took Kit Library (NLTK), is one of the more aggressive and was used in this textual analysis to group terms based on root. 


Table \ref{tab: Freqs} shows the top-15 most frequent words in each of the two data sets after removing stop-words and stemming. The emphasized terms are uniquely more common to that narrative. Among the stimulus headlines, the roots ``econom", ``stimul", ``busy", and ``market" all appear in the top 15 most common roots. This proves that the filtering during data collection was successful in collecting headlines relevant to the targeted narrative. Similar patterns can be identified in the vaccine news data, where roots ``vaccin", ``cas" (as in cases), ``lockdown", and ``test" (as in testing) all appear in the top-15 most frequent terms. It's important to point out a few anomalies unique to this data set. You'll notice that the root ``hap" appears in the top-15 terms for both news groupings. The root ``hap" actually corresponds to the term happened. This is because both the New York Times and Guardian publishes several recurring, weekly summaries called ``As it Happened?" and ``How it happened". It's crucial to point out that these two lists do not provide any context relevant to the financial data, yet table \ref{tab: Freqs} still provides a useful overview of the semantic patterns in the news data. 

\begin{table}[]
% TK Bold rows with unique terms 
    \centering
    \begin{tabular}{ cc }
        Stimulus News & Vaccine News \\ 
        \centering
        \begin{tabular}{lr}
        \toprule
              Word &  Frequency \\
        \midrule
         coronavir &        821 \\
               hap &        576 \\
            \textbf{econom} &        \textbf{452} \\
             trump &        446 \\
             covid &        291 \\
             brief &        265 \\
            \textbf{stimul} &        \textbf{259} \\
               \textbf{fed} &        \textbf{255} \\
                us &        246 \\
               new &        244 \\
              \textbf{busy} &        \textbf{232} \\
               say &        224 \\
            \textbf{market} &        \textbf{175} \\
               \textbf{vir} &        \textbf{169} \\
            pandem &        168 \\
        \bottomrule
        \end{tabular} &
        \centering
        \begin{tabular}{lr}
            \toprule
                  Word &  Frequency \\
            \midrule
             coronavir &       1261 \\
                 covid &        856 \\
                   hap &        843 \\
                \textbf{vaccin} &        \textbf{750} \\
                 trump &        438 \\
                    uk &        356 \\
                    us &        338 \\
                   new &        326 \\
                   \textbf{cas} &        \textbf{311} \\
                 brief &        305 \\
                   say &        298 \\
              \textbf{lockdown} &        \textbf{220} \\
                  \textbf{test} &        \textbf{167} \\
                pandem &        158 \\
               \textbf{austral} &        \textbf{157} \\
            \bottomrule
            \end{tabular}
    \end{tabular}
    \caption{Top 15 Most Frequent Words}{} \\
    \label{tab: Freqs}
\end{table}

%\textit{TK Maybe I should Consider creating two tables-- eliminating words that are contained in both frequency tables and look at the unique trends between data sets (i.e remove covid/coronavirus/trump/US/UK)}


## Time Series Analysis



### Detrending Time Series Stock Data
Because the focus of this project is primarily on determining which narrative has more explanatory power and not on predicting specific prices, the historical price data was detrended to maximize the relevant signal. Time series data poses several challenges, one of which is noise or cyclical trends that tend to obscure the underlying signal from being studied. The non-stationary, financial time-series data in this project were very noisy. 2020 was after all a year with record volatility levels. Fortunately, there are well established methods for detrending economic variables with heavy cyclical components. One especially popular method is the Hodrick-Prescott filter. This detrending method effectively decomposes a time series $y_t$ into a cyclical component $\zeta_t$ and trend $\tau_t$, shown below \cite{hodrick1997postwar}. 
\begin{equation}
    \begin{aligned}
    y_t = \zeta_t + \tau_t, 
    \hspace*{.5cm}
    t = 1, 2, ... , T
    \end{aligned}
\end{equation}
The components are derived by minimizing the quadratic loss function in equation \ref{eq:Hodrick-Prescott}, which minimizes the predictive accuracy lost from the model. The first term in the equation minimizes the variance of $\zeta_t$, and coefficient $\lambda$, or smoothing factor. The function simultaneously punishes a lack of smoothness in the second term. It's important to note that as $\lambda$ converges to zero, the trend $\tau_t$ becomes equal to the original time series $y_t$. Conversely, as $\lambda$ diverges to $\infty$, $\tau_t$ becomes linear. 
\begin{equation}
    \begin{aligned}
    \label{eq:Hodrick-Prescott}
    \min_{\tau_t} \sum_{t}^{T}\zeta_t^{2} + \lambda\sum_{t=1}^{T}{[(\tau_t - \tau_{t-1}) - (\tau_{t-1} - \tau_{t-2})]^{2}}
    \end{aligned}
\end{equation}
Traditionally a smoothing factor $\lambda = 1600$ is reserved for macro variables with quarterly data \cite{maravall2001time}. Because the stock market contains cyclical patterns similar to those of other macro variables, the Hodrick-Prescott filter is still a viable approach using the smoothing factor $\lambda = 1600$. Stock data tends to have a larger cyclical component than the business cycle too, which further justifies the use of such a low smoothing factor \cite{adam2019stock}. For these reasons, not only can the Hodrick-Prescott filter be applied, but also does an excellent job of removing the noise and cyclical component while preserving the underlying signal. Figure \ref{fig:SPXhp} shows this detrending in action for the S\&P 500. The same procedure and smoothing factor was used on all indices, the S\&P 500 is just one visual example. In figure \ref{fig:SPXhp}, the signal extracted retains certain features of the original series, yet is also dramatically smoother. Using this filtered time series, the financial news immediately had stronger relationships with the news sentiment scores. 
\begin{figure}[H]
    \centering
    \resizebox{1\textwidth}{!}{\input{./Figures/SPY_hp_visualizations.pgf}}
    \caption{Detrending Financial Data}
    \label{fig:SPXhp}
\end{figure}



### The Dependent Variable
Because each financial time series data was non-stationary and had its own unique mean and variance, the dependent variable needed to be something comparable between assets. In alignment with Atkins' work predicting stock volatility and price using financial news, in which he concluded news data to be a very poor predictor of closing price, this study will take a similar approach and focus on predicting directional movement. A discrete binary variable indicating if the stock increased on a given day, was calculated. Denoted by $ P(Asset\:Increased)$, the dependent variable was equal to 1 if the stock did increase and 0 if not. This variable was based on the percent change calculated using the open and adjusted closing prices during regular trading hours after detrending the time series. If the stock price closed higher than it opened, after dividends and splits had been filtered out, then $P(Asset\:Increased = 1)$. Logistic regressions have interesting interpretations, namely the dependent variable becomes a probability distribution function that follows a Bernoulli distribution with probability $p_t$. One implication of using a linear probability model, however, is that the Gauss-Markov assumption of homoskedasticity quickly fails. Yet logistic models are still quite powerful way to study binary outcomes. The conditional expectation and conditional probability can then be written as 

\begin{equation}
    E_{t-1}(y_t) = P_{t-1}(y_t = 1) = p_t
\end{equation}

	After computing this discrete variable, indices were compared based on proportion of increases to decreases. Figure \ref{fig:dist} shows the distribution of days based on whether or not the index closed higher than it opened. This plot reveals key characteristics of each individual index that will be discussed later. Notice, however, that both the Russel 2000 and NASDAQ Biotech Index have considerably less up-days than their coutnerparties.
\begin{figure}
    \centering
    \resizebox{0.75\textwidth}{!}{\input{./Figures/Trend Distribution.pgf}}
    \caption{Distribution of Negative \& Positive Trading Days Per Asset}
    \label{fig:dist}
\end{figure}



### Cointegration
Spurious correlation is a common issue  encountered in econometric models that deal with one time-series regressed on another. Spurious correlation often leads to incorrect causal interpretations from test statistics because either the two series are coincidentally correlated over time or correlated with some unknown third variable. When this is the case, the two functions also tend to be stationary, only different by some factor $\beta$ \cite{wooldridge2016introductory}. Thus, before proceeding to complete any regression, the time-series were tested for cointegration. 

One popular test for cointegration is the the Augmented Engle-Granger Two-step Test. This testing method first introduced in 1987 works by applying the Augmented Dickey-Fuller (ADF) unit root test \cite{engle1987co, durbin1950testing}. The Engle-Granger method determines whether two times series variables are cointegrated by first obtaining the residuals from a simple linear regression, $\hat{u}_t$, then by carrying out an ADF root test. This test can very powerfully be augmented by including additional lags of $\hat{u}_t$ to account for any serial correlation, but will not be for this experiment. The Engle-Granger hypotheses are typically written as: 
\begin{center}
            $H_0:$ \textit{No Cointegration Exists between x and y}
    
            $H_1:$ \textit{Cointegration Exists between x and y} .
\end{center}
Conveniently, the Python library Stats Models includes a method for conducting Engle-Granger tests. The Stats Models method returns t-statistics and critical values from the ADF root test. Using this library, sentiment scores and the discrete variable $P(Asset Increased)$ were checked for cointegration. Table \ref{tab:coint} shows the results from the Engle-Granger test. 
\begin{table}[H]
    \centering
    \begin{tabular}{@{\extracolsep{5pt}}lcccc}
        \cr
        \toprule
          Index &  Positive  &  Negative  &  Compound  & Critical Values \\
        \midrule
         NASDAQ &               -2.82         &               -2.86        &               -2.85        &                                 -3.93, -3.36, -3.06 \\
            SPY &                  -3.97$^{***}$ &                 -3.47$^{**}$ &                 -3.41$^{**}$ &                          -3.93, -3.36, -3.06 \\
            RUT &               -3.03         &               -2.94        &               -2.75        &                                 -3.93, -3.36, -3.06 \\
            NBI &                 -3.47$^{**}$  &                 -3.86$^{**}$ &                 -3.56$^{**}$ &                           -3.93, -3.36, -3.06 \\
        \bottomrule
        \hline
        \hline \\[-1.8ex]
        \textit{Note:} & \multicolumn{4}{r}{$^{*}$p$<$0.1; $^{**}$p$<$0.05; $^{***}$p$<$0.01} \\
        \end{tabular}
    \caption{Engle-Granger Test for Cointegration}
    \label{tab:coint}
\end{table}
The  dependent variable $P(Asset\: Increased)$ was cointegrated with sentiment for the S\&P 500 and NASDAQ Biotech Index according to the results from the Engle-Granger test of cointegration. This cointegration is also visible in figure \ref{fig:coint}. There was also a considerable level of cointegration in the other indices, although not statistically significant at the 10\% level. Thus, to err on the side of caution, the first difference in all four time series was taken to avoid possibly obtaining inflated test statistics due to any cointegration. 
\begin{sidewaysfigure}
    \centering
    \resizebox{1.15\textwidth}{!}{\input{./Figures/cointegration.pgf}}
    \caption{Sentiment Daily-Total Scores and the $P(Asset Increased)$}
    \label{fig:coint}
\end{sidewaysfigure}



### Taking First Difference
Because the two time series data are cointegrated, meaning they are upward or downward trending together, there is a possibility that the apparent correlation may be spurious or false. Differencing is a method of transforming a time-series data to rid of any cointegration with another time series. The first difference, or lag-1 difference, is when the difference is taken between each consecutive observation \cite{wooldridge2016introductory}. When the first difference of a binary variable is taken however, the dependent series is no longer binary but rather ranges from -1 to 1. Although, the dependent series' variances still fail the Gauss-Markov assumption of homoskedasticity \cite{wooldridge2016introductory}. Thus, in absence of this Gauss-Markov assumption, a feasible Generalized Least Squares (GLS) regression was used in placement of ordinary least Squares (OLS). Because the data has a time series dimension, the model used in the feasible GLS is defined as:
\begin{equation}
    P_{t-1}(y_t = 1) = \alpha_0 + \beta_{1}Pos_t + \beta_{2}Neg_t + \beta_{3}Neu_t + \beta_{4}Comp_t + u_t
\end{equation}
Where $Pos_t$, $Neg_t$, and $Neu_t$ are the positive, negative, and neutral VADER sentiment ratings respectively and $t$ is one business day. $P_{t-1}(y_t = 1)$ is a binary variable indicating whether the filtered trend increased during the period $t$. After taking the lag-1 difference, the model becomes
\begin{equation}
    \Delta y_t =  \theta P_{t-1}(y_t = 1)  + e_t .
\end{equation}
A feasible GLS regression was then used to estimate coefficients $\theta$ because it is the best linear unbiased estimate under the presence of heteroskedasticity \cite{wooldridge2016introductory}. 


