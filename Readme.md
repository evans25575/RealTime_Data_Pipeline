# Real-Time Data Pipeline

## Project Overview
This project demonstrates the creation of a real-time data pipeline that ingests data from various sources, processes it using Apache Kafka and Apache Spark, and stores the processed results in a MySQL database. The project showcases skills in data engineering, including data ingestion, transformation, scheduling, and real-time analytics.

## Tools Used
- **Programming Languages**: Python
- **Data Engineering Tools**: Apache Kafka, Apache Spark, Apache Airflow
- **Database**: MySQL
- **Cloud Services**: AWS (optional for deployment)
- **Data Visualization**: Tableau (optional for visualizing processed data)

## Key Features
1. **Data Ingestion**: Kafka producer simulates data streaming from various sources to the Kafka topic.
2. **Real-Time Processing**: Apache Spark consumes data from Kafka and performs transformations.
3. **Data Storage**: Processed data is stored in a MySQL database for easy querying.
4. **Task Scheduling**: Apache Airflow schedules and automates the Kafka producer and consumer tasks.
5. **Data Analytics**: The pipeline can be extended for real-time analytics using Spark.

## Project Structure
- `kafka_producer.py`: Simulates a Kafka producer that sends data to the Kafka topic.
- `kafka_consumer_spark.py`: A Kafka consumer that processes data using Apache Spark and stores the results in MySQL.
- `airflow_dag.py`: Airflow DAG to schedule the Kafka producer and consumer tasks.
- `create_mysql_db.sql`: SQL commands to set up the MySQL database and table.
- `requirements.txt`: Python dependencies for the project.

## How to Run
1. Clone this repository:
   ```bash
   git clone https://github.com/evans25575/RealTime_Data_Pipeline.git
from kafka import KafkaProducer
import json
import time
import random

# Initialize Kafka producer
producer = KafkaProducer(bootstrap_servers=['localhost:9092'], value_serializer=lambda v: json.dumps(v).encode('utf-8'))

# Simulated data stream
def generate_data():
    while True:
        data = {
            "timestamp": time.time(),
            "value": random.randint(1, 100)
        }
        producer.send('real_time_topic', value=data)  # Send to Kafka topic
        print(f"Sent data: {data}")
        time.sleep(2)

# Start data generation
generate_data()

from pyspark.sql import SparkSession
from pyspark.sql.functions import *
from pyspark.sql.types import StringType
from pyspark.sql import Row
from kafka import KafkaConsumer
import json
import mysql.connector

# Initialize Spark session
spark = SparkSession.builder \
    .appName("RealTime Data Pipeline") \
    .getOrCreate()

# Set up Kafka consumer to read data from the Kafka topic
consumer = KafkaConsumer('real_time_topic', bootstrap_servers='localhost:9092', group_id='data-processing-group')

# Process each message from Kafka
for message in consumer:
    record = json.loads(message.value.decode('utf-8'))
    timestamp = record['timestamp']
    value = record['value']

    # Convert to Spark DataFrame
    row = Row(timestamp=timestamp, value=value)
    df = spark.createDataFrame([row])

    # Process data (e.g., calculating average)
    df.show()

    # Write processed data to MySQL database
    conn = mysql.connector.connect(
        host="localhost",
        user="root",
        password="password",
        database="real_time_db"
    )
    cursor = conn.cursor()
    cursor.execute("INSERT INTO data_stream (timestamp, value) VALUES (%s, %s)", (timestamp, value))
    conn.commit()
    conn.close()

CREATE DATABASE real_time_db;

USE real_time_db;

CREATE TABLE data_stream (
    id INT AUTO_INCREMENT PRIMARY KEY,
    timestamp DOUBLE,
    value INT
);

from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from datetime import datetime

def run_kafka_producer():
    # Run Kafka producer (this is a simplified version)
    import subprocess
    subprocess.call(['python', 'kafka_producer.py'])

def run_kafka_consumer():
    # Run Kafka consumer with Spark (this is a simplified version)
    import subprocess
    subprocess.call(['python', 'kafka_consumer_spark.py'])

# Define the DAG
dag = DAG(
    'real_time_data_pipeline',
    description='A simple real-time data pipeline',
    schedule_interval='*/5 * * * *',  # Run every 5 minutes
    start_date=datetime(2023, 1, 1),
    catchup=False,
)

# Define tasks
task1 = PythonOperator(
    task_id='run_kafka_producer',
    python_callable=run_kafka_producer,
    dag=dag,
)

task2 = PythonOperator(
    task_id='run_kafka_consumer',
    python_callable=run_kafka_consumer,
    dag=dag,
)

# Task dependencies
task1 >> task2

kafka-python
pyspark
mysql-connector-python
apache-airflow
