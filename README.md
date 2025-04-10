# Data Processing with Databricks Spark for New York City’s Taxi and Limousine Commission (TLC) 

# Introduction
The purpose of this project is to process and analyze a large dataset of taxi trip records from New York City’s Taxi and Limousine Commission (TLC) using Databricks and Spark, with data stored in Azure Blob Storage. The dataset consists of millions of records from both yellow and green taxis, covering the years 2015 to 2022. The yellow taxi dataset records trips from taxis operating citywide, while the green taxi dataset covers trips from taxis serving areas outside Manhattan. Both datasets include fields such as pickup and dropoff times, location IDs, trip distance, passenger count, and fare details, though some schema differences were resolved during processing.
<img alt="image" src="https://github.com/user-attachments/assets/cc173662-0298-4807-9203-eaa536cbecfd" />

The raw taxi trip data was stored in Azure Blob Storage for scalability, with Parquet chosen for efficient storage and processing. These files were ingested into Databricks, where they were processed in DBFS and analyzed. In addition, a location referential dataset was used to map pickup and dropoff location IDs to geographic information, such as boroughs, zones, and service zones.

The goals of this project is to clean, enrich, and combine the datasets, answer a set of business-related questions, and develop machine learning models to predict trip costs. Databricks’ cloud infrastructure, along with Azure Blob Storage, allowed us to handle the large datasets efficiently using Spark’s distributed computing capabilities. Key steps included data ingestion, transformation, joining datasets, and predictive modeling.

---
# Data Ingestion and Preparation
This section details the process of ingesting the taxi trip datasets from Azure Blob Storage into Databricks, preparing them for analysis, and applying initial data cleaning and transformations.

1. The comprehensive trip data for both yellow and green taxis from the New York City Taxi and Limousine Commission (TLC) and cover the period from 2015 to 2022. These datasets were categorized by year and kept in Azure Blob Storage to facilitate easy retrieval and analysis.

   - Data on yellow taxis: 2015–2022  
   - Data on green taxis: 2015–2022
   - <img width="468" alt="image" src="https://github.com/user-attachments/assets/36b984b6-8717-47ce-964f-4b8cf029c8cb" />


2. Azure Blob Storage datasets were read and copied into Databricks File System (DBFS) for processing. To optimize Spark-based analysis, Databricks stored the data locally.

3. To verify the data ingestion, total number of rows in the dataset were counted for both yellow and green taxis:
   - **Green taxi data:** 66,200,401 rows  
   - **Yellow taxi data:** 663,055,251 rows

4. The location referential dataset was downloaded and imported into Databricks to add geographic information to taxi trip data. This dataset maps pickup/dropoff IDs to boroughs, zones, and service zones.
   - <img width="326" alt="image" src="https://github.com/user-attachments/assets/adc4bf03-1dbf-4f81-91d1-39a16afa9586" />
   - <img width="325" alt="image" src="https://github.com/user-attachments/assets/6e7f9c28-4bf2-4680-a6cb-807f182f9e77" />



6. The Parquet format offers significant advantages over CSV, particularly for large datasets like the NYC Taxi data. Parquet's columnar storage allows for better compression and efficient querying, making it ideal for big data use cases. It supports schema evolution and efficient data retrieval, reducing storage costs and enhancing performance. In contrast, CSV files are larger and slower to process due to their lack of compression and less efficient data structure. Thus, Parquet is the preferred format for handling large-scale data efficiently.
   - <img width="468" alt="image" src="https://github.com/user-attachments/assets/95ceb5b5-d390-4284-a8bc-eb0b2abaebdd" />
   - <img width="468" alt="image" src="https://github.com/user-attachments/assets/8dceea14-0734-4eff-bcf3-5d2eea75c6d5" />




---
 
 
## Analysis
### Analysis of Monthly Trip Data
Each month, we looked at NYC taxi data to understand travel trends, passenger numbers, and fare patterns. First, we analyzed the total number of trips, pinpointing the busiest days and hours to spot patterns in when people ride the most. We also examined the average number of passengers per trip to get a sense of how often taxis were shared, while looking at the average fare per trip and per passenger to understand how costs vary with ride distance and group size. These insights help reveal what impacts taxi demand and fare structure—useful for planning, pricing, and adjusting to rider needs.
 - <img width="343" alt="image" src="https://github.com/user-attachments/assets/e5b51386-bab0-4c2c-b6dc-73893d2138db" />

 
