# Yellevate: Client Disputes Data Analysis

**Overview of the Problem**
▪ The problem was that Yellevate company has been struggling with client disputes over the past few years (2020-2022). 
  Yellevate defines disputes as clients expressing dissatisfaction with the company’s services and clients refusing payment to the company.
▪ It has imposed a significant financial strain on the company: roughly 20% of the disputes raised against Yellevate led to a payment opt-out and resulted in a 5% yearly revenue loss (USD).

**Methodology**

PostgreSQL is used for data cleaning and in-depth analysis, complemented by Excel for visualization and summarization. 
This combination enhances our capacity to extract actionable insights and make informed decisions efficiently.

&nbsp; **Data Manipulation SQL**

**i. The processing time in which invoices are settled**
SELECT country, ROUND(AVG(days_settled)) "AVG_days_settle" FROM yellevate_invoices
GROUP BY country
ORDER BY country DESC;
SELECT ROUND(AVG(days_settled)) "AVG_days_invoice_settled" FROM yellevate_invoices;*

**The average processing time in which invoices are settled is 26 days.**

**ii. The processing time for the company to settle disputes.**
SELECT DISTINCT(country) FROM yellevate_invoices;
SELECT country, ROUND(AVG(days_settled)) FROM yellevate_invoices
WHERE disputed = 1
GROUP BY ROLLUP(1)
ORDER BY country DESC;
SELECT ROUND(AVG(days_settled)) FROM yellevate_invoices
WHERE disputed = 1;

**The processing time for the company to settle disputes was 36 days.**

**iii. Percentage of disputes received by the company that were lost.**
SELECT sum(dispute_lost) FROM yellevate_invoices;
SELECT SUM(disputed) FROM yellevate_invoices
WHERE disputed = 1;
SELECT ROUND((SELECT sum(dispute_lost) FROM yellevate_invoices) /
(SELECT SUM(disputed) FROM yellevate_invoices WHERE disputed = 1)*100,2) "Revenue Lost"
FROM yellevate_invoices
GROUP BY "Revenue Lost";

**There are 101 Companies whose disputes were “lost”, out of the 571 disputes. A total of 17.69% of all disputes were lost.**

**iv. Percentage of revenue lost from disputes.**

SELECT country, SUM(invoice_amount) FROM yellevate_invoices
WHERE dispute_lost = 1
GROUP BY ROLLUP(1);
SELECT country, SUM(invoice_amount) FROM yellevate_invoices
GROUP BY ROLLUP(1);
SELECT (ROUND((SELECT SUM(invoice_amount) FROM yellevate_invoices
WHERE dispute_lost = 1)/
(SELECT SUM(invoice_amount) FROM yellevate_invoices),4)*100) "Percentage of Revenue Lost"
FROM yellevate_invoices
GROUP BY "Percentage of Revenue Lost";

**The total revenue lost amounted to $690,167, which is 4.67% of the total revenue of $14,770,318**

**v. The country where the company reached the highest losses from disputes.**
SELECT country, disputed, dispute_lost FROM yellevate_invoices;
SELECT country, SUM(invoice_amount)
FROM yellevate_invoices
WHERE "disputed" = 1
AND "dispute_lost" = 1
GROUP BY country
ORDER BY SUM(invoice_amount) DESC;
SELECT country, SUM(invoice_amount) FROM yellevate_invoices
WHERE dispute_lost = 1 and country = 'France'
GROUP BY country;
SELECT country, customer_id, SUM(invoice_amount) AS total_invoice_amount FROM yellevate_invoices
WHERE dispute_lost = 1 and country = 'France'
GROUP BY country, customer_id
ORDER BY total_invoice_amount DESC;
SELECT DISTINCT customer_id, country FROM yellevate_invoices
WHERE country = 'France' and dispute_lost = 1;

**The country with the highest losses from lost disputes is France amounting to $526,264.00 out of the total loss amounting to $690,167.00.**

**Visualizations of Findings**





