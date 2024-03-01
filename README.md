# Yellevate: Client Disputes - Data Analysis  

**Overview of the Problem**  
  
▪ The Yellevate company has been struggling with client disputes over the past few years (2020-2022). Yellevate defines disputes as instances where clients express dissatisfaction with the company’s services and refuse payment.    
▪ It has imposed a significant financial strain on the company: roughly 20% of the disputes raised against Yellevate led to a payment opt-out and resulted in a 5% yearly revenue loss (USD).  
  
**Methodology**  
  
PostgreSQL is used for data cleaning and in-depth analysis, complemented by Excel for visualization and summarization.  
This combination enhances our capacity to extract actionable insights and make informed decisions efficiently.  
  
**Data Manipulation -- SQL**  
  
**i. The processing time in which invoices are settled**  
SELECT country, ROUND(AVG(days_settled)) "AVG_days_settle" FROM yellevate_invoices  
GROUP BY country  
ORDER BY country DESC;  
SELECT ROUND(AVG(days_settled)) "AVG_days_invoice_settled" FROM yellevate_invoices;*  
  
**ii. The processing time for the company to settle disputes.**  
SELECT DISTINCT(country) FROM yellevate_invoices;  
SELECT country, ROUND(AVG(days_settled)) FROM yellevate_invoices  
WHERE disputed = 1  
GROUP BY ROLLUP(1)  
ORDER BY country DESC;  
SELECT ROUND(AVG(days_settled)) FROM yellevate_invoices  
WHERE disputed = 1;  
  
**iii. Percentage of disputes received by the company that were lost.**  
SELECT sum(dispute_lost) FROM yellevate_invoices;  
SELECT SUM(disputed) FROM yellevate_invoices  
WHERE disputed = 1;  
SELECT ROUND((SELECT sum(dispute_lost) FROM yellevate_invoices) /  
(SELECT SUM(disputed) FROM yellevate_invoices WHERE disputed = 1)*100,2) "Revenue Lost"  
FROM yellevate_invoices  
GROUP BY "Revenue Lost";  
  
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
  
**Visualizations of Findings**  
  
i. The average processing time in which invoices are settled was 26 days.  
![Alt text](https://github.com/xyrncl/Client-Disputes-Analysis-Yellevate/blob/main/Findings%20Yellevate/F-1.jpg)  

ii. The processing time for the company to settle disputes was 36 days.
![Alt_text](https://github.com/xyrncl/Client-Disputes-Analysis-Yellevate/blob/main/Findings%20Yellevate/F-2.jpg)  

iii. There are 101 Companies whose disputes were “lost”, out of the 571 disputes. A total of 17.69% of all disputes were lost.  
![Alt_text](https://github.com/xyrncl/Client-Disputes-Analysis-Yellevate/blob/main/Findings%20Yellevate/F-3.jpg)   
![Alt_text](https://github.com/xyrncl/Client-Disputes-Analysis-Yellevate/blob/main/Findings%20Yellevate/F-3i.jpg)  
  
iv. The total amount of revenue lost was 690,167 (out of the total revenue of 14,770,318), equivalent to 4.67%.  
![Alt_text](https://github.com/xyrncl/Client-Disputes-Analysis-Yellevate/blob/main/Findings%20Yellevate/F-4.jpg)  

v. The country with the highest losses from lost disputes is France amounting to $526,264.00 out of the total loss amounting to $690,167.00.  
![Alt_text](https://github.com/xyrncl/Client-Disputes-Analysis-Yellevate/blob/main/Findings%20Yellevate/F-5.jpg)  
  

    
**Recommendations**
  
i. Conduct a thorough review of contracts, focusing on Terms and Conditions, Client Obligations, Services, Fees, and Payment terms, identifying and addressing any discrepancies or weaknesses.  
ii. Consider revising contracts to clearly define time boundaries for invoice scheduling and dispute resolution, specifying Yellevate's desired time allocation.  
iii. Adjust the scheduling of invoices and dispute resolution processes as needed for efficiency.  
iv. Introduce a policy allowing Yellevate to suspend services if clients fail to make payments on time, even before completion, to prevent future financial losses.  
v. Implement periodic payments, possibly on a monthly basis, ensuring services continue only if each payment is fulfilled.  
vi. Analyze the nature of businesses Yellevate serves to identify specific areas causing disputes, enabling targeted improvement efforts, enhancing customer satisfaction, and maintaining revenue.  


