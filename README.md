The S&P 500 comprises ~ 500 stocks and represents the broad stock market. You have a
hypothesis that families of stocks comprising the index actually belong to a few groups that
move in sync to each other. Intuitively it makes sense that stocks in the same or similar sector
behave similarly but you believe that there might be stocks in not directly related sectors that
behave similarly. You want to explore this hypothesis further.

## Data Source

The composition of S&P500 changes often and for this analysis, the most updated composition is used. The updated list of stocks in S&P 500 are extracted from this [Github Repo](https://github.com/datasets/s-and-p-500-companies/tree/main). This repository runs a scheduled scraper and lists all companies in S&P 500 along with their ticker, GICS Sector and other information.

After getting the list of stocks in S&P500, 2 year market value (Open, Close, High, Low, Volume) data is then retrieved using Yahoo Finance API. 


## Data Wrangling

For each stock we receive multiple market values such as Open (market open price), Close (market close price), High (Daily highest stock value), Low (daily lowest stock value) and Volume (number of stocks trader per day). The average of daily open and close price is used as the daily price for each stock. To avoid any short term volatility in stock prices, rolling mean (10 days) is taken for each stock.

The percentage change between the price of the stock from initial price(Oct 18, 2021) to end of 2 year price were calculated for all stocks in S&P500. The stocks were then assigned to 5 different labels based on their 2 year performance (% change) using quantiles. 

![alt text](https://github.com/swami84/SP500/blob/master/results/Performance%20Bins.jpg)

|     Performance Label    	|                   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2y % Change Bins    ||
|:------------------------:	|:-----------------------:	|:-----------:	|
|                          	|            Min          	|      Max    	|
|        decline-high      	|           -78%          	|     -28%    	|
|        decline-low       	|           -28%          	|     -12%    	|
|            flat          	|           -12%          	|      3%     	|
|           up-low         	|            3%           	|      20%    	|
|          up-high         	|            20%          	|     182%    	|


## Results

Among all sectors Energy sector stocks show the highest gains
and Comm Service sector stocks show the lowest gain. S&P index seems to follow higher weightage sectors – IT, healthcare and Financials.

img_source = TS_Sector

### 2-Year Performance Distribution

Time series clustering was done on the 2 year market data (freq = daily). Each stock’s timeseries was normalized between 0 and 1. 5 clusters were used to match with binned labels earlier identified


img_source = ts_clusters


Gradient Boosted Tree (GBT) model was also developed using the clustering labels as one of the feature (Clustering  + GBT). Overall accuracy of the combined Clustering  + GBT = 74%

img_source = classification_heatmap





