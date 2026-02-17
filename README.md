This project is an ETL (Extract, Transform, Load) data pipeline built using Apache Airflow. It fetches real-time weather data from the Open-Meteo API, transforms it, and loads it into a PostgreSQL database for further analysis.

Astronomer manages the entire Airflow so I installed Astro, Docker desktop and visual studio code.

ETL Pipeline Breakdown

1️⃣ Extract Data from Open-Meteo API
===============
HttpHook is used to connect to Open-Meteo API.  
API Request is built using latitude & longitude.  
Response is checked: ✅ If status code = 200, return JSON response.

2️⃣ Transform Data
===============
Extracts useful weather parameters from the API response.  
Converts raw JSON into a structured Python dictionary for easy database insertion.  
❌ Else, raise an error  

3️⃣ Load Data into PostgreSQL
===============
Uses PostgresHook to connect to the PostgreSQL database.  
Creates a table (weather_data) if it doesn’t exist.  
Inserts transformed data into the table.  
Commits the transaction to save changes.  


Step 1: Import Necessary Libraries
===========
Apache Airflow: For task orchestration,  
HttpHook: To fetch data from the Open-Meteo API,  
PostgresHook: To connect and interact with PostgreSQL,  
requests: To make HTTP requests,  
json: To parse JSON responses,  
datetime: To manage timestamps.  

 Step 2: Define Global Variables
 ===============
Latitude & Longitude: Used to fetch weather data for a specific location (London in this case).  
postgres_default: Used to connect to PostgreSQL.  
open_meteo_api: Used to connect to Open-Meteo API.  

Step 3: Define Default Arguments for Airflow DAG
===============
owner: Specifies the DAG owner.  
start_date: Defines when the DAG should start running.  

Step 4: Define the DAG
===============
dag_id: Unique identifier for the DAG.  
schedule_interval='@daily': Runs once per day.  
catchup=False: Ensures only the latest run is executed (prevents old unprocessed data from being executed in bulk).

Step 5: Deployment
===============
Starting Airflow by running 'astro dev start'    
Postgres: Airflow's Metadata Database  
Webserver: The Airflow component responsible for rendering the Airflow UI  
Scheduler: The Airflow component responsible for monitoring and triggering tasks  
Triggerer: The Airflow component responsible for triggering deferred tasks  

Step 6: Testing
===============
Running the Dags in the Airflow-UI  
Loading the data in Postgres(DBeaver)  
