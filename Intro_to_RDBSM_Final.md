
# Overview
## Scenario
In this scenario, you have recently been hired as a Data Engineer by a New York based coffee shop chain that is looking to expand nationally by opening a number of franchise locations. As part of their expansion process, they want to streamline operations and revamp their data infrastructure.

Your job is to design their relational database systems for improved operational efficiencies and to make it easier for their executives to make data driven decisions.

Currently their data resides in several different systems: accounting software, suppliers’ databases, point of sales (POS) systems, and even spreadsheets. You will review the data in all of these systems and design a central database to house all of the data. You will then create the database objects and load them with source data. Finally, you will create subsets of data that your business partners require, export them, and then load them into staging databases that use different RDBMS.

## Tools
In this project, you will use PostgreSQL Database, IBM Db2 Database, and MySQL. 

# Project

## Task 3: Create an ERD

Create a new database named COFFEE, view the schemas in the new COFFEE database, and then start a new ERD project. Add a table to the ERD for the sale transactions entity
 ![task3a](https://user-images.githubusercontent.com/47308962/193956881-90b58495-8d24-41e5-a8a4-95a54f8d878a.jpg)

Add a table to the ERD for the product entity using the information in the following table.
![task3b](https://user-images.githubusercontent.com/47308962/193957149-1e808f6f-6a49-4fa3-ad8b-2cb8ca1c92fc.jpg)

## Task 4: Normalize tables

Review the data in the sales transaction table. Note that the transaction id column does not contain unique values because some transactions include multiple products.

Determine which columns should be stored in a separate table to remove the repeating rows and to put this table into second normal form.

Add a new table named sales_detail to the ERD, define the columns in the new table, and delete the moved columns from the sales transaction table, leaving a matching column in each of two tables to later create a relationship between them.

![task4a](https://user-images.githubusercontent.com/47308962/193957630-d219cd81-6935-43b7-a66b-94c4e67aa8a7.jpg)

Review the data in the product table. Note that the product category and product type columns contain redundant data.

Determine which columns should be stored in a separate table to reduce redundant data and to put this table into second normal form.

Add a new table named product_type to the ERD, define the columns in the new table, and delete the moved columns from the product table, , leaving a matching column in each of two tables to later create a relationship between them.

![task4b](https://user-images.githubusercontent.com/47308962/193957677-2cba6283-cf47-43d3-bac1-5919e7157e35.jpg)


## Task 5: Define keys and relationships
After normalizing your tables, you can define their primary keys and define relationships between the tables in your ERD.

Identify an appropriate column in each table to be a primary key and create the primary keys in the tables in your ERD.


![task5a](https://user-images.githubusercontent.com/47308962/193957681-0982981c-694c-4128-ba7c-719a82163b04.jpg)

Identify the relationships between the following pairs of tables and then create the relationships in your ERD:

sales_detail to sales_transaction
sales_detail to product
product to product_type

![task5b](https://user-images.githubusercontent.com/47308962/193957687-c433f58c-fed8-4645-8a2c-23b57ccf81f2.jpg)

## Task 6: Create database objects by generating and running the SQL script from the ERD Tool
Now that your design is complete, you will generate an SQL script from your ERD which you could use to create your database schema. For the purposes of this project, you will then use a provided SQL script to ensure that you will be able to successfully load the sample data into the schema. Finally, you will load the existing data from the various data sources into your new database schema.

Use the Generate SQL functionality in the ERD Tool to create an SQL script from your ERD.

Download the GeneratedScript.sql file below to your local computer storage.

In pgAdmin, open the Query Tool, upload and open the GeneratedScript.sql file from your local computer storage, and then execute the script to create the tables defined in the ERD. Verify that the tables now exist in the public schema of the COFFEE database.

![task6a](https://user-images.githubusercontent.com/47308962/193957692-e0561062-8181-4279-815a-3dabf0f798ed.jpg)

Download the CoffeeData.sql file below to your local computer storage.

CoffeeData.sql
In pgAdmin, open another instance of the Query Tool, upload and open the CoffeeData.sql file from your local computer storage, and then execute the script to populate the tables you just created.

In pgAdmin, view the first 100 rows of the sales_detail table.

![task6b](https://user-images.githubusercontent.com/47308962/193957698-3f7ed917-691d-4701-a67d-722d12b546a0.jpg)

## Task 7: Create a view and export the data
The external payroll company have requested a list of employees and the locations at which they work. This should not include the CEO or CFO who own the company. In this task, you will create a view in your PostgreSQL database that returns this information and export the results to a CSV file.

In your COFFEE database, create a new view named staff_locations_view using the following SQL:

    SELECT staff.staff_id, staff.first_name, staff.last_name, staff.location
    FROM staff
    WHERE "position" NOT IN ('CEO', 'CFO');


![task7](https://user-images.githubusercontent.com/47308962/193957708-03b2122b-d56b-4216-95c0-a756c6caa17b.jpg)

## Task 8: Create a materialized view and export the data
A marketing consultant requires access to your product data in their MySQL database for a marketing campaign. You will create a materialized view in your PostgreSQL database that returns this information and export the results to a CSV file.

In your COFFEE database, create a new materialized view named product_info_m-view using the following SQL:

    SELECT product.product_name, product.description, product_type.product_category
    FROM product
    JOIN product_type
    ON product.product_type_id = product_type.product_type_id;

View all the rows returned from the view.
![task8](https://user-images.githubusercontent.com/47308962/193957717-154ca1f3-b392-4f1e-bdc3-fbe941c2718f.jpg)

## Task 9: Import data into a Db2 database

The external payroll company have asked you to upload the staff location information to their Db2 database.

In a new browser tab, go to https://cloud.ibm.com/login, log in using your credentials, and then open a console for your Db2 on Cloud instance that you created earlier in this course.

Use the Load Data feature to load a new table named STAFF_LOCATIONS with the staff location information saved in the staff_locations_view.csv file that you exported from the view you created in Task 7.

Explore the new table and then view the data in it.

![task9](https://user-images.githubusercontent.com/47308962/193958022-3c64099c-2b6a-4b6c-a0c8-531ff9f9fd23.jpg)

## Task 10: Import data into a MySQL database
The marketing consultant has asked you to upload the product information to their MySQL database.

In the terminal from the side-by-side Cloud IDE, use the start_mysql command to start a My SQL service session in the Cloud IDE.

Use the browser weblink to open phpMyAdmin in a new tab in your browser.

In phpMyAdmin, create a new database named coffee_shop_products, and then import the product information saved in the product_info_m-view.csv file from your materialized view into a new table in the coffee_shop_products database.

Browse the contents of the new table.

![task10](https://user-images.githubusercontent.com/47308962/193957728-57b762fb-f30c-412a-9675-72d245d62a2b.jpg)