# Jacobi Data Engineering Technical Test
# Data Wrangling Topic

## Background

You are a data engineer at a small company called Euler, and you work with financial data. Primarily you are involved in maintaining lists of securities for clients, where securities trade on Stock Exchanges. One of your clients, "Strong Oak Security Management" has realised they need to outsource their security management, and they admit that their data is quite the mess. At Euler you have a platform to manage securites, so your job is to take their list of securities, and load it into Euler's proprietary security management platform.

This load is a multi-step process, firstly identifying valid securites, then uploading attributes for these, and then answering questions provided by the client. Securities may not be valid for numerous reasons such as no longer being traded on an exchange, and we are unable to load these securities. Ensure you read through this full document, including the notes section before beginning, and understand the requirements for each section.

## Loading Securities into the platform

To load the securities into the platform you need the following identifiers:
`MIC, QUEUESIP, Symbol, RequestId` where one of `QUEUESIP` or `Symbol` must be populated. It is important to include `MIC` as part of the identifiers, as the same security listed on different exchanges may return different results when querying data vendors.
Each of these will be assigned an `EulerId`, which should be an iterator starting at 1, that is the first security uploaded will be 1, the second 2, and the *n*th security designated n.
An example csv upload file would be:

```csv
EulerId,MIC,QUEUESIP,Symbol,RequestId
1,XNYS,0x5f4120,ABB,654
2,XNAS,,BGT,2045
```

We want to maximise the number of securities we can upload, but we cannot upload a security without one of `QUEUESIP` or `Symbol`. i.e., `QUEUESIP` may be null as long as a `Symbol` is provided.

* The client wants to minimise the number of null values in `QUEUESIP` and `Symbol`.

* Provide us with your generated upload in a csv format, named `{firstName}_{lastName}_section1.csv`.

* Files required: `stock.data`, `exchange.data`, `strong_oak_security_master.csv`.

### Notes

Consider the following (without needing to communicate these in your response):

* What are some issues you encountered in this process and how did you overcome them?
* How many merges did your solution require?
* Did you find many false positive matches that you had to filter out?

## Uploading Attributes

Next, relevant attributes need to be uploaded. For explicitness, the platform requires these to be uploaded in *long format*. The exact format to be uploaded requires 3 columns: `EulerId, AttributeName, AttributeValue`. There should be no null values in the `AttributeValue` column. An example csv upload considering a subset of the attributes would be:

```csv
EulerId,AttributeName,AttributeValue
1,Asset Class,International Equity
1,Inception Date,2010-08-09
1,Return Since Inception,0.0664140167
2,Asset Class,International Equity
2,Return Since Inception,-0.0746303434
```

The attributes the client want are "Asset Class", "Inception Date", "Exchange Name", "Exchange Location", "Security Name", "Strong Oak Identifier", and "Return Since Inception".
This data should be sourced from the several `.data` files, and the `EulerId` should correspond to the values generated from the first response.

* Exchange location should be given as the combination of Exchange Country and City (i.e. `{country} - {city}`).

* Provide us with your generated upload in a csv format, named `{firstName}_{lastName}_section2.csv`.

* Files required: `attributes.data`, `exchange.data`, `{firstName}_{lastName}_section1.csv` (or just use the outputted data from section 1)

## Notes and Comments

* We recommend using any pandas however you should prioritise correctness of your responses over utilising pandas.
* RequestId is a unique value that is used to match to the data vendor (the supplier of this data, where data is collected from on a daily basis to maintain the securities)
* Each row in the Stock List is a single security, and will have a unique QUEUESIP and Symbol
* Ticker and Symbol are the same thing
* Ensure that code is well commented, and use docstrings where appropriate
* Code cleanliness will be considered. It should be clear where the solution to section 1 ends and section 2 begins for example.
* All data required is provided in `/data` however, you are welcome to access the internet to better understand the data set.
* QUEUESIP is a made up identifier.
