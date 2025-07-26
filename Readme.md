

# ⚙️ Real-Time Data Pipeline

A real-time data engineering pipeline that ingests simulated stock data using Apache Kafka, processes it using Apache Spark, stores the results in MySQL, and schedules tasks using Apache Airflow.

---

## 📌 Objectives

- Simulate real-time data streaming from a Python producer.
- Ingest data via Kafka and process it in real time using Spark Structured Streaming.
- Store transformed data in MySQL for querying and reporting.
- Automate the end-to-end process using Apache Airflow.

---

## 🛠 Tech Stack

| Component         | Tool/Framework               |
|------------------|------------------------------|
| Data Source       | Python Generator             |
| Messaging Queue   | Apache Kafka                 |
| Stream Processing | Apache Spark (Structured)    |
| Storage           | MySQL                        |
| Orchestration     | Apache Airflow               |
| Language          | Python                       |

---

## 📂 Project Structure

```bash
.
├── kafka_producer.py          # Simulates streaming data into Kafka
├── kafka_consumer_spark.py    # Consumes Kafka messages and processes in Spark
├── create_mysql_db.sql        # MySQL schema setup
├── airflow_dag.py             # Airflow DAG to schedule tasks
├── requirements.txt           # Dependencies
└── README.md                  # Project documentation


---

🚀 How to Run the Project

1. Setup MySQL

CREATE DATABASE real_time_db;
USE real_time_db;
CREATE TABLE data_stream (
  id INT AUTO_INCREMENT PRIMARY KEY,
  timestamp DOUBLE,
  value INT
);

2. Start Kafka Broker

Ensure Kafka is running on localhost:9092.

3. Run Kafka Producer

python kafka_producer.py

4. Run Spark Consumer

python kafka_consumer_spark.py

5. Schedule with Airflow

Use airflow_dag.py to run the producer and consumer every 5 minutes.


---

🧠 Sample Output

Sent data: {'timestamp': 1722001944.174284, 'value': 45}
+------------------+-------+
|     timestamp    | value |
+------------------+-------+
| 1722001944.174284|   45  |
+------------------+-------+




👨‍💻 Author

Evans Kiplangat

📜 License

MIT Licens
