# Taxi trip data NYC

## About Dataset
The NYC Cabs dataset provides insights into the differences between traditional taxis and Uber in New York City. While both charge fares based on time and distance, taxis have fixed surcharges during certain times, while Uber implements surge pricing during periods of high demand. Taxis accept street hails and offer regulated fares for airport trips, whereas Uber requires users to book through the app and calculates fares based on dynamic pricing algorithms. Additionally, payment methods and tipping practices differ between the two services. Overall, the dataset sheds light on the nuances of transportation options in a bustling city like New York.

## Case Understanding
The business team has provided a data project focusing on analyzing the NYC Taxi Trip data. Their key requirements include deriving insights such as monthly total passengers, monthly transactions per payment type, and monthly trip distance per rate code. To fulfill these requirements, the data will be transformed using dbt, with the raw data stored in the raw.nyc_taxi_trip table. Multiple models will be generated based on the specific analytical needs, ensuring that each model includes thorough testing for at least 60% of its columns. Additionally, it is mandatory to provide descriptions for the table and its columns to ensure proper governance. As part of the project, the team will also create custom macros and tests to enhance the data transformation process and ensure data quality.

## Load and Transform Raw data
### Data Ingestion from CSV to BigQuery
First, let's create a virtual environment in the project folder.

![venv](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/bigquery/create-venv.png)

Then, let's create a dataset. We'll name the dataset 'weekly'.

![dataset](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/bigquery/create-weekly-dataset.png)

Next, activate the venv and run `dbt init`. The goal is to ensure that the dependencies and Python packages needed by our dbt project are well isolated from the global Python environment, while `dbt init` sets up the basic structure of the required project. Name our project 'taxi'.

![dbt-init](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/bigquery/activate-vevn-dbt-init.png)

We select BigQuery as the database to be used. Also, choose [2] service_account as the authentication method, input the GCP project ID, dataset, threads, and location. Then, enter the 'taxi' project folder, run 'dbt debug', and make sure all checks have passed.

![dbt-debug](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/bigquery/dbt-debug-weekly.png)

Move the CSV dataset file into the taxi\seeds folder. Then, run 'dbt seed'. The function of 'dbt seed' is to load initial data or seed data into a database table. Make sure that in the BigQuery dataset 'weekly', the table 'taxi' already exists.

![dbt-seed](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/bigquery/dbt-seed-weekly.png)

![taxi-table](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/bigquery/taxi-table.png)

### Create dbt Models
In this case, we are tasked with developing 5 models. Among them, 3 are mandatory tasks, namely Monthly Total Passengers, Monthly Transactions per Payment Type, and Monthly Trip Distance per Rate Code. Meanwhile, the Extra per Trip Type and Congestion Surcharge per Rate Code models are additional ones.

![list](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/dbt-models.png)

#### Monthly Total Passengers
![terminal](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/monthly_total_passengers.png)

![passengers](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/bq_monthly_total_passengers.png)

From the table, we can observe data representing the monthly passenger count from 2008 to 2021. Here are some insights that can be derived:

1. **Passengers per Month**: The table illustrates the monthly count of passengers from 2008 to 2021. It shows that the number of passengers varies from month to month and from year to year.
2. **Growth Trends**: The data indicates growth trends in passenger numbers from 2008 to 2021. It's noticeable that the number of passengers tends to increase over the years, with the highest surge occurring in July 2021.
3. **Upward Trend**: Despite normal monthly fluctuations, there's a general upward trend in passenger numbers over time. The significant increase in July 2021 might be attributed to seasonal factors or specific events affecting transportation demand.
4. **Potential for Further Analysis**: This data can be used for further analysis, such as identifying factors influencing passenger demand, predicting future trends, and developing more effective marketing or operational strategies to optimize fleet utilization.

#### Monthly Transactions per Payment Type
![terminal](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/transactions_per_payment_type.png)

![transactions](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/bq_transactions_per_payment_type.png)

From the provided table, we can derive the following insights:

