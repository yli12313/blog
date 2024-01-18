+++
title = 'Google Finance'
date = 2024-01-09T10:43:53-05:00
draft = false
+++

So [Google Finance](https://support.google.com/docs/answer/3093281?hl=en) is pretty cool. It’s integrated into Google Sheets such that you can get live and historic market data through sheet formulas and the data automatically populates within the Google Sheets. The syntax is:

```excel
GOOGLEFINANCE(ticker, [attribute], [start_date], [end_date|num_days], [interval])
```
I just tried executing the command: `=GOOGLEFINANCE(“MES”, “price”, DATE(2023,1,1), DATE(2023,12,31))` and got the historic Micro E-mini S&P 500 Index Futures for 2023:

![p1](/blog/20240109_Google_Finance/data.png)

The only thing that I think could be improved upon would be to get multiple attributes in tabular form with one command. Right now, you can only get one attribute with one command and that makes it so that you have to write many commands. For a quick way to get financial data, Google Finance integrated into Google Sheets is definitely not bad!

Update: For the historic data, there is an `all` feature that will grab the attributes - Open, High, Low, Close, and Volume.