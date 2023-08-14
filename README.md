# Dividend Roth Analysis!
This notebook gets quote/dividend data from the Yahoo Finance API and runs analysis on dividend returns for different securities.

## Setup
```sh
git clone git@github.com:ntbanks/dividends.git && cd dividends
python3 -m venv .env && source .env/bin/activate && pip install -r requirements.txt
jupyter notebook
```

## Code Breakdown

### get_ticker_data
Uses the `stock_info` module of the python package [yahoo_fin](https://theautomatic.net/yahoo_fin-documentation/) to return market data for a security from the Yahoo Finance API. The module has a function to return daily quote data (`get_data`) and another to return past and scheduled dividend payouts (`get_dividends`). 

**Params:**
- ticker (string): 2-5 character ticker representation for security

**Usage:**
```python
market_data = get_ticker_data('ibm')
```

### get_analysis
This returns a dataframe that is essentially the same as the main table in the "Stock#_" sheets in the excel file. Keeps a running total of the stocks purchased either from DRIP or incremental investments. 

**Params:**
- ticker (string): 2-5 character ticker representation for security
- buy_date (date): date of the initial investment
- inc_start_date (date): date to start incremental investments
- end_date (date): simulation sell date
- initial_amt (int): dollar amount of initial investment
- inc_amt (int): YEARLY dollar amount of incremental investments
- freq (string): frequency of incremental investments
  - options: 'annually', 'biannually' or 'quarterly'
  - optional | default: 'quarterly'

**Usage:**

```python
ibm = get_analysis('ibm', date(2013, 2, 6), date(2013, 2, 6), date(2023, 1, 26),  5000, 6000, 'annually')

mdt = get_analysis('mdt', date(2013,1,25), date(2018,2,20), date(2023, 8, 11), 15000, 6000)
```

### get_summary
Returns a Series with aggregated summary metrics describing the returns for a given security. 

**Params:**
- df (pd.DataFrame): the output of `get_analysis`
- display name (string): whatever you want the displayed name to be

**Usage:**
```python
altria = get_analysis('mo', date(2013, 2, 6), date(2013, 2, 6), date(2023, 1, 26),  5000, 6000, 'annually')
summary = get_summary(altria, "Altria (MO)")
```
