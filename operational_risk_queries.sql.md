**-- PROJECT: Operational Risk and Loss Analysis**

**-- SECTION 1: Create the table**



CREATE TABLE Operational\_Risk (

&#x20;   Event\_ID INTEGER PRIMARY KEY,

&#x20;   Event\_Date TEXT,

&#x20;   Risk\_Category TEXT,

&#x20;   Description TEXT,

&#x20;   Loss\_Amount REAL,

&#x20;   Status TEXT

);



**-- SECTION 2: Insert sample data**



INSERT INTO Operational\_Risk

(Event\_ID, Event\_Date, Risk\_Category, Description, Loss\_Amount, Status)

VALUES

(1, '2026-10-15', 'Process Error', 'Incorrect transaction entry', 1200, 'Closed'),

(2, '2026-10-15', 'System Failure', 'Temporary system outage', 3500, 'Closed'),

(3, '2026-10-16', 'Process Error', 'Duplicate payment', 2400, 'Under Review'),

(4, '2026-10-16', 'Fraud', 'Suspicious transaction', 8000, 'Under Review'),

(5, '2026-10-17', 'System Failure', 'Reporting system interruption', 1800, 'Closed'),

(6, '2026-10-17', 'Process Error', 'Incorrect account update', 600, 'Open');





**-- SECTION 3: View all operational risk events**



SELECT \*

FROM Operational\_Risk;





**-- SECTION 4: Calculate total losses**



SELECT

&#x20;   SUM(Loss\_Amount) AS Total\_Loss

FROM Operational\_Risk;





**-- SECTION 5: Analyze losses by category**



SELECT

&#x20;   Risk\_Category,

&#x20;   COUNT(\*) AS Number\_of\_Events,

&#x20;   SUM(Loss\_Amount) AS Total\_Loss,

&#x20;   AVG(Loss\_Amount) AS Average\_Loss,

&#x20;   MAX(Loss\_Amount) AS Largest\_Loss

FROM Operational\_Risk

GROUP BY Risk\_Category

ORDER BY Total\_Loss DESC;



**-- SECTION 6: Analyze events by status**



SELECT

&#x20;   Status,

&#x20;   COUNT(\*) AS Number\_of\_Events,

&#x20;   SUM(Loss\_Amount) AS Total\_Loss

FROM Operational\_Risk

GROUP BY Status

ORDER BY Total\_Loss DESC;





**-- SECTION 7: Find events with losses of 2000 or more**



SELECT

&#x20;   Event\_ID,

&#x20;   Event\_Date,

&#x20;   Risk\_Category,

&#x20;   Loss\_Amount,

&#x20;   Status

FROM Operational\_Risk

WHERE Loss\_Amount >= 2000

ORDER BY Loss\_Amount DESC;





**-- SECTION 8: Find events that are not closed**



SELECT

&#x20;   Event\_ID,

&#x20;   Event\_Date,

&#x20;   Risk\_Category,

&#x20;   Loss\_Amount,

&#x20;   Status

FROM Operational\_Risk

WHERE Status != 'Closed'

ORDER BY Loss\_Amount DESC;





**-- SECTION 9: Analyze daily losses**



SELECT

&#x20;   Event\_Date,

&#x20;   COUNT(\*) AS Number\_of\_Events,

&#x20;   SUM(Loss\_Amount) AS Daily\_Loss

FROM Operational\_Risk

GROUP BY Event\_Date

ORDER BY Event\_Date;





**-- SECTION 10: TOP 3 EVENTS BY LOSS AMOUNT**

**-- Purpose: Identify the operational risk events**

**-- with the largest financial losses.**





SELECT

Event\_ID,

Event\_Date,

Risk\_Category,

Description,

Loss\_Amount,

Status

FROM Operational\_Risk

ORDER BY Loss\_Amount DESC

LIMIT 3;





**-- SECTION 11: LOSS PERCENTAGE BY RISK CATEGORY**

