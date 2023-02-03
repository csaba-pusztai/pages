Overview
========

The sample
----------

The org sample used for the analysis is standard SBI and includes orgs
that:

-   have any product option other than 'Ledger', 'GST Cashbook' or
    'Non-GST Cashbook'
-   are identified as 'Sole Trader', 'Company' or 'Partnership'
-   are not in trial mode (~paying customers)
-   have been on Xero for at least 12 months
-   had less than 10 million pounds of revenue in the last 12 months
    (relative to the date of analysis)

Geographic coverage
-------------------

-   'London' (NUTS1)
-   'Cardiff' (LAU1)
-   'Leeds' (LAU1)
-   'Manchester' (LAU1)
-   'Glasgow City' (LAU1)
-   'Bristol, City of' (LAU1)

Time period covered
-------------------

-   2019-01-01 - 2019-12-31

Question 1
==========

> The amount that the average small business is owed in late payments on
> any given day in that city (so Cash Mountain but for that city)

Metric
------

-   **Average Median Overdue Accounts Receivable Balance at
    End-of-Month**
-   **Average Overdue Accounts Receivable Balance at End-of-Month**

### Derivation

1.  AR Balance: the balance of all outstanding payments by customers
    (money owed to a business by its customers) on a particular day for
    a particular business
2.  Overdue AR Balance: the part of (1) past due date on a particular
    day for a particular business
3.  Overdue AR Balance at EOM: (2) on the last day of a particular month
    for a particular business
4.  Median Overdue AR Balance at EOM: The median value of (3) across a
    group of businesses (~the sample)
5.  Average Median Overdue AR Balance at EOM: (4) averaged across a 12
    consecutive months
6.  Average Overdue Accounts Receivable Balance at EOM: the average
    value of (3) across 12 consecutive months

### Comment

Orgs with no outstanding AR balance at EOM are not included in
calculating the median/average in the given month, so the correct
interpretation of the metric is as follows: '*Among businesses that were
owed past due date, the median/average overdue amount was X.*'

Input dataset
-------------

-   `xsbi_pipeline.credit_sales_ar_om__sbi`: Monthly sales on credit,
    opening and closing Accounts Receivable balances (Column:
    `overduebalanceatmonthend`)

Output
------

<table>
<caption>Avg Median Overdue AR balance at EOM (GBP)</caption>
<thead>
<tr class="header">
<th align="left">City</th>
<th align="right">Avg Median Overdue AR at EOM (£)</th>
<th align="right">Avg Overdue AR at EOM (£)</th>
<th align="right">Avg Median Overdue AR at EOM - All Orgs (£)</th>
<th align="right">Avg Overdue AR at EOM - All Orgs (£)</th>
<th align="right">Avg Overdue Invoice Sample Size</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Bristol, City of</td>
<td align="right">6,397</td>
<td align="right">54,758</td>
<td align="right">1,966</td>
<td align="right">41,344</td>
<td align="right">53405.67</td>
</tr>
<tr class="even">
<td align="left">Cardiff</td>
<td align="right">7,746</td>
<td align="right">48,975</td>
<td align="right">2,321</td>
<td align="right">36,011</td>
<td align="right">18814.08</td>
</tr>
<tr class="odd">
<td align="left">Glasgow City</td>
<td align="right">8,970</td>
<td align="right">53,725</td>
<td align="right">1,921</td>
<td align="right">38,078</td>
<td align="right">21736.67</td>
</tr>
<tr class="even">
<td align="left">Leeds</td>
<td align="right">10,404</td>
<td align="right">61,730</td>
<td align="right">3,823</td>
<td align="right">48,032</td>
<td align="right">57068.42</td>
</tr>
<tr class="odd">
<td align="left">London</td>
<td align="right">12,120</td>
<td align="right">95,524</td>
<td align="right">2,966</td>
<td align="right">68,770</td>
<td align="right">787182.2</td>
</tr>
<tr class="even">
<td align="left">Manchester</td>
<td align="right">12,491</td>
<td align="right">94,112</td>
<td align="right">4,549</td>
<td align="right">72,294</td>
<td align="right">36064.75</td>
</tr>
</tbody>
</table>

### Comment

Clearly, there is a huge gap between averages and medians suggesting
that the distribution is highly skewed by some businesses running large
overdue balances. If we take the average and the median across all orgs
with outstanding balances at EOM (not necessarily overdue) then of
course both measures are lower. These are all end-of-month figures, that
is calculated for a single day for each month in the period rather than
for each day in the month and then taking the daily average. The EOM
value may be higher than the daily average as outstanding and overdue
balances are likely to grow toward the end of the month. They are
adequate for making comparisons across the cities (cross sectionally)
but they are not directly compatible with earlier the Cash Mountain
calculations as those were based on daily balances averaged across a
month.

