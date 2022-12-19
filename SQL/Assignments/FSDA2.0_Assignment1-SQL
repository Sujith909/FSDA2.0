
/* 

SNOWFLAKE ASSIGNMENT 1 -- https://drive.google.com/file/d/1gdDnV_zaEfJzIz8v-JzH1s9JAjqyxu5e/view

1. Load the given dataset into snowflake with a primary key to Order Date column.

https://drive.google.com/drive/folders/1YktkyxlphjA1TmIO2GUXL9elUf1s4sfn

2. Change the Primary key to Order Id Column.

3. Check the data type for Order date and Ship date and mention in what data type
it should be?

4. Create a new column called order_extract and extract the number after the last
‘–‘from Order ID column.

5. Create a new column called Discount Flag and categorize it based on discount.
Use ‘Yes’ if the discount is greater than zero else ‘No’.

6. Create a new column called process days and calculate how many days it takes
for each order id to process from the order to its shipment.

7. Create a new column called Rating and then based on the Process dates give
rating like given below.
a. If process days less than or equal to 3days then rating should be 5
b. If process days are greater than 3 and less than or equal to 6 then rating
should be 4
c. If process days are greater than 6 and less than or equal to 10 then rating
should be 3
d. If process days are greater than 10 then the rating should be 2.


*/

--1st Question was to load data set

USE WAREHOUSE DEMO_WAREHOUSE;

USE DATABASE AGENTS;

CREATE TABLE SALES_DATA
(
Order_id varchar (30),	
Order_date varchar (12),
Ship_date varchar (12),		
Ship_mode varchar (15),	
Customer_name varchar (30),
Segment	 varchar (12),
State	varchar (60),
Country	varchar (60),
Market	varchar (6),
Region	varchar (15),
Product_id varchar (20),
Category Varchar (20),
Sub_category Varchar (20),	
Product_name Varchar,
Sales	number (10,0),
Quantity number (10,0),
Discount number (10,0),	
Profit	number (10,0),
Shipping_cost number (10,0),	 
Order_priority	Varchar (10),
Year number (5,0)
) ;

-- NO PRIMARY KEY EXISTS
DESCRIBE TABLE SALES_DATA ;


-- Q2 Change the Primary key to Order Id Column

CREATE OR REPLACE TABLE SALES_DATA
(
Order_id varchar (30)  PRIMARY KEY,
Order_date varchar (12),
Ship_date varchar (12) ,		
Ship_mode varchar (15),	
Customer_name varchar (30),
Segment	 varchar (12),
State	varchar (60),
Country	varchar (60),
Market	varchar (6),
Region	varchar (15),
Product_id varchar (20),
Category Varchar (20),
Sub_category Varchar (20),	
Product_name Varchar,
Sales	number (10,0),
Quantity number (10,0),
Discount number (10,0),	
Profit	number (10,0),
Shipping_cost number (10,0),	 
Order_priority	Varchar (10),
Year number (5,0)
) ;


-- Checking if the update worked correctly And checking data type of Order date and Ship date column which is varchar currently
-- Ideally should be date data type
DESCRIBE TABLE SALES_DATA;


-- INSERT THE TABLE DATA NEXT via LOAD TABLE OPTION 
SELECT * FROM SALES_DATA;


--Q3 Updating Data type of ORDER_DATE COLUMN  

ALTER TABLE SALES_DATA RENAME COLUMN ORDER_DATE TO NEW_ORDER_DATE;
--DESCRIBE TABLE SALES_DATA_ASSIGNMENT;

ALTER TABLE SALES_DATA ADD COLUMN ORDER_DATE DATE;
--DESCRIBE TABLE SALES_DATA_ASSIGNMENT;

UPDATE SALES_DATA SET ORDER_DATE = NEW_ORDER_DATE;
--DESCRIBE TABLE SALES_DATA_ASSIGNMENT;

