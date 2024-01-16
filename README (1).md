# Crowdfunding_ETL
Project 2 ETL

## Create the Category and Subcategory DataFrames

1. Extract and transform the crowdfunding.xlsx Excel data to create a category DataFrame that has the following columns:

   * A "category_id" column that has entries going sequentially from "cat1" to "catn", where n is the number of unique categories

   * A "category" column that contains only the category titles

   * The following image shows this category DataFrame:

        ![category_df](Images/category_df.png)

2. Export the category DataFrame as category.csv and save it to your GitHub repository.

    ![cat_subcat_export](Images/cat_subcat_export.png)

3. Extract and transform the crowdfunding.xlsx Excel data to create a subcategory DataFrame that has the following columns:

   * A "subcategory_id" column that has entries going sequentially from "subcat1" to "subcatn", where n is the number of unique subcategories

   * A "subcategory" column that contains only the subcategory titles

   * The following image shows this subcategory DataFrame:

       ![subcategory_df](Images/subcategory_df.png)

4. Export the subcategory DataFrame as subcategory.csv and save it to your GitHub repository.

    ![cat_subcat_export](Images/cat_subcat_export.png)

## Create the Campaign DataFrame

1. Extract and transform the crowdfunding.xlsx Excel data to create a campaign DataFrame has the following columns:

   * The "cf_id" column

   * The "contact_id" column

   * The "company_name" column

   * The "blurb" column, renamed to "description"

   * The "goal" column, converted to the float data type

   * The "pledged" column, converted to the float data type

   * The "outcome" column

   * The "backers_count" column

   * The "country" column

   * The "currency" column

   * The "launched_at" column, renamed to "launch_date" and with the UTC times converted to the datetime format

   * The "deadline" column, renamed to "end_date" and with the UTC times converted to the datetime format

   * The "category_id" column, with unique identification numbers matching those in the "category_id" column of the category DataFrame

   * The "subcategory_id" column, with the unique identification numbers matching those in the "subcategory_id" column of the subcategory DataFrame

   * The following image shows this campaign DataFrame:

        ![campaign_df](Images/campaign_df.png)

2. Export the campaign DataFrame as campaign.csv and save it to your GitHub repository.

    ![campaign_export](Images/campaign_export.png)

## Create the Contacts DataFrame

1. Choose one of the following two options for extracting and transforming the data from the contacts.xlsx Excel data:

   * Option 1: Use Python dictionary methods.

2. If you chose Option 1, complete the following steps:

   * Import the contacts.xlsx file into a DataFrame.

   * Iterate through the DataFrame, converting each row to a dictionary.

   * Iterate through each dictionary, doing the following:

        * Extract the dictionary values from the keys by using a Python list comprehension.

        * Add the values for each row to a new list.

   * Create a new DataFrame that contains the extracted data.

   * Split each "name" column value into a first and last name, and place each in a new column.

   * Clean and export the DataFrame as contacts.csv and save it to your GitHub repository.

        ![contacts_export](Images/contacts_export.png)

4. Check that your final DataFrame resembles the one in the following image:

    ![contacts_df](Images/contacts_df.png)

## Create the Crowdfunding Database

1. Inspect the four CSV files, and then sketch an ERD of the tables by using QuickDBDLinks

    ![Table_Structure](Images/Table_Structure.png)

2. Use the information from the ERD to create a table schema for each CSV file.

    Note: Remember to specify the data types, primary keys, foreign keys, and other constraints.

    ```sql
    CREATE TABLE categories (
	    category_id VARCHAR(7) PRIMARY KEY,
	    category VARCHAR(30) NOT NULL
    );

    CREATE TABLE subcategories (
	    subcategory_id VARCHAR(10) PRIMARY KEY,
	    subcategory VARCHAR(30) NOT NULL
    );

    CREATE TABLE contacts (
        contact_id VARCHAR(4) PRIMARY KEY,
        first_name VARCHAR(30) NOT NULL,
        last_name VARCHAR(30) NOT NULL,
        email VARCHAR(255) NOT NULL
    );

    CREATE TABLE campaigns (
        cf_id VARCHAR(4) PRIMARY KEY,
        contact_id VARCHAR(4) NOT NULL,
        company_name VARCHAR(255) NOT NULL,
        description VARCHAR(255) NOT NULL,
        gaol MONEY NOT NULL,
        pledged MONEY,
        outcome VARCHAR(10),
        backers_count INT,
        country VARCHAR(2) NOT NULL,
        currency VARCHAR(3) NOT NULL,
        launch_date DATE NOT NULL,
        end_date DATE NOT NULL,
        category_id VARCHAR(7) NOT NULL,
        subcategory_id VARCHAR(10) NOT NULL,
        FOREIGN KEY (contact_id) REFERENCES contacts (contact_id),
        FOREIGN KEY (category_id) REFERENCES categories (category_id),
        FOREIGN KEY (subcategory_id) REFERENCES subcategories (subcategory_id)
    );
    ```

3. Save the database schema as a Postgres file named crowdfunding_db_schema.sql, and save it to your GitHub repository.

4. Create a new Postgres database, named crowdfunding_db.

    ![Database](Images/Database.png)

5. Using the database schema, create the tables in the correct order to handle the foreign keys.

6. Verify the table creation by running a SELECT statement for each table.

    ```sql
    SELECT * FROM campaigns;

    SELECT * FROM categories;

    SELECT * FROM contacts;

    SELECT * FROM subcategories;
    ```

7. Import each CSV file into its corresponding SQL table.

    Additional line of code (used in SQL Challenge) added to support a change in the datatype for the DATE columns prior to importing csv to the campaigns table:

    ```sql
    SET datestyle TO iso, mdy;
    ```

    ```sql
    COPY categories(
    category_id,
    category
    )
    FROM '/Users/julie/psql_temp2/category.csv'
    DELIMITER ','
    CSV HEADER;

    COPY subcategories(
        subcategory_id,
        subcategory
    )
    FROM '/Users/julie/psql_temp2/subcategory.csv'
    DELIMITER ','
    CSV HEADER;

    COPY contacts(
        contact_id,
        first_name,
        last_name,
        email
    )
    FROM '/Users/julie/psql_temp2/contacts.csv'
    DELIMITER ','
    CSV HEADER;

    SET datestyle TO iso, mdy;
    COPY campaigns (
        cf_id,
        contact_id,
        company_name,
        description,
        gaol,
        pledged,
        outcome,
        backers_count,
        country,
        currency,
        launch_date,
        end_date,
        category_id,
        subcategory_id
    )
    FROM '/Users/julie/psql_temp2/campaign.csv'
    DELIMITER ','
    CSV HEADER;
    ```


8. Verify that each table has the correct data by running a SELECT statement for each.

    ![SELECT_Campaigns](Images/SELECT_Campaigns.png)