Question 2
==========

> 'The percentage of small businesses in that city that are owed money
> at any one time outside of agreed payment terms'

Metric
------

-   **Average monthly proportion of orgs with at least one overdue
    invoice** (`avg_org_proportion_owed`)

### Derivation

1.  Number of orgs with at least one overdue invoice in the month
    (monthly, within the covered period)
2.  Number of orgs with at least one invoice in the month (monthly,
    within the covered period)
3.  Proportion of orgs with at least one overdue invoice in the
    month: (2) / (1) (monthly, within the covered period)
4.  Average monthly proportion of orgs with at least one overdue
    invoice: SUM((3)) / number of months in the period

### Comment

Rather than calculating the proportion of orgs across the whole period
directly, first monthly proportions are calculated which are then
averaged over the period covered. This is a slightly more 'conservative'
approach yealding a lower value for the metric capturing the average
monthly proportion in the period. The less conservative approach would
capture the overall proportion in the period as a whole. It would be a
higher number as a greater proportion of orgs would be affected by an
overdue invoice at any one point during the period (12 months).

Input dataset
-------------

-   `xsbi_pipeline.sales_invoices__sbi`: source documents documenting
    sales (cash sales, AR invoices) for SBI orgs

Output
------

<table>
<caption>Avg Proportion of Orgs with Overdue</caption>
<thead>
<tr class="header">
<th align="left">City</th>
<th align="right">Avg Proportion of Orgs with Overdue (%)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Glasgow City</td>
<td align="right">80.31</td>
</tr>
<tr class="even">
<td align="left">Leeds</td>
<td align="right">83.19</td>
</tr>
<tr class="odd">
<td align="left">Bristol, City of</td>
<td align="right">81.70</td>
</tr>
<tr class="even">
<td align="left">Manchester</td>
<td align="right">85.20</td>
</tr>
<tr class="odd">
<td align="left">London</td>
<td align="right">80.39</td>
</tr>
<tr class="even">
<td align="left">Cardiff</td>
<td align="right">82.78</td>
</tr>
</tbody>
</table>

Question 3
==========

> 'The month in that city when payments take the longest to get through
> - and how many days late the invoice was paid'

Metric
------

-   **Monthly Median Days To Pay**

### Derivation

1.  Days to Pay: difference between invoice date and due date in days
2.  Median Days To Pay: the 'days to pay' value of the invoice that sits
    in the middle of the distribution ordered on 'days to pay'.
3.  Monthly Median Days To Pay: the 'days to pay' distribution is
    grouped on the month of full payment (rather than invoice date).

Input dataset
-------------

-   `xsbi_pipeline.paid_ar_invoices__sbi`

Output
------

