# Bank_Data_Analysis
-- 01) Retrive the account & loan details whose accounts were issued in the year between 1993 & 1997 with districtid more than 2 for a given loan id and also,
--     loan issued date between 1995 & 1996 with loan amount more than 25 K.
SELECT a.*,l.*
FROM account a
JOIN loan l
ON a.account_id=l.account_id
WHERE YEAR(a.DATE) BETWEEN 1993 AND 1997
AND a.district_id >2
AND YEAR(l.DATE) BETWEEN 1995 AND 1997
AND l.amount> 20000;

-- 02) Retrive the accounts and orders details (No duplicate data), whose accounts were initiated after the year 1994 with Loan_id more than 2 for a given 
--     accountid and also order id range is 29000 & 45000 with the order amount more than 3 K.
SELECT a.*,l.loan_id,o.*
FROM account a
JOIN ordert o
ON a.account_id=o.account_id
JOIN loan l
ON a.account_id=l.account_id
WHERE YEAR(a.DATE) > 1994
AND l.loan_id > 2
AND order_id BETWEEN 29000 AND 45000
AND o.amount > 3000;

-- 03) Retrive the districtid,districtname, latest account opening date in that district, First Account opening date in that district, No of Accounts opened 
--    in that district whose districtid is odd number or account opening year is odd number.
SELECT a.district_id,dg.A2,
MAX(a.DATE) AS Latest_Date,
MIN(a.DATE) AS First_Date,
COUNT(a.account_id) AS No_of_Accounts
FROM account a
JOIN demographic dg
ON a.district_id=dg.district_id
WHERE a.district_id %2=1
OR YEAR(a.DATE) %2=1
GROUP BY a.district_id,dg.A2;

-- 04) Retrive the account details like accountid,districtid, districtname, whose accounts are opend in the month January, March, May & December.
SELECT a.account_id,a.district_id,dg.A2 AS district_name
FROM account a
JOIN demographic dg
ON a.district_id=dg.district_id
WHERE MONTH(a.DATE) IN (01,03,05,12);

-- 05) Retrive the transaction details like No of Transactions, Total Transaction Amount, Max Transaction amount,minimum transaction amount,average transaction 
--    amount for each and every combination of Account, operation and whose transaction done b/n 1995 to 2000.
SELECT account_id,operation,
COUNT(trans_id) AS No_Of_Transactions,
SUM(amount) AS Total_Transaction_Amount,
MAX(amount) AS Max_Transaction_Amount,
MIN(amount) AS Minimum_Transaction_Amount,
AVG(amount) AS Average_Transaction_Amount
FROM transaction
WHERE YEAR(DATE) BETWEEN 1995 AND 2000
GROUP BY account_id,operation;

-- 06) Retrive the account, demographic details like accountid,districtid, districtname, no. of enterpreneurs per 1000 inhabitants, no. of inhabitants 
--    whose accounts are opened in odd months with year ranges 1993 to 1997 with no. of inhabitants more than 50 K and no.of enterpreneurs per 1000 
--    inhabitants under 100.
SELECT a.account_id,a.district_id,
dg.A2 AS district_name,
dg.A14 AS no_of_Enterpreneures,
a.Date,
dg.A4 AS no_of_inhabitants
FROM account a
JOIN demographic dg
ON a.district_id=dg.district_id
WHERE YEAR(a.DATE) %2=1 
AND YEAR(a.DATE) BETWEEN 1993 AND 1997
AND dg.A4 > 50000
AND dg.A14 < 100;

-- 07) Retrive the accounts and orders details (No duplicate data), whose accounts were initiated after the year 1994 with account_id more than 4 for given 
--    order id and also order id range is 25000 & 40000 with the order amount less than 3 K.
SELECT a.*,o.*
FROM account a
JOIN ordert o
ON a.account_id=o.account_id
WHERE YEAR(a.DATE) > 1994
AND o.account_id > 4
AND o.order_id BETWEEN 25000 AND 40000
AND o.amount < 3000;

-- 08) Retrive the loan details like Total No of Loans, Total Loan Amount, Maximum Loan Amount, minimum loan amount, average loan amount for each and 
--    every combination of status & Duration which are opened in the odd months.
SELECT loan_id,STATUS , duration,
COUNT(*) AS No_of_Loans,
SUM(amount) AS Total_Loan_Amount,
MAX(amount) AS Maximum_Loan_Amount,
MIN(amount) AS Minimum_Loan_Amount,
AVG(amount) AS Average_Loan_Amount
FROM loan 
GROUP BY loan_id,duration
HAVING duration %2 = 1 ;

-- 09) Retrive the account details like accountid,account opening date, districtname,regionname whose region is either 'central Bohemia' or 'north Bohemia' 
--    and account opening year more than 1994.
SELECT a.account_id,a.DATE,dg.A2 AS district_name,dg.A3 AS region_name
FROM account a
JOIN demographic dg
ON a.district_id=dg.district_id
WHERE dg.A3 IN ('central bohemia','north Bohemia')
AND YEAR(a.date) > 1994;

-- 10) Retrive the odd account details which doesn't have loans with loan status as 'A' or 'C' and loan issued year greater than 1996 with loan amount between 
--    50 K & 55 K and payments range is 4 K to 4.5 K.
SELECT a.*,l.loan_id, l.STATUS, l.amount, l.payments,l.amount,l.payments
FROM account a
JOIN loan l
ON a.account_id=l.account_id
WHERE l.STATUS NOT IN ('A','C')
AND YEAR(a.DATE) > 1996
AND l.amount BETWEEN 50000 AND 55000
AND l.payments BETWEEN 4000 AND 4500;