1. **Payment Type Distribution**: The data showcases the distribution of payment types utilized for taxi trips over different months and years. The predominant payment types are Cash and Credit Card, with occasional occurrences of Dispute, No Charge, and Unknown.
2. **Variability in Transaction Amounts**: The monthly transaction amounts vary significantly across payment types and time periods. For instance, in July 2021, the transaction amounts for Cash and Credit Card payments are substantially higher compared to other months, indicating a potential surge in taxi usage during that period.
3. **Prevalence of Electronic Payments**: Credit Card payments appear to be more prevalent in recent years, as indicated by the higher transaction amounts compared to Cash payments in 2021. This trend may suggest a shift towards electronic payment methods among taxi passengers.
4. **Anomalies**: There are instances of Dispute, No Charge, and Unknown payment types, which may represent exceptional cases or errors in the payment processing system. Further investigation may be required to understand the nature of these transactions and their impact on overall revenue and operations.
5. **Seasonal Variations**: The data reflects seasonal variations in taxi transactions, with higher transaction amounts observed during certain months (e.g., July 2021). These fluctuations could be influenced by factors such as tourism, events, or changes in consumer behavior.
Overall, this data provides valuable insights into the payment behavior of taxi passengers over time, highlighting trends, anomalies, and potential areas for further analysis and optimization.

#### Monthly Trip Distance per Rate Code
![terminal](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/trip-distance-per-rate-code.png)

![distance](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/bq-trip-distance-per-rate-code.png)

Based on the provided data, the following insights can be derived:

1. Variation in Trip Types: The data reveals different types of trip rates, including Standard Rate, Negotiated Fare, JFK, Nassau or Westchester, and Newark. Each rate code corresponds to specific trip categories, such as standard city trips, negotiated fares, trips to airports (JFK and Newark), and trips to nearby counties (Nassau or Westchester).
2. Trip Distance Distribution: The monthly trip distance varies across different rate codes and time periods. For instance, in July 2021, trips under the Standard Rate exhibit significantly higher distances compared to other rate codes, indicating a higher frequency of longer trips during that month.
3. Seasonal Trends: There are fluctuations in trip distances over time, with variations observed across different months and years. For example, in June 2021, the trip distance for Negotiated Fare is recorded as 0.0, suggesting a potential anomaly or a change in trip patterns during that period.
4. Airport Trips: Trips to airports (JFK and Newark) tend to have longer distances compared to standard city trips, as evidenced by the higher trip distances recorded for these rate codes. This could be attributed to the distance between city locations and airport terminals.
5. Operational Insights: The data provides insights into the operational aspects of taxi services, such as the distribution of trip types, the variability in trip distances, and the impact of factors like negotiated fares on overall trip metrics. Understanding these patterns can help optimize service operations, route planning, and resource allocation.

#### Extra per Trip Type
"Miscellaneous extras and surcharges" refers to additional fees that may be applied on top of the base fare in a transaction or payment. In this context, it specifically mentions the $0.50 and $1 charges for rush hour and overnight services.

![terminal](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/extra_per_trip_type.png)

![extra](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/bq_extra_per_trip_type.png)

Here are some insights from the data above:

1. Monthly additional charges imposed by VeriFone Inc and Creative Mobile Technologies, LLC vary from month to month, with the highest peak occurring in July 2021.
2. The majority of trips do not use the Store and Forward Trip method, except in July 2021, where Creative Mobile Technologies, LLC has some trips using this method.
3. The trend of additional charges seems to increase from year to year, especially in July 2021.
4. Additional charges imposed by VeriFone Inc tend to be more varied compared to Creative Mobile Technologies, LLC.
5. In August 2021, the amount of additional charges imposed by VeriFone Inc is lower compared to previous months.

#### Congestion Surcharge per Rate Code
![terminal](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/congestion_surcharge_per_rate_code.png)

![surcharge](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/models/bq_congestion_surcharge_per_rate_code.png)

Here are some insights derived from the table:

1. The "Standard Rate" has the highest congestion surcharge compared to other rate codes, indicating that trips under this rate are subject to significant congestion fees.
2. Trips to or from JFK airport have a moderate congestion surcharge, suggesting that travel to and from airports may incur additional charges due to congestion.
3. The "Negotiated Fare" rate code has a notably high congestion surcharge, indicating that negotiated fares may include surcharges for congestion.
4. The absence of a congestion surcharge for trips to Newark suggests that this area may not experience significant congestion-related costs.
5. The "Nassau or Westchester" rate code has a lower congestion surcharge compared to the "Standard Rate," indicating that trips to these areas may involve less congestion or shorter distances.

