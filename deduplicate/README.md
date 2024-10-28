### 1. Overview
This project was created to:

Implement data pipelines using DBT with a data lake structure.
Keep data consistent and without duplication using macros and deduplication models.
Automate ETL processes and optimize data documentation.

### 2. Project structure
The project is organized into:

Models: Contains all the data transformation models. 
Macros: Reusable functions for tasks such as deduplication (deduplicate_iceberg) and error control.
Docs: Contains the documentation generated with dbt docs.

### 3. Requirements and configurations
Dependencies
DBT >= 1.8.8
Adapters and Packages:
Snowflake Adapter
dbt_utils

Clone the repository:

bash
git clone <URL_DO_REPOSITORIO>
cd nome_do_projeto

Configure profiles.yml: 

In your user directory, configure the profiles.yml file with your database credentials. Example for Snowflake:
deduplicate:
  outputs:
    dev:
      account: lwnwyeb-cr66126
      database: DATA
      password: (password.txt)
      role: ACCOUNTADMIN
      schema: PUBLIC
      threads: 10
      type: snowflake
      user: marianatestbrazil
      warehouse: COMPUTE_WH
  target: dev

### 4. Model execution:

bash
python -m venv .ven
.\.venv\Scripts\activate
python -m pip install dbt-core dbt-snowflake
enter the dbt folder dbt_project\deduplicate
dbt deps
dbt debug
dbt run -s .\models\main\customers_main.sql

### 5. Visualização da Documentação:

bash
dbt docs generate
dbt docs serve

### 6. Viewing the documentation:

Deduplication macro
The deduplicate_iceberg macro is used to remove duplicate records in specific tables. It ensures that only the most recent record, based on the updated_at column, is kept.

Example of use:

sql
Copy code
{{ deduplicate_iceberg(ref('orders'), 'updated_at', 'order_id') }}

### 7. Known Problems and Solutions:

Connection error with Snowflake: Check profiles.yml and user permissions in Snowflake.

Documentation does not appear: Make sure that dbt docs generate was run after the changes to schema.yml.

Errors when running due to nulls or duplicates: When running the project without looking at the \models\test folder, deactivate the models in this folder so that there is no error when running. when you want to use them, just enable them.

Go to the file dbt_project.yml
    tests:
      orders_null_main:
        enabled: false
      customers_new_column_main:
        enabled: false