### Analysis of taxi duration, distance and speed
Both yellow and green taxis were analyzed to understand key trip characteristics, looking at the average, median, minimum, and maximum values for trip duration, distance, and speed. This comparison gives a clearer picture of how each taxi type operates—for instance, whether yellow taxis generally cover longer distances or if green taxis tend to focus on quicker, shorter trips. These insights can help refine route planning, adjust fares, and improve service based on typical trip patterns.
- <img width="472" alt="image" src="https://github.com/user-attachments/assets/502a9353-7013-498c-9c1a-77dbb9f0ab86" />

 
### Analysis of trip, distance and amount
For each taxi color (yellow and green), we analyzed trips based on every combination of pickup and drop-off locations by borough, as well as by month, day of the week, and hour. For each of these combinations, we looked at the total number of trips, average trip distance, average amount paid per trip, and total amount paid.
 - <img width="468" alt="image" src="https://github.com/user-attachments/assets/7b004447-8ddf-4d18-8ef5-63f594ab6598" />

This breakdown gives a detailed view of travel patterns across different times and locations, showing when and where trips are most frequent, how far people tend to travel, and what they typically pay. These insights are invaluable for understanding peak demand times, preferred routes, and fare trends, which can help with better planning, pricing strategies, and adjusting services to meet rider demand.


### Analysis of tips received
The analysis found that 62% of trips included a tip. This substantial tipping rate suggests that a majority of passengers choose to tip their drivers, which could indicate positive rider experiences or satisfaction with the service. Understanding this behavior helps highlight potential factors that influence tipping, such as service quality, trip length, or location, providing valuable insights for improving customer service and driver incentives.
 - <img width="468" alt="image" src="https://github.com/user-attachments/assets/2326b998-a0bc-4e6d-9342-002e43766b35" />


### Analysis of tips received of at least 5$
For trips where drivers received tips, 11.8% included tips of $5 or more. This figure highlights that while many passengers do tip, only a smaller portion provide a more generous amount. Understanding this pattern can help in designing initiatives to encourage higher tipping, such as improved service quality or targeted incentives, benefiting both drivers and overall customer satisfaction.
 - <img width="468" alt="image" src="https://github.com/user-attachments/assets/d3ed82b5-e903-45b8-bc8f-feee6553ea4b" />

### Analysis of average speed and trip duration
Trips are grouped by duration to see how trip characteristics vary with time spent. The categories include Under 5 mins, 5 to 10 mins, 10 to 20 mins, 20 to 30 mins, 30 to 60 mins, and At least 60 mins. For each category, the average speed (in km per hour) and average distance per dollar is looked to understand how efficiently trips cover distance relative to time and cost. Here’s a breakdown:
- Short trips (Under 5 Mins) show a quick average speed of 12.2 km/h but the lowest distance per dollar efficiency at 0.1 km/$, likely due to minimum fare charges.
- Moderately short trips (5 to 10 mins) slightly improve in efficiency with an average speed of 10.56 km/h and 0.13 km/$, suggesting a balance between time and cost for short urban trips.
- Medium trips (10 to 20 mins) offer a good blend of time and cost efficiency with a speed of 10.97 km/h and a distance per dollar of 0.16 km/$, typical for urban travel.
- Longer trips (20 to 30 mins) see increased speeds of 13.07 km/h and better value at 0.19 km/$ as they likely move beyond congested city centers.
- Extended trips (30 to 60 mins) achieve the highest speeds at 15.7 km/h and an improved 0.23 km/$, indicating efficient travel likely on less congested routes.
- Long-haul trips (At least 60 mins), although slightly slower at 14.01 km/h, offer the best distance per dollar value at 0.36 km/$, showing that fares become more proportional to distance traveled on the longest routes.
<img width="468" alt="image" src="https://github.com/user-attachments/assets/fff41de5-cb3d-4752-85ea-0a4a403bcc7c" />


 

### Suggestion to taxi driver for maximized earning
Given the data on average speed and distance per dollar for various trip durations, the most optimal duration bin for a taxi driver to target to maximize income would be the "At least 60 mins" duration bin. This recommendation is based on several key factors derived from your analysis:
- Highest Distance per Dollar: The "At least 60 mins" bin offers the highest average distance per dollar at 0.36 km/$. This indicates that longer trips are more cost-efficient, providing greater earnings per unit of distance compared to shorter trips.
- Good Average Speed: Although not the highest, the average speed of 14.01 km/h in this duration bin is still reasonable. This speed suggests that these longer trips manage to avoid the slowest traffic conditions, potentially maximizing the fare collected per hour.
- Economic Efficiency: Longer trips often mean consistent earnings over a period, reducing the downtime and costs associated with frequent short trips, such as fuel costs and vehicle wear. They also allow drivers to make efficient use of their time, accumulating earnings steadily.
- Customer Satisfaction: Longer trips can also lead to higher customer satisfaction, as they are often planned and less susceptible to the variability of short urban rides. Satisfied customers are more likely to tip well, further increasing the driver's income.
  <img width="468" alt="image" src="https://github.com/user-attachments/assets/d0c506d1-256a-42ec-baa0-c7b733b2d72a" />