These insights provide valuable information about the distribution of congestion surcharges across different rate codes, helping to understand the cost implications of taxi trips in various locations and under different fare structures.

### dbt Test
#### dbt Test Raw Data
First, create a [source]() and [model]() file for raw data. dbt testing is performed on raw data with the goal of validating that the values in each column meet the expected criteria, ensuring that the models built from this raw data will be accurate. Testing includes checks for Null values, Data Types, and accepted value ranges.

You can view the code for dbt testing on raw data at this [link]()

#### dbt Test Monthly Total Passengers
![passengers](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/dbt-test/dbt_test_monthly_total_passengers.png)

Based on the dbt test model above, the following insights are obtained:
1. **"year" Column**: There are three types of tests performed on the "year" column:
    - not_null Test: Ensures that every value in the "year" column must not be null.
    - accepted_range Test: Ensures that every value in the "year" column must be within the minimum range of 0 (cannot be negative).
    - expect_column_values_to_be_of_type Test: Ensures that all values in the "year" column have the data type int64.
2. **"month_name" Column**: There are three types of tests performed on the "month_name" column:
    - not_null Test: Ensures that every value in the "month_name" column must not be null.
    - accepted_values Test: Ensures that every value in the "month_name" column can only be a month from January to December.
    - expect_column_values_to_be_of_type Test: Ensures that all values in the "month_name" column have the data type string.
3. **"monthly_passengers" Column**: There are three types of tests performed on the "monthly_passengers" column:
    - not_null Test: Ensures that every value in the "monthly_passengers" column must not be null.
    - accepted_range Test: Ensures that every value in the "monthly_passengers" column must be within the minimum range of 0 (cannot be negative).
    - expect_column_values_to_be_of_type Test: Ensures that all values in the "monthly_passengers" column have the data type int64.

Thus, the purpose of the dbt test model is to validate the consistency and accuracy of the data to be used in the analysis based on the "year", "month_name", and "monthly_passengers" columns.

#### dbt Test Monthly Transactions per Payment Type
![transactions](https://github.com/aisyahputami/Taxi-trip-data-NYC/blob/main/dbt-test/dbt_test_monthly_transactions.png)

Based on the dbt test model transactions_per_payment_type above, the following insights can be derived:

1. Column "year":
   There are three types of tests conducted on the "year" column:
    - not_null test: Ensures that every value in the "year" column is not null.
    - dbt_utils.accepted_range test: Ensures that every value in the "year" column is within the minimum accepted range of 0 (not negative).
    - dbt_expectations.expect_column_values_to_be_of_type test: Ensures that all values in the "year" column have a data type of int64.
2. Column "month_name":
   There are three types of tests conducted on the "month_name" column:
    - not_null test: Ensures that every value in the "month_name" column is not null.
    - accepted_values test: Ensures that every value in the "month_name" column is one of the months from January to December.
    - dbt_expectations.expect_column_values_to_be_of_type test: Ensures that all values in the "month_name" column have a data type of string.
3. Column "monthly_transactions":
    There are three types of tests conducted on the "monthly_transactions" column:
    - not_null test: Ensures that every value in the "monthly_transactions" column is not null.
    - dbt_utils.accepted_range test: Ensures that every value in the "monthly_transactions" column is within the minimum accepted range of 0 (not negative).
    - dbt_expectations.expect_column_values_to_be_of_type test: Ensures that all values in the "monthly_transactions" column have a data type of float64.
4. Column "payment_type":
    There are three types of tests conducted on the "payment_type" column:
    - not_null test: Ensures that every value in the "payment_type" column is not null.
    - accepted_values test: Ensures that every value in the "payment_type" column is one of the specified payment types: Credit Card, Cash, No Charge, Dispute, Unknown, Voided trip.
    - dbt_expectations.expect_column_values_to_be_of_type test: Ensures that all values in the "payment_type" column have a data type of string.

These tests aim to validate the consistency and accuracy of data that will be used in the analysis based on the "year", "month_name", "monthly_transactions", and "payment_type" columns.









