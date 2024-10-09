# ELT Pipeline Project: dbt, Snowflake, and Airflow

## Project Overview

This project demonstrates how to build an Extract, Load, Transform (ELT) pipeline using **dbt** (data build tool), **Snowflake** as the data warehouse, and **Airflow** for orchestration. The pipeline ingests sample data, applies transformations, and prepares the data for analysis.

### Key Components
- **dbt**: For data transformation and modeling.
- **Snowflake**: As the cloud data warehouse.
- **Airflow**: For workflow orchestration.

## Project Structure

project-root/
│
├── models/
│   ├── marts/
│   │   ├── fct_orders.sql
│   │   ├── int_order_items.sql
│   │   └── int_order_items_summary.sql
│   └── staging/
│       ├── stg_tpch_orders.sql
│       └── stg_tpch_line_items.sql
│
├── macros/
│   └── pricing.sql
│
├── tests/
│   ├── fct_orders_discount.sql
│   └── fct_orders_date_valid.sql
│
├── dbt_dag.py
├── Dockerfile
└── requirements.txt


## Setup Instructions

### Prerequisites

- **Docker**: Ensure Docker is installed on your machine.
- **Airflow**: Set up an Airflow instance using Docker or any other method.
- **Snowflake Account**: Access to a Snowflake account for data storage.

### Step 1: Configure Snowflake Environment

Run the following SQL commands in your Snowflake environment to set up the necessary database, warehouse, and role:

```sql
-- create accounts
use role accountadmin;

create warehouse dbt_wh with warehouse_size='x-small';
create database if not exists dbt_db_saa;
create role if not exists dbt_role_saa;

grant role dbt_role to user <your_username>;
grant usage on warehouse dbt_wh_saa to role dbt_role_saa;
grant all on database dbt_db_saa to role dbt_role_saa;

use role dbt_role;

create schema if not exists dbt_db.dbt_schema_saa;

## Setup Instructions

### Step 2: Configure dbt Profile

Update the `profiles.yml` file to connect dbt with Snowflake.

### Step 3: Build the Models and Tests

Create the necessary SQL files for staging and transformation models. Implement generic and singular tests as shown in the project structure.

### Step 4: Update Docker Configuration

Modify the `Dockerfile` to include dbt dependencies:

```docker
RUN python -m venv dbt_venv && source dbt_venv/bin/activate && \
    pip install --no-cache-dir dbt-snowflake && deactivate
### Step 5: Set Up Airflow

1. **Update the `requirements.txt`:**

2. **Add the Snowflake connection in the Airflow UI with the following configuration:**

```json
{
  "account": "<account_locator>-<account_name>",
  "warehouse": "dbt_wh_saa",
  "database": "dbt_db_saa",
  "role": "dbt_role_saa",
  "insecure_mode": false
}
### Step 6: Create the DAG

Configure `dbt_dag.py` to define the Airflow DAG for orchestrating the dbt models.

## Usage

1. **Start Airflow**: Launch the Airflow instance to manage your workflows.
2. **Trigger the DAG**: Navigate to the Airflow UI and trigger the `dbt_dag`.
3. **Monitor Runs**: Check the status of your runs and logs for troubleshooting.

## Testing

Run the singular tests using dbt:

```bash
dbt test

##
Here’s the updated README.md section including Step 6 and the usage instructions:

markdown
Copy code
### Step 6: Create the DAG

Configure `dbt_dag.py` to define the Airflow DAG for orchestrating the dbt models.

## Usage

1. **Start Airflow**: Launch the Airflow instance to manage your workflows.
2. **Trigger the DAG**: Navigate to the Airflow UI and trigger the `dbt_dag`.
3. **Monitor Runs**: Check the status of your runs and logs for troubleshooting.

## Testing

Run the singular tests using dbt:

dbt test

## Contributing
Feel free to submit issues, suggestions, or pull requests. Please ensure to follow the coding standards and maintain documentation.

