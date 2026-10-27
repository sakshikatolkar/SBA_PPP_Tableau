# SBA PPP Loans: How Many Jobs Did the Money Keep?

The Paycheck Protection Program (PPP) put billions of dollars into small businesses during COVID. The idea was simple: lend money, and forgive it if the business kept its people on payroll. I wanted to see how that played out once you break the loans down by industry, region and type of borrower. So I built a Tableau dashboard around one question: **how many jobs were reported for every $100K that was forgiven?**

![SBA PPP Tableau dashboard](images/dashboard.png)

## The data

The data is the SBA's public PPP loan data, filtered down to loans of **$150K and above** from the second wave of the program. After cleaning, there are **61,043 loans** across all 50 states, DC and four US territories, with approval amounts from $150K up to $2M.

Each row is one loan. These are the fields I used most:

- **Borrower State / Region** (Midwest, Northeast, South, West, US Territories)
- **NAICS Code and Description** (the borrower's industry)
- **Current Approval Amount**, **Forgiveness Amount** and **Jobs Reported**
- **Gender**, **Race**, **Veteran**, **Business Type**
- **Rural Urban Indicator**, **LMI Indicator** (low/moderate income area) and **Hubzone Indicator**

I also added a few columns while preparing the data:

- **Efficiency ratio** = `(Jobs Reported / Forgiveness Amount) * 100000`, which works out to jobs supported per $100K forgiven
- **50_percentile_efficiency**, a 0/1 flag for whether a loan is above the median efficiency (about 8.9 jobs per $100K)
- Short flag versions of region, gender, race and veteran status, plus 2-digit NAICS codes and 5-digit zip codes

## What's on the dashboard

| View | What it shows |
|---|---|
| Industry Sector Performance | A highlight table of total approved amount by industry (rows) and region (columns) |
| Borrower Characteristics | Average efficiency split by gender and rural/urban, coloured by LMI area |
| Loan Volume vs. Efficiency | Number of loans per region, split into below-median and above-median efficiency |
| Heat map | Average efficiency by state, shaded by how each state ranks |
| Race | Each race group's share of the total approved amount |
| KPIs | Total loans and the overall average efficiency ratio |

You can filter everything by region and state, and clicking a race group highlights it across the other views.

## A few things I found

- **Restaurants and hotels got the most money.** Accommodation & Food Services had about 16K loans worth $5.9B, followed by Construction ($4.1B) and Manufacturing ($2.9B).
- **The jobs-per-dollar picture is different.** Arts & Entertainment (17.7), Educational Services (14.1) and Accommodation & Food (14.0) reported the most jobs per $100K. Professional Services and Mining sat at the bottom, at around 7.
- **US Territories had the highest efficiency** (18.6 jobs per $100K), but from only 281 loans. Among the mainland regions, the Midwest (11.9) and South (11.5) were ahead of the West and Northeast (both around 10.4).
- **Rural borrowers did a bit better than urban ones** (about 11.8 vs 11.0), and female-owned businesses did a bit better than male-owned ones (11.5 vs 10.9).
- **The money was very uneven by race.** Businesses listed as White received about 82% of the approved amount, Asian-owned about 12%, and every other group combined less than 7%.

Across the dataset, the average is **11.07 jobs per $100K forgiven**.

## Files in this repo

```
SBA_PPP_Final.twb                              Tableau workbook (the dashboard + 6 sheets)
SBA_PPP_150K_secondwave_final_dataset.hyper    Tableau data extract the workbook reads from
images/dashboard.png                           Screenshot of the dashboard
```

## How to open it

1. Install [Tableau Public](https://www.tableau.com/products/public/download) (free) or Tableau Desktop.
2. Clone or download this repo, and keep the `.twb` and `.hyper` files in the same folder.
3. Open `SBA_PPP_Final.twb`. If Tableau asks where the data is, point it to the `.hyper` file.

## Known issues / what I'd improve next

- One loan in Tennessee has a forgiveness amount of $0.03, which gives it a ratio in the hundreds of millions. I left it out of the numbers above, but it's still in the workbook, so it inflates the "Female Owned / Urban" bar and Tennessee on the map. It should be filtered out at the data source.
- The KPI numbers are typed in as text right now, so they don't change when you filter. I want to switch them to live calculations.
- The "average efficiency" is the average of each loan's ratio. Total jobs divided by total forgiveness (about 10.0) would be another fair way to measure it, and I'd like to show both.
- The sheets still have default names (Sheet 1 to 6), and I'd like to publish an interactive version on Tableau Public.

## Tools

Tableau Desktop / Tableau Public for the dashboard, and Excel for cleaning and preparing the data.
