
# Home Sales Analysis using PySpark

## Overview

This project is a comprehensive analysis of home sales data using PySpark SQL. The primary objective is to calculate key metrics such as the average home prices based on various parameters like the number of bedrooms, bathrooms, square footage, and view ratings. Additionally, performance optimization techniques like caching and partitioning are used to enhance the query efficiency.

## Tools and Technologies

- **PySpark SQL**: For performing SQL-like queries on large datasets.
- **AWS S3**: Data source for the home sales dataset.
- **Parquet Format**: Used for partitioning and optimizing large datasets.
- **Python**: The programming language used for implementing the analysis.

## Dataset

The dataset consists of home sales data which includes information such as:
- Number of bedrooms
- Number of bathrooms
- Square footage of the living area
- Year the home was built
- Sale price
- View rating (waterfront views, etc.)

## Key Steps

1. **Data Ingestion**:
   - The dataset is read from an AWS S3 bucket into a PySpark DataFrame.
   
2. **Data Queries**:
   - SQL queries are run on the DataFrame to compute the following metrics:
     - Average price of a four-bedroom house for each year.
     - Average price of a house with three bedrooms and three bathrooms for each year it was built.
     - Average price of a house with three bedrooms, three bathrooms, two floors, and a living area of at least 2,000 square feet.
     - Average price of homes per view rating, where the average price is greater than $350,000.
   
3. **Optimization Techniques**:
   - The temporary table is cached to speed up repeated queries.
   - The dataset is partitioned by the year the homes were built and stored in Parquet format to optimize query performance.

4. **Performance Comparison**:
   - Queries were run on both the cached and uncached datasets to compare runtimes and evaluate performance improvements.

## Query Examples

### Example 1: Average price of four-bedroom houses sold per year
```python
spark.sql("""
    SELECT YEAR(date) as year, ROUND(AVG(price), 2) as avg_price
    FROM home_sales
    WHERE bedrooms = 4
    GROUP BY YEAR(date)
    ORDER BY year
""").show(truncate=False)
```

### Example 2: Average price of homes per view rating, where average price is greater than $350,000
```python
spark.sql("""
    SELECT view, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    GROUP BY view
    HAVING AVG(price) >= 350000
    ORDER BY view DESC
""").show(truncate=False)
```

## Resources

- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [AWS S3 Documentation](https://aws.amazon.com/s3/)
- [Parquet Format](https://parquet.apache.org/)
- [Python Documentation](https://docs.python.org/3/)

## Conclusion

This project demonstrates the efficient use of PySpark for big data analysis, highlighting the importance of data partitioning and caching for optimizing performance in large-scale datasets. By leveraging these techniques, we were able to gain insights into home sales while reducing query runtime significantly.
