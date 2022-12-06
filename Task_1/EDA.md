---
title: "Data Quality Report"
output:
  html_document:
    keep_md: true
    theme: simplex
    highlight: pygments
    toc: true
    toc_float:
      collapsed: true
      smooth_scroll: true
    toc_depth: 4
    df_print: paged
    code_folding: hide
---









Hello

This e-mail was sent to you in order to address some issues we have been having in terms of data quality. I will be starting with outlining the issues we have been encountered and then more detailed analysis will be laid out to exemplify the quality issues with the data.

## Data Quality Outline

Before starting out with problems of the data tables that we were presented. I have to point out the importance of the meta data. To produce a sound analysis getting the context of the data is very important. This context is almost always supplied by the meta data. Within meta data columns that a data table contains are explained so there is no questions about the data itself. Thus, we need you to provide the meta data as well.

#### Transactions 

1. `online_order` status is missing for some observations.
2. Some entries miss product information. Affected columns are `brand, product_line, product_class, product_size, standard_cost, product_first_sold_date`.
3. Columns `transaction_date` and `product_first_sold_date` are both dates but they have inconsistent formats.

#### New Customer List

1. Encountered some hidden columns while reading the data. Please make sure that you expand all the columns and name them correctly.
2. Also one of the hidden columns is a duplicate of `Rank` column.
3. `job_industry_category` has missing values but the representation of the missing values should not be a text placeholder such as `n/a`.
4. There are many inconsistencies about job titles. There are Latin numbers with job titles and meaning of them are vague. There are also missing values.
5. There is a gender called "U".
6. The Date of Birth variable has inconsistent, non-accurate values and missing. Time formatting should be consistent.

#### Customer Demographic

1. Time formatting is inconsistent. There is one customer who was born in 1843. The same customer also has a gender of "U"
2. Gender Values are inconsistent.
3. Job Titles are ver inconsistent. They should be all lower case. There are also values with Latin numbers such as `Health Coach I` and `Health Coach III`.
4. Customer Demographic table requires a proper Missing Value place holder. In `job_industry_category` there are string "n/a" values.
5. Some customers have missing last names. 
6. There is a "default" column with many random values.

#### Customer Address

1. There are abbreviations for state names along with long state names such as New South Vales and NSW.

#### Problems which are related to more than one table

1. Some customers in `CustomerDemographic` don't have their correspondents in `CustomerAddress`.
2. Also there are some customers only with addresses in `CustomerAddress`.


## In Depth Analysis

### Transactions




There are some entries which miss the identification of online_order status. This should be addressed in the data collection process.