**-- Purpose: Calculate each category's share of**

**-- total operational losses.**





SELECT

Risk\_Category,

SUM(Loss\_Amount) AS Total\_Loss,

ROUND(

100.0 \* SUM(Loss\_Amount) /

(SELECT SUM(Loss\_Amount)

FROM Operational\_Risk),

2

) AS Loss\_Percentage

FROM Operational\_Risk

GROUP BY Risk\_Category

ORDER BY Total\_Loss DESC;





**-- SECTION 12: OPEN AND UNDER-REVIEW EVENTS**

**-- Purpose: Identify operational risk events that**

**-- have not yet been closed.**





SELECT

Event\_ID,

Event\_Date,

Risk\_Category,

Description,

Loss\_Amount,

Status

FROM Operational\_Risk

WHERE Status != 'Closed'

ORDER BY Loss\_Amount DESC;





**-- SECTION 13: DAILY LOSS MONITORING**

**-- Purpose: Monitor total losses and event counts**

**-- for each date.**



SELECT

Event\_Date,

COUNT(\*) AS Number\_of\_Events,

SUM(Loss\_Amount) AS Total\_Daily\_Loss,

AVG(Loss\_Amount) AS Average\_Loss

FROM Operational\_Risk

GROUP BY Event\_Date

ORDER BY Event\_Date;





**-- SECTION 14: OPERATIONAL RISK SUMMARY**

**-- Purpose: Create an overall summary of event**

**-- volumes, losses, and event status.**



SELECT

COUNT(\*) AS Total\_Events,

SUM(Loss\_Amount) AS Total\_Loss,

ROUND(AVG(Loss\_Amount), 2) AS Average\_Loss,

MAX(Loss\_Amount) AS Largest\_Loss,

SUM(

CASE WHEN Status = 'Closed'

THEN 1 ELSE 0 END

) AS Closed\_Events,

SUM(

CASE WHEN Status = 'Under Review'

THEN 1 ELSE 0 END

) AS Events\_Under\_Review,

SUM(

CASE WHEN Status = 'Open'

THEN 1 ELSE 0 END

) AS Open\_Events

FROM Operational\_Risk;



**-- SECTION 15: DATA QUALITY CHECK**

**-- Purpose: Identify records with missing or invalid**

**-- key information.**





SELECT \*

FROM Operational\_Risk

WHERE Event\_Date IS NULL

OR Risk\_Category IS NULL

OR Loss\_Amount IS NULL

OR Status IS NULL

OR Loss\_Amount < 0;





**-- SECTION 16: LOSS ANALYSIS BY STATUS**

**-- Purpose: Compare losses across event statuses.**





SELECT

Status,

COUNT(\*) AS Number\_of\_Events,

SUM(Loss\_Amount) AS Total\_Loss,

ROUND(AVG(Loss\_Amount), 2) AS Average\_Loss,

MAX(Loss\_Amount) AS Largest\_Loss

FROM Operational\_Risk

GROUP BY Status

ORDER BY Total\_Loss DESC;





**-- SECTION 17: HIGH-LOSS EVENTS UNDER REVIEW**

**-- Purpose: Identify significant losses that still**

**-- require investigation.**





SELECT

Event\_ID,

Event\_Date,

Risk\_Category,

Description,

Loss\_Amount,

Status

FROM Operational\_Risk

WHERE Status = 'Under Review'

AND Loss\_Amount >= 2000

ORDER BY Loss\_Amount DESC;





**-- SECTION 18: RISK CATEGORY PRIORITIZATION**

**-- Purpose: Rank risk categories by total financial loss.**





SELECT

Risk\_Category,

COUNT(\*) AS Number\_of\_Events,

SUM(Loss\_Amount) AS Total\_Loss,

ROUND(

100.0 \* SUM(Loss\_Amount) /

(SELECT SUM(Loss\_Amount)

FROM Operational\_Risk),

2

) AS Share\_of\_Total\_Loss