ALTER TABLE SALES_DATA DROP NEW_ORDER_DATE;
DESCRIBE TABLE SALES_DATA_ASSIGNMENT;
SELECT * FROM SALES_DATA;


-- SAME PROCESS ABOVE FOLLOWED FOR SHIP DATE AS WELL SINCE ITS IN VARCHAR FORMAT BUT HAS TO BE IN DATE FORMAT

ALTER TABLE SALES_DATA RENAME COLUMN SHIP_DATE TO NEW_SHIP_DATE;
ALTER TABLE SALES_DATA ADD COLUMN SHIP_DATE DATE;
UPDATE SALES_DATA SET SHIP_DATE = NEW_SHIP_DATE;
ALTER TABLE SALES_DATA DROP NEW_SHIP_DATE;
DESCRIBE TABLE SALES_DATA;
SELECT * FROM SALES_DATA;


--Q4 Create a new column called order_extract and extract the number after the last ‘–‘from Order ID column.

SELECT SPLIT_PART (ORDER_ID, '-',  3) AS ORDER_EXTRACT FROM SALES_DATA;
SELECT * FROM SALES_DATA;


-- Q5 Create a new column called Discount Flag and categorize it based on discount. Use ‘Yes’ if the discount is greater than zero else ‘No’.

SELECT ORDER_ID, SALES, DISCOUNT, 
CASE
    WHEN DISCOUNT > 0 THEN 'YES'
    ELSE 'NO'
END AS DISCOUNT_FLAG
FROM SALES_DATA;


-- Q6 Create a new column called process days and calculate how many days it takes for each order id to process from the order to its shipment.

SELECT DATEDIFF (day, ORDER_DATE, SHIP_DATE) AS DAYS_TO_DELIVER FROM SALES_DATA;



-- Q7 
-- Create a new column called Rating and then based on the Process dates give rating like given below.
-- a. If process days less than or equal to 3 days then rating should be 5
-- b. If process days are greater than 3 and less than or equal to 6 then rating should be 4
-- c. If process days are greater than 6 and less than or equal to 10 then rating should be 3
-- d. If process days are greater than 10 then the rating should be 2.

 
SELECT ORDER_DATE, SHIP_DATE, DATEDIFF (day, ORDER_DATE, SHIP_DATE) AS DAYS_TO_DELIVER,
 CASE
    WHEN DAYS_TO_DELIVER <= 3 THEN 5
    WHEN DAYS_TO_DELIVER >3 AND DAYS_TO_DELIVER <=6 THEN 4
    WHEN DAYS_TO_DELIVER >6 AND DAYS_TO_DELIVER <=10 THEN 3
    WHEN DAYS_TO_DELIVER > 10 THEN 2
    ELSE DAYS_TO_DELIVER 
END AS RATING
FROM SALES_DATA;
 

SELECT * FROM SALES_DATA;


-- Create a new view to incorporate new columns we need 
 
CREATE OR REPLACE VIEW SALES_DATA_ASSIGNMENT_DONE AS
 
SELECT ORDER_ID, PRODUCT_NAME, CATEGORY, SUB_CATEGORY, PROFIT, QUANTITY, DATEDIFF (day, ORDER_DATE, SHIP_DATE) AS DAYS_TO_DELIVER, LEFT (ORDER_DATE, 4) AS YEAR_ONLY,
    CASE
        WHEN DAYS_TO_DELIVER <= 3 THEN 5
        WHEN DAYS_TO_DELIVER >3 AND DAYS_TO_DELIVER <=6 THEN 4
        WHEN DAYS_TO_DELIVER >6 AND DAYS_TO_DELIVER <=10 THEN 3
        WHEN DAYS_TO_DELIVER > 10 THEN 3
        ELSE DAYS_TO_DELIVER 
    END AS RATING,


    CASE
        WHEN DISCOUNT > 0 THEN 'YES'
        ELSE 'NO'
    END AS DISCOUNT_FLAG
    
    
FROM SALES_DATA;


SELECT * FROM SALES_DATA_ASSIGNMENT_DONE;