```r
Transactions %>% 
  count(online_order, sort = T)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["online_order"],"name":[1],"type":["lgl"],"align":["right"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"TRUE","2":"9829"},{"1":"FALSE","2":"9811"},{"1":"NA","2":"360"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
Transactions %>% 
  filter(is.na(online_order)) %>% 
  head()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["transaction_id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["product_id"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["customer_id"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["transaction_date"],"name":[4],"type":["dttm"],"align":["right"]},{"label":["online_order"],"name":[5],"type":["lgl"],"align":["right"]},{"label":["order_status"],"name":[6],"type":["chr"],"align":["left"]},{"label":["brand"],"name":[7],"type":["chr"],"align":["left"]},{"label":["product_line"],"name":[8],"type":["chr"],"align":["left"]},{"label":["product_class"],"name":[9],"type":["chr"],"align":["left"]},{"label":["product_size"],"name":[10],"type":["chr"],"align":["left"]},{"label":["list_price"],"name":[11],"type":["dbl"],"align":["right"]},{"label":["standard_cost"],"name":[12],"type":["dbl"],"align":["right"]},{"label":["product_first_sold_date"],"name":[13],"type":["date"],"align":["right"]}],"data":[{"1":"98","2":"49","3":"333","4":"2017-06-23","5":"NA","6":"Approved","7":"Trek Bicycles","8":"Road","9":"medium","10":"medium","11":"533.51","12":"400.13","13":"2003-07-21"},{"1":"167","2":"90","3":"3177","4":"2017-04-26","5":"NA","6":"Approved","7":"Norco Bicycles","8":"Standard","9":"low","10":"medium","11":"363.01","12":"290.41","13":"2005-05-10"},{"1":"170","2":"6","3":"404","4":"2017-10-16","5":"NA","6":"Approved","7":"OHM Cycles","8":"Standard","9":"high","10":"medium","11":"227.88","12":"136.73","13":"2003-08-05"},{"1":"251","2":"63","3":"1967","4":"2017-04-11","5":"NA","6":"Approved","7":"Solex","8":"Standard","9":"medium","10":"medium","11":"1483.20","12":"99.59","13":"2015-05-21"},{"1":"301","2":"78","3":"2530","4":"2017-03-24","5":"NA","6":"Approved","7":"Giant Bicycles","8":"Standard","9":"medium","10":"large","11":"1765.30","12":"709.48","13":"1997-01-25"},{"1":"337","2":"82","3":"1615","4":"2017-10-30","5":"NA","6":"Approved","7":"Norco Bicycles","8":"Standard","9":"high","10":"medium","11":"1148.64","12":"689.18","13":"2013-09-16"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


There are some entries in data table which have no product information. The missing columns are `brand, product_line, product_class, product_size, standard_cost, product_first_sold_date`. This problem might have arisen from a system bug. Additionally, all the missing products have the id of "0".


```r
Transactions %>% 
  filter(is.na(brand)) %>% 
  head()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["transaction_id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["product_id"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["customer_id"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["transaction_date"],"name":[4],"type":["dttm"],"align":["right"]},{"label":["online_order"],"name":[5],"type":["lgl"],"align":["right"]},{"label":["order_status"],"name":[6],"type":["chr"],"align":["left"]},{"label":["brand"],"name":[7],"type":["chr"],"align":["left"]},{"label":["product_line"],"name":[8],"type":["chr"],"align":["left"]},{"label":["product_class"],"name":[9],"type":["chr"],"align":["left"]},{"label":["product_size"],"name":[10],"type":["chr"],"align":["left"]},{"label":["list_price"],"name":[11],"type":["dbl"],"align":["right"]},{"label":["standard_cost"],"name":[12],"type":["dbl"],"align":["right"]},{"label":["product_first_sold_date"],"name":[13],"type":["date"],"align":["right"]}],"data":[{"1":"137","2":"0","3":"431","4":"2017-09-23","5":"FALSE","6":"Approved","7":"NA","8":"NA","9":"NA","10":"NA","11":"1942.61","12":"NA","13":"<NA>"},{"1":"160","2":"0","3":"3300","4":"2017-08-27","5":"FALSE","6":"Approved","7":"NA","8":"NA","9":"NA","10":"NA","11":"1656.86","12":"NA","13":"<NA>"},{"1":"367","2":"0","3":"1614","4":"2017-03-10","5":"FALSE","6":"Approved","7":"NA","8":"NA","9":"NA","10":"NA","11":"850.89","12":"NA","13":"<NA>"},{"1":"407","2":"0","3":"2559","4":"2017-06-14","5":"TRUE","6":"Approved","7":"NA","8":"NA","9":"NA","10":"NA","11":"710.59","12":"NA","13":"<NA>"},{"1":"677","2":"0","3":"2609","4":"2017-07-02","5":"FALSE","6":"Approved","7":"NA","8":"NA","9":"NA","10":"NA","11":"1972.01","12":"NA","13":"<NA>"},{"1":"781","2":"0","3":"897","4":"2017-05-10","5":"TRUE","6":"Approved","7":"NA","8":"NA","9":"NA","10":"NA","11":"311.54","12":"NA","13":"<NA>"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
Transactions %>% 
  filter(is.na(brand)) %>% 
  count(product_id)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["product_id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"0","2":"197"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


Also this table needs a consistent time formatting. Variables `transaction_date` and `product_first_sold_date` are both dates but their inconsistent formatting make it harder to read and parse the data.

### New Customer List

This table has some "hidden" columns! These columns could easily be missed when analyzing the data in Excel. Make sure to expand all the columns that the data table has. Additionally the `Rank` and one of the hidden columns `...21` are duplicated.


```r
NewCustomerList %>% 
  select(...21, Rank) %>% 
  head()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["...21"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["Rank"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"1"},{"1":"1","2":"1"},{"1":"1","2":"1"},{"1":"4","2":"4"},{"1":"4","2":"4"},{"1":"6","2":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
NewCustomerList %>% 
  select(contains("...")) %>% 
  head()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["...17"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["...18"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["...19"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["...20"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["...21"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.56","2":"0.7000","3":"0.8750","4":"0.743750","5":"1"},{"1":"0.89","2":"0.8900","3":"1.1125","4":"0.945625","5":"1"},{"1":"1.01","2":"1.0100","3":"1.0100","4":"1.010000","5":"1"},{"1":"0.87","2":"1.0875","3":"1.0875","4":"1.087500","5":"4"},{"1":"0.52","2":"0.5200","3":"0.6500","4":"0.650000","5":"4"},{"1":"0.43","2":"0.5375","3":"0.5375","4":"0.537500","5":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

There is no customer_id column in this table and this might cause problems. Data creation process for new and old customers should be always the same.



`job_industry_category` has missing values but the representation of the missing values should not be a text placeholder such as `n/a`.


```r
NewCustomerList %>% 
  count(job_industry_category, sort = T)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["job_industry_category"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Financial Services","2":"203"},{"1":"Manufacturing","2":"199"},{"1":"n/a","2":"165"},{"1":"Health","2":"152"},{"1":"Retail","2":"78"},{"1":"Property","2":"64"},{"1":"IT","2":"51"},{"1":"Entertainment","2":"37"},{"1":"Argiculture","2":"26"},{"1":"Telecommunications","2":"25"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

There are many inconsistencies about job titles. There are Latin numbers with job titles and meaning of them are vague, they should be merged. There are also missing values.


```r
NewCustomerList %>% 
  count(job_title, sort = T) %>% 
  slice(80:90)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["job_title"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Occupational Therapist","2":"5"},{"1":"Programmer III","2":"5"},{"1":"Research Nurse","2":"5"},{"1":"Sales Associate","2":"5"},{"1":"Speech Pathologist","2":"5"},{"1":"Tax Accountant","2":"5"},{"1":"Accountant III","2":"4"},{"1":"Budget/Accounting Analyst III","2":"4"},{"1":"Community Outreach Specialist","2":"4"},{"1":"Database Administrator III","2":"4"},{"1":"Editor","2":"4"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

There is a gender called "U". Is this a bug or some placeholder for non-binary persons?



```r
NewCustomerList %>% 
  count(gender)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["gender"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Female","2":"513"},{"1":"Male","2":"470"},{"1":"U","2":"17"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
The Date of Birth variable has inconsistent and non-accurate values. Time formatting should be consistent.

### Customer Demographic



Time formatting is inconsistent in this table too. ALso someone was born in 1843.


```r
CustomerDemographic %>% 
  select(DOB) %>% 
  slice(34:38)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["DOB"],"name":[1],"type":["date"],"align":["right"]}],"data":[{"1":"1843-12-21"},{"1":"1963-09-28"},{"1":"1977-11-09"},{"1":"1985-12-22"},{"1":"1955-10-29"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Gender Values are inconsistent. Values entry for these types of columns should be restricted to pre-defined values.


```r
CustomerDemographic %>% 
  count(gender, sort = T)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["gender"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Female","2":"2037"},{"1":"Male","2":"1872"},{"1":"U","2":"88"},{"1":"F","2":"1"},{"1":"Femal","2":"1"},{"1":"M","2":"1"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Job Titles are ver inconsistent. They should be all lower case. There are also values with Latin numbers such as `Health Coach I` and `Health Coach III`.


```r
CustomerDemographic %>% 
  count(job_title, sort = T) %>% 
  slice(90:99)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["job_title"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Computer Systems Analyst I","2":"15"},{"1":"Safety Technician II","2":"15"},{"1":"Computer Systems Analyst II","2":"14"},{"1":"Computer Systems Analyst IV","2":"14"},{"1":"Database Administrator III","2":"13"},{"1":"Software Test Engineer III","2":"13"},{"1":"Account Representative IV","2":"12"},{"1":"Budget/Accounting Analyst IV","2":"12"},{"1":"Engineer IV","2":"12"},{"1":"Statistician II","2":"12"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Customer Demographic table requires a proper Missing Value place holder. In `job_industry_category` there are string "n/a" values. Those should be proper NA values.


```r
CustomerDemographic %>% 
  count(job_industry_category, sort = T)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["job_industry_category"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"Manufacturing","2":"799"},{"1":"Financial Services","2":"774"},{"1":"n/a","2":"656"},{"1":"Health","2":"602"},{"1":"Retail","2":"358"},{"1":"Property","2":"267"},{"1":"IT","2":"223"},{"1":"Entertainment","2":"136"},{"1":"Argiculture","2":"113"},{"1":"Telecommunications","2":"72"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


Some customers have missing last names. 


```r
CustomerDemographic %>% 
  filter(is.na(last_name)) %>%
  head()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["customer_id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["first_name"],"name":[2],"type":["chr"],"align":["left"]},{"label":["last_name"],"name":[3],"type":["chr"],"align":["left"]},{"label":["gender"],"name":[4],"type":["chr"],"align":["left"]},{"label":["past_3_years_bike_related_purchases"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["DOB"],"name":[6],"type":["date"],"align":["right"]},{"label":["job_title"],"name":[7],"type":["chr"],"align":["left"]},{"label":["job_industry_category"],"name":[8],"type":["chr"],"align":["left"]},{"label":["wealth_segment"],"name":[9],"type":["chr"],"align":["left"]},{"label":["deceased_indicator"],"name":[10],"type":["chr"],"align":["left"]},{"label":["default"],"name":[11],"type":["chr"],"align":["left"]},{"label":["owns_car"],"name":[12],"type":["chr"],"align":["left"]},{"label":["tenure"],"name":[13],"type":["dbl"],"align":["right"]}],"data":[{"1":"4","2":"Talbot","3":"NA","4":"Male","5":"33","6":"1961-10-03","7":"NA","8":"IT","9":"Mass Customer","10":"N","11":"() { _; } >_[$($())] { touch /tmp/blns.shellshock2.fail; }","12":"No","13":"7"},{"1":"67","2":"Vernon","3":"NA","4":"Male","5":"67","6":"1960-06-14","7":"Web Developer II","8":"Retail","9":"Mass Customer","10":"N","11":"<svg><script>0<1>alert('XSS')<\/script>","12":"No","13":"18"},{"1":"106","2":"Glyn","3":"NA","4":"Male","5":"54","6":"1966-07-03","7":"Software Test Engineer III","8":"Health","9":"High Net Worth","10":"N","11":"ãã¼ãã£ã¼ã¸è¡ããªãã","12":"Yes","13":"18"},{"1":"139","2":"Gar","3":"NA","4":"Male","5":"1","6":"1964-07-28","7":"Operator","8":"Telecommunications","9":"Affluent Customer","10":"N","11":"-100","12":"No","13":"4"},{"1":"197","2":"Avis","3":"NA","4":"Female","5":"32","6":"1977-01-27","7":"NA","8":"n/a","9":"High Net Worth","10":"N","11":"NA","12":"No","13":"5"},{"1":"211","2":"Beitris","3":"NA","4":"Female","5":"6","6":"1974-03-04","7":"VP Marketing","8":"Manufacturing","9":"Mass Customer","10":"N","11":"-0.5","12":"Yes","13":"5"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


There is a "default" column with many random values. Probably because of some bug in the data acquisition process.


```r
CustomerDemographic %>% 
  select(default) %>% 
  slice_head(n = 10)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["default"],"name":[1],"type":["chr"],"align":["left"]}],"data":[{"1":"\"'"},{"1":"<script>alert('hi')<\/script>"},{"1":"43132"},{"1":"() { _; } >_[$($())] { touch /tmp/blns.shellshock2.fail; }"},{"1":"NIL"},{"1":"<U+00F0>µ <U+00F0> <U+00F0> <U+00F0>"},{"1":"â°â´âµâââ"},{"1":"(â¯Â°â¡Â°ï¼â¯ï¸µ â»ââ»)"},{"1":"0/0"},{"1":"<U+00F0>©<U+00F0>½"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>



### Customer Address




New South Vales and NSW are probably the same state. The similar situation goes for VIC and Victoria.


```r
CustomerAddress %>% 
  count(state)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["state"],"name":[1],"type":["chr"],"align":["left"]},{"label":["n"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"New South Wales","2":"86"},{"1":"NSW","2":"2054"},{"1":"QLD","2":"838"},{"1":"VIC","2":"939"},{"1":"Victoria","2":"82"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


### Joins

These customers don't have their address information.


```r
CustomerDemographic %>% 
  anti_join(CustomerAddress, by = "customer_id") 
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["customer_id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["first_name"],"name":[2],"type":["chr"],"align":["left"]},{"label":["last_name"],"name":[3],"type":["chr"],"align":["left"]},{"label":["gender"],"name":[4],"type":["chr"],"align":["left"]},{"label":["past_3_years_bike_related_purchases"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["DOB"],"name":[6],"type":["date"],"align":["right"]},{"label":["job_title"],"name":[7],"type":["chr"],"align":["left"]},{"label":["job_industry_category"],"name":[8],"type":["chr"],"align":["left"]},{"label":["wealth_segment"],"name":[9],"type":["chr"],"align":["left"]},{"label":["deceased_indicator"],"name":[10],"type":["chr"],"align":["left"]},{"label":["default"],"name":[11],"type":["chr"],"align":["left"]},{"label":["owns_car"],"name":[12],"type":["chr"],"align":["left"]},{"label":["tenure"],"name":[13],"type":["dbl"],"align":["right"]}],"data":[{"1":"3","2":"Arlin","3":"Dearle","4":"Male","5":"61","6":"1954-01-20","7":"Recruiting Manager","8":"Property","9":"Mass Customer","10":"N","11":"43132","12":"Yes","13":"15"},{"1":"10","2":"Fiorenze","3":"Birdall","4":"Female","5":"49","6":"1988-10-11","7":"Senior Quality Engineer","8":"Financial Services","9":"Mass Customer","10":"N","11":"<U+00F0>©<U+00F0>½","12":"Yes","13":"20"},{"1":"22","2":"Deeanne","3":"Durtnell","4":"Female","5":"79","6":"1962-12-10","7":"NA","8":"IT","9":"Mass Customer","10":"N","11":"ï¼ï¼ï¼","12":"No","13":"11"},{"1":"23","2":"Olav","3":"Polak","4":"Male","5":"43","6":"1995-02-10","7":"NA","8":"n/a","9":"High Net Worth","10":"N","11":"43132","12":"Yes","13":"1"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

There are also customers who have their address information but they don't exist in customer data.


```r
CustomerAddress %>% 
  anti_join(CustomerDemographic, by = "customer_id") 
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["customer_id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["address"],"name":[2],"type":["chr"],"align":["left"]},{"label":["postcode"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["state"],"name":[4],"type":["chr"],"align":["left"]},{"label":["country"],"name":[5],"type":["chr"],"align":["left"]},{"label":["property_valuation"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"4001","2":"87 Crescent Oaks Alley","3":"2756","4":"NSW","5":"Australia","6":"10"},{"1":"4002","2":"8194 Lien Street","3":"4032","4":"QLD","5":"Australia","6":"7"},{"1":"4003","2":"320 Acker Drive","3":"2251","4":"NSW","5":"Australia","6":"7"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>























































