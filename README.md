Here’s the updated README without placeholders:

---

# **Trendify: Exploring and Visualizing Trends with Google Trends**

## **Overview**

Trendify is a web-based application designed to analyze and visualize trending topics using data sourced from Google Trends. By leveraging the power of BigQuery, Trendify provides users with insights into trends across locations, timeframes, and categories. Whether you're a marketer, researcher, journalist, or student, Trendify turns data into actionable insights.

---

## **Features**

- **Explore Trends by Location and Time**  
  Search for trending topics in specific locations (e.g., cities, states) during specified timeframes.

- **Keyword-Specific Trend Analysis**  
  Track the popularity of specific keywords over time with visualized insights.

- **Custom Alerts**  
  Set up notifications for real-time updates on trending topics.

- **Visualization Tools**  
  Access data presented through graphs, heatmaps, and tables for better understanding.

---

## **Technologies Used**

- **Frontend:** React.js  
- **Backend:** Python, Django  
- **Database:** BigQuery (primary), PostgreSQL (secondary)  
- **Tools:** TensorFlow (for predictive analytics)  
- **Deployment:** AWS  
- **Visualization:** D3.js (or any visualization library)

---

## **Database Design**

### **ER Diagram**
The application uses a relational database schema with the following key components:
- **User Table:** Stores user details such as ID, email, and preferences.
- **Trends Table:** Contains Google Trends data categorized by location, time, and topics.

Primary Keys:
- User ID, Location ID (User Table)
- Trend ID (Trends Table)

Foreign Key:
- Location ID linking User Table and Trends Table (1-to-many relationship).

---

## **SQL Queries**

### **Location-Based Trends Query**

#### Unoptimized:
```sql
SELECT term, score, week, dma_name
FROM top_rising_terms
WHERE dma_name LIKE '%Nashville%'
AND week BETWEEN '2024-01-01' AND '2024-01-31'
ORDER BY LENGTH(term), score DESC, term ASC;
```

#### Optimized:
```sql
SELECT term, score, week, dma_name
FROM top_rising_terms
WHERE dma_name = 'Nashville TN'
AND week BETWEEN '2024-01-01' AND '2024-01-31'
ORDER BY score DESC, term ASC;
```

### **Keyword-Specific Trend Analysis Query**

#### Unoptimized:
```sql
SELECT week AS date, 
       (SELECT SUM(score)
        FROM top_rising_terms AS subquery
        WHERE subquery.term = 'football'
        AND subquery.week = main.week) AS total_popularity,
       MIN(rank) AS best_rank
FROM top_rising_terms AS main
WHERE term = 'football'
AND week BETWEEN '2024-01-01' AND '2024-01-31'
GROUP BY week
ORDER BY week ASC;
```

#### Optimized:
```sql
SELECT week AS date, 
       SUM(score) AS total_popularity,
       MIN(rank) AS best_rank
FROM top_rising_terms
WHERE term = 'football'
AND week BETWEEN '2024-01-01' AND '2024-01-31'
GROUP BY week
ORDER BY week ASC;
```

---

## **Optimizations**

- **Indexing:**  
  Added indexes on location, term, and date columns to speed up filtering and sorting operations.
  
- **Partitioning:**  
  Partitioned datasets by date to reduce scanned data for time-specific queries.

- **Clustering:**  
  Clustered data by keywords to group similar values and enhance query performance.



## **Usage**

- Open your browser and navigate to `http://localhost:3000`.  
- Use the search interface to explore trending topics by location, keyword, or time.  
- Configure custom alerts and save trends for later analysis.

---

## **Contributors**

- **Ruyi Sule**  
  - Query development and optimization.  
  - Documentation and analysis.

- **Jason Hernandez**  
  - Query implementation and project features.  
  - Documentation on database design.

- **Anik Budhathoki**  
  - Database design and schema creation.  
  - Optimization and runtime analysis.

---

## **Future Improvements**

- **Predictive Analytics:**  
  Integrate machine learning models to forecast future trends.

- **User-Friendly Interface:**  
  Develop a no-code solution for BigQuery interactions.

- **Advanced Visualizations:**  
  Add dynamic charts and heatmaps to enhance data interpretation.

---