FROM Operational\_Risk

GROUP BY Risk\_Category

ORDER BY Total\_Loss DESC;





**-- SECTION 19: OUTSTANDING LOSS EXPOSURE**

**-- Purpose: Calculate losses associated with events**

**-- that have not been closed.**





SELECT

COUNT(\*) AS Outstanding\_Events,

SUM(Loss\_Amount) AS Outstanding\_Loss,

ROUND(

100.0 \* SUM(Loss\_Amount) /

(SELECT SUM(Loss\_Amount)

FROM Operational\_Risk),

2

) AS Share\_of\_Total\_Loss

FROM Operational\_Risk

WHERE Status != 'Closed';





**-- SECTION 20: MANAGEMENT ATTENTION LIST**

**-- Purpose: Create a concise list of unresolved**

**-- events with losses of at least 2,000.**



SELECT

Event\_ID,

Event\_Date,

Risk\_Category,

Loss\_Amount,

Status

FROM Operational\_Risk

WHERE Status != 'Closed'

AND Loss\_Amount >= 2000

ORDER BY Loss\_Amount DESC;





**-- SECTION 21: FINAL PROJECT VALIDATION**

**-- Purpose: Verify the total number of events and**

**-- the total financial loss in the dataset.**





SELECT

COUNT(\*) AS Total\_Events,

SUM(Loss\_Amount) AS Total\_Loss,

COUNT(DISTINCT Risk\_Category) AS Risk\_Categories,

COUNT(DISTINCT Status) AS Event\_Statuses

FROM Operational\_Risk;





**-- SECTION 22: FINAL EVENT REGISTER**

**-- Purpose: Display the complete event register in**

**-- chronological order for review and reporting.**





SELECT

Event\_ID,

Event\_Date,

Risk\_Category,

Description,

Loss\_Amount,

Status

FROM Operational\_Risk

ORDER BY Event\_Date, Event\_ID;





***Operational Risk and Loss Analysis***



1\. Project Objective



The objective of this project was to analyse operational risk events, quantify financial losses, monitor outstanding cases, and identify areas requiring further investigation.





2\. Dataset Overview



The sample dataset contains six operational risk events across three risk categories: Process Error, System Failure, and Fraud.



Total recorded loss: 17,500



Total events: 6



Events closed: 3



Events under review: 2



Open events: 1



Note: The dataset is fictional and intended for learning purposes only.





3\. Key Findings



Loss by Risk Category



Fraud: 8,000 (45.71% of total losses)



System Failure: 5,300 (30.29%)



Process Error: 4,200 (24.00%)



Fraud accounted for the largest share of recorded losses and would be a priority for further investigation in this sample.



Outstanding Loss Exposure



Three events remained unresolved, with combined losses of 11,000, representing 62.86% of total recorded losses.



Two events under review had individual losses of at least 2,000 and were identified for management attention.



Daily Loss Monitoring



The highest daily loss occurred on 16 October 2026, totalling 10,400 across two events.





4\. SQL Techniques Used



SELECT and WHERE for data retrieval and filtering



ORDER BY and LIMIT for prioritising high-loss events



GROUP BY for aggregating events by category, date, and status



SUM, COUNT, AVG, and MAX for loss analysis



CASE WHEN for conditional status counts



Subqueries for calculating percentages of total losses



Basic data quality checks for missing values and negative losses







5\. Conclusion



This project demonstrates how SQL can support operational risk monitoring, loss analysis, data quality checks, and the prioritisation of unresolved risk events. The analysis provides a structured basis for reporting and further investigation.



The findings are illustrative and do not represent actual bank risk exposures.











CV:

Operational Risk and Loss Analysis — SQL Project



Built a sample operational risk event database using SQL.



Analysed financial losses by risk category, event date, and status.



Identified high-loss events and outstanding loss exposure using filtering, aggregation, and subqueries.



Performed basic data quality checks and prepared management-oriented risk summaries.