Therefore, targeting longer trips can be a strategic choice for drivers aiming to maximize their income. These trips provide a higher return on investment per trip and may also offer more predictable and stable earnings. Taxi drivers and companies should consider strategies to attract customers needing longer rides, such as airport transfers, inter-city trips, or scheduled long-distance services.
 
# Machine Learning
In this section, machine learning models have been developed to predict the **Total Amount (`total_amount`)** for each taxi trip using Spark ML pipelines.

 <img width="379" alt="image" src="https://github.com/user-attachments/assets/fae5b273-7211-4a42-924b-20ab84c9dd68" />

 
### Machine learning models
Two models were built:

- **Linear Regression**  
  - Assumes a linear relationship  
  - Simple, interpretable, and fast

- **Random Forest Regressor**  
  - Ensemble-based, handles non-linear relationships  
  - More complex and computationally intensive

Both were trained using Spark ML pipelines with feature indexing and vector assembly.

#### Model evaluation
The evaluation of both models was conducted using RMSE on the test data from October to December 2022 to assess prediction accuracy. The results were as follows:
| Model              | RMSE |
|--------------------|------|
| Baseline           | 78   |
| Linear Regression  | 3.8  |
| Random Forest      | 10.8 |

 <img width="468" alt="image" src="https://github.com/user-attachments/assets/49c7b5cb-fb7a-4920-b038-1c093bb523bd" />


In the comparison of these RMSE values, the model with the lowest error was identified as the best predictor. The Linear Regression model outperformed the Random Forest model in this case, demonstrating its effectiveness for the dataset.
 
#### Optimal Model Selection

Linear Regression was selected because:
- It achieved the **lowest RMSE**
- It is **simpler and faster** to train
- It provided **effective performance** for the task

#### Final Prediction and Evaluation

The analysis employed a **Linear Regression model** using key features such as `passenger_count`, `trip_distance`, `congestion_surcharge`, and `airport_fee` to predict the `total_amount` of taxi trips. The Linear Regression model achieved a **Root Mean Squared Error (RMSE)** of **3.82**, a significant improvement over the **baseline model’s RMSE of 78**. This indicates that the selected features have a strong influence on fare amounts and that the model can predict fares with high accuracy.

 <img alt="image" src="https://github.com/user-attachments/assets/21838242-6743-4436-b968-6ed85ed57934" />
 <img alt="image" src="https://github.com/user-attachments/assets/6dce2220-31e4-4e83-b0ce-a2607f1b7f80" />

To evaluate the model’s generalizability, predictions were made on taxi trips occurring from **October to December 2022**, and the RMSE for this period was calculated. The resulting RMSE remained significantly lower than the baseline, confirming the model’s effectiveness in forecasting fare amounts and demonstrating improved prediction accuracy on more recent, unseen data.

Due to **computational and time constraints**, only a **small portion of the available dataset** was used for model development. This sampling strategy allowed for faster processing and quicker iteration but may limit the model’s generalizability across the entire dataset. Future work should focus on **increasing the sample size** and utilizing **more advanced computational resources** to ensure scalability and robustness.

In conclusion, this approach offered valuable insights into predicting taxi fares using machine learning. The Linear Regression model outperformed the baseline, emphasizing the importance of including relevant trip-level features. By improving fare estimation accuracy, such models can contribute to better **operational efficiency** and improved **customer experience** in urban taxi services.

# Conclusion
This project successfully utilized Databricks and Spark to process and analyze a large-scale dataset from New York City’s Taxi and Limousine Commission (TLC). By leveraging the scalability of Azure Blob Storage and the processing power of Apache Spark, it was possible to ingest, transform, and analyze over 700 million taxi trip records efficiently.

Key insights were drawn from detailed analyses of trip patterns, passenger behavior, fare structures, and tipping trends. Business-oriented analyses provided actionable suggestions for taxi drivers and operators, such as focusing on longer trips for higher earnings.

Furthermore, machine learning models were developed to predict the total trip amount. Linear Regression was selected as the best-performing model due to its low RMSE and efficiency, offering a practical solution for real-time or large-scale fare prediction.

This end-to-end pipeline demonstrates the potential of cloud-based big data solutions to drive insight, support decision-making, and enable intelligent transportation analytics at scale.