<table>
<caption>Median Days to Pay (max. term 31 days</caption>
<thead>
<tr class="header">
<th align="left">City</th>
<th align="right">2019M01</th>
<th align="right">2019M02</th>
<th align="right">2019M03</th>
<th align="right">2019M04</th>
<th align="right">2019M05</th>
<th align="right">2019M06</th>
<th align="right">2019M07</th>
<th align="right">2019M08</th>
<th align="right">2019M09</th>
<th align="right">2019M10</th>
<th align="right">2019M11</th>
<th align="right">2019M12</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Bristol, City of</td>
<td align="right">8</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
</tr>
<tr class="even">
<td align="left">Cardiff</td>
<td align="right">6</td>
<td align="right">4</td>
<td align="right">4</td>
<td align="right">10</td>
<td align="right">14</td>
<td align="right">14</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">14</td>
<td align="right">14</td>
<td align="right">15</td>
<td align="right">12</td>
</tr>
<tr class="odd">
<td align="left">Glasgow City</td>
<td align="right">7</td>
<td align="right">6</td>
<td align="right">6</td>
<td align="right">6</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">8</td>
</tr>
<tr class="even">
<td align="left">Leeds</td>
<td align="right">18</td>
<td align="right">13</td>
<td align="right">14</td>
<td align="right">15</td>
<td align="right">14</td>
<td align="right">14</td>
<td align="right">15</td>
<td align="right">13</td>
<td align="right">14</td>
<td align="right">14</td>
<td align="right">13</td>
<td align="right">13</td>
</tr>
<tr class="odd">
<td align="left">London</td>
<td align="right">7</td>
<td align="right">6</td>
<td align="right">6</td>
<td align="right">7</td>
<td align="right">7</td>
<td align="right">6</td>
<td align="right">7</td>
<td align="right">6</td>
<td align="right">6</td>
<td align="right">6</td>
<td align="right">6</td>
<td align="right">6</td>
</tr>
<tr class="even">
<td align="left">Manchester</td>
<td align="right">14</td>
<td align="right">11</td>
<td align="right">10</td>
<td align="right">11</td>
<td align="right">10</td>
<td align="right">11</td>
<td align="right">14</td>
<td align="right">12</td>
<td align="right">13</td>
<td align="right">13</td>
<td align="right">10</td>
<td align="right">12</td>
</tr>
</tbody>
</table>

As the median days to pay metric is always an integer value with a
limited range, multiple months can be associated with the highest valu,
especially when there is little variability over time. If we look at the
median values across the whole period covered, we can see that there is
very little movement in the metric in Bristol, Glasgow, and London. In
these cities median days to pay sits roughly at the same level across
the entire study period and with a minor hike in Bristol at the very
beginning of the period. These three cities are actually quite similar
not only in terms of this flat pattern but also the absolute level of
the metric. Leeds also exhibits a reasonably stable pattern (+/-1 day of
variability) but with a bigger spike in January (+4 days MoM). Also,
Leeds is clearly worse off than the other three cities with its median
days to pay sitting about twice as high throughout the period.
Manchester is somewhere between the three cities and Leeds with its
median days to pay metric varying between 10 and 14 with its worst
months being January and July, the same as in Leeds. Cardiff is the odd
one out in that it started the period very favourably (very low median
days to pay) which then jumped up in April and remained high for the
rest of the period (14-15 days).

Question 4
==========

> 'How long it takes for the average late invoice to be paid in that
> city?'

Metric
------

-   **Monthly Median Overdue Days**

### Derivation

1.  Overdue Days: difference between due date and date of full payment
    in days
2.  Median Overdue Days: the 'overdue days' value of the invoice that
    sits in the middle of the distribution ordered on 'overdue day'
    (only overdue invoices considered).
3.  Monthly Median Overdue Days: the 'overdue days' distribution is
    grouped on the month of full payment (rather than invoice date),
    median value in the month.

Input dataset
-------------

-   `xsbi_pipeline.paid_ar_invoices__sbi`

Output
------

<table>
<caption>Monthly Median Overdue Days</caption>
<thead>
<tr class="header">
<th align="left">City</th>
<th align="right">2019M01</th>
<th align="right">2019M02</th>
<th align="right">2019M03</th>
<th align="right">2019M04</th>
<th align="right">2019M05</th>
<th align="right">2019M06</th>
<th align="right">2019M07</th>
<th align="right">2019M08</th>
<th align="right">2019M09</th>
<th align="right">2019M10</th>
<th align="right">2019M11</th>
<th align="right">2019M12</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Bristol, City of</td>
<td align="right">12</td>
<td align="right">8</td>
<td align="right">8</td>
<td align="right">8</td>
<td align="right">8</td>
<td align="right">7</td>
<td align="right">8</td>
<td align="right">8</td>
<td align="right">8</td>
<td align="right">9</td>
<td align="right">8</td>
<td align="right">7</td>
</tr>
<tr class="even">
<td align="left">Cardiff</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">14</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">15</td>
</tr>
<tr class="odd">
<td align="left">Glasgow City</td>
<td align="right">13</td>
<td align="right">13</td>
<td align="right">12</td>
<td align="right">12</td>
<td align="right">14</td>
<td align="right">13</td>
<td align="right">12</td>
<td align="right">15</td>
<td align="right">10</td>
<td align="right">13</td>
<td align="right">15</td>
<td align="right">11</td>
</tr>
<tr class="even">
<td align="left">Leeds</td>
<td align="right">18</td>
<td align="right">17</td>
<td align="right">15</td>
<td align="right">15</td>
<td align="right">14</td>
<td align="right">16</td>
<td align="right">14</td>
<td align="right">16</td>
<td align="right">15</td>
<td align="right">13</td>
<td align="right">14</td>
<td align="right">13</td>
</tr>
<tr class="odd">
<td align="left">London</td>
<td align="right">16</td>
<td align="right">15</td>
<td align="right">13</td>
<td align="right">12</td>
<td align="right">14</td>
<td align="right">13</td>
<td align="right">13</td>
<td align="right">14</td>
<td align="right">14</td>
<td align="right">14</td>
<td align="right">14</td>
<td align="right">11</td>
</tr>
<tr class="even">
<td align="left">Manchester</td>
<td align="right">13</td>
<td align="right">13</td>
<td align="right">11</td>
<td align="right">10</td>
<td align="right">12</td>
<td align="right">12</td>
<td align="right">13</td>
<td align="right">12</td>
<td align="right">13</td>
<td align="right">12</td>
<td align="right">12</td>
<td align="right">11</td>
</tr>
</tbody>
</table>

In some cities, January appears to be a spike in terms of late payment.
This likely to be due to invoices raised in December, end of the
calendar year marked with holidays and lower prudency in terms of
payments. This is quite pronounced in Leeds, London and Bristol.
Cardiff, on the other hand, has quite a smooth/flat profile throughout
the period.

Question 5
==========

> 'The cash flow status of businesses in that city on average (also the
> tightest month).'

Metric
------

-   **Proportion of Cash Flow Positive Orgs**

### Derivation

1.  Number of orgs with cash flow in a particular month
2.  Number of orgs with positive cashflow (cash in &gt; cash out) in the
    month
3.  Proportion of Cash Flow Positive Orgas: (2) / (1)

Input dataset
-------------

-   `xsbi_pipeline.cashflow_omc__sbi`

Output
------

<table>
<caption>% of Cash Flow Positive Businesses</caption>
<thead>
<tr class="header">
<th align="left">City</th>
<th align="right">Average Proportion of Cash Flow Positive Orgs (%)</th>
<th align="right">Relative variability (%)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Bristol, City of</td>
<td align="right">50.0</td>
<td align="right">6.6</td>
</tr>
<tr class="even">
<td align="left">Cardiff</td>
<td align="right">50.1</td>
<td align="right">6.8</td>
</tr>
<tr class="odd">
<td align="left">Glasgow City</td>
<td align="right">49.3</td>
<td align="right">6.2</td>
</tr>
<tr class="even">
<td align="left">Leeds</td>
<td align="right">50.1</td>
<td align="right">5.1</td>
</tr>
<tr class="odd">
<td align="left">London</td>
<td align="right">48.3</td>
<td align="right">4.0</td>
</tr>
<tr class="even">
<td align="left">Manchester</td>
<td align="right">48.8</td>
<td align="right">3.8</td>
</tr>
</tbody>
</table>

<table>
<caption>Tightest Cash Flow Month</caption>
<thead>
<tr class="header">
<th align="left">City</th>
<th align="left">Month</th>
<th align="right">Proportion of Cash Flow Positive Orgs (%)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Glasgow City</td>
<td align="left">2019M01</td>
<td align="right">42.3</td>
</tr>
<tr class="even">
<td align="left">Cardiff</td>
<td align="left">2019M01</td>
<td align="right">43.6</td>
</tr>
<tr class="odd">
<td align="left">Bristol, City of</td>
<td align="left">2019M01</td>
<td align="right">44.1</td>
</tr>
<tr class="even">
<td align="left">London</td>
<td align="left">2019M01</td>
<td align="right">44.2</td>
</tr>
<tr class="odd">
<td align="left">Leeds</td>
<td align="left">2019M01</td>
<td align="right">45.3</td>
</tr>
<tr class="even">
<td align="left">Manchester</td>
<td align="left">2019M01</td>
<td align="right">45.6</td>
</tr>
</tbody>
</table>

All cities appear to be in the same pack in terms of the proportion of
cash flow positive orgs throughout the period with London having the
lowest value. In terms of variability, however, the difference is more
pronounced across the cities. Bristol and Cardiff have the highest
average proportion of cash flow positive orgs but at the same time they
show the highest variability in the metric throughout the year (relative
standard deviation being 7.4% and 7.8% around their respective means).

Cash Mountain
=============

Metric
------

-   **Average Overdue Accounts Receivable Balance**
-   **Average Median Overdue Accounts Receivable Balance**

### Derivation

Similar as in Question 1, however, here the averages are across all
daily AR balances on the 365 days of the period, rather then just
end-of-month days. This conveys an 'average day' sense.

Input dataset
-------------

-   `xsbi_pipeline.credit_sales_ar_od__sbi`: daily credit sales and
    accounts receivable balance

Output
------

<table>
<caption>Overdue AR on 'average' day</caption>
<thead>
<tr class="header">
<th align="left">City</th>
<th align="right">Avg Overdue AR (£)</th>
<th align="right">Avg Median Overdue AR</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Bristol, City of</td>
<td align="right">16,862</td>
<td align="right">191</td>
</tr>
<tr class="even">
<td align="left">Cardiff</td>
<td align="right">17,073</td>
<td align="right">63</td>
</tr>
<tr class="odd">
<td align="left">Glasgow City</td>
<td align="right">16,597</td>
<td align="right">7</td>
</tr>
<tr class="even">
<td align="left">Leeds</td>
<td align="right">19,696</td>
<td align="right">577</td>
</tr>
<tr class="odd">
<td align="left">London</td>
<td align="right">24,666</td>
<td align="right">0</td>
</tr>
<tr class="even">
<td align="left">Manchester</td>
<td align="right">28,916</td>
<td align="right">694</td>
</tr>
</tbody>
</table>

Yet again, the difference between the average and median-based metric
shows that the distribution is quite skewed. For instance the average
overdue balance in London is more than 24,000 pounds across all
businesses, but more than half of the businesses actually have no
overdue balance at all (hence the median value of 0).
