# 🚌 Real-Time Transit Data Pipeline

![Python](https://img.shields.io/badge/Python-3.9-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-Web_Service-000000?style=for-the-badge&logo=flask&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![MBTA API](https://img.shields.io/badge/API-Integration-green?style=for-the-badge)

## 📖 Overview
This project builds a real-time data ingestion pipeline for the **MBTA (Massachusetts Bay Transportation Authority)** transit system. It continuously polls the MBTA v3 API for live vehicle positions, stores the telemetry data in a relational database (**MySQL**), and exposes this data via a **Flask** REST endpoint for downstream applications or visualization.

This architecture simulates a production-grade IoT data collection service, capable of handling streaming geospatial data.

## 🏗️ Architecture

```mermaid
graph LR
    A[MBTA Public API] -->|JSON Stream| B(MBTAApiClient.py)
    B -->|Parse & Structure| C{Data Ingestion Layer}
    C -->|Persist| D[(MySQL Database)]
    C -->|Cache Object| E[Flask Server]
    E -->|Serve JSON| F[Web Client / Dashboard]
````

## 🚀 Key Features

  * **Live API Integration:** Connects to the MBTA v3 API to fetch real-time status of buses on Route 1.
  * **Persistent Storage:** Custom DAO (`mysqldb.py`) that maps JSON responses to a structured MySQL schema (`mbta_buses`).
  * **Microservice Architecture:** A lightweight Flask server (`server.py`) acts as a middleware, caching the latest positions and serving them via HTTP.
  * **Automated Polling:** Background threads configured to refresh data at 10-second intervals to respect API rate limits while ensuring freshness.

## 🛠️ Tech Stack

  * **Language:** Python 3.x
  * **Web Framework:** Flask
  * **Database:** MySQL
  * **Libraries:** `urllib`, `mysql-connector-python`, `threading`
  * **Data Source:** [MBTA V3 API](https://api-v3.mbta.com/)

## 📂 Project Structure

```text
├── server.py           # Main entry point: Flask app + Background Polling Loop
├── MBTAApiClient.py    # Logic to fetch and parse data from external API
├── mysqldb.py          # Data Access Object (DAO) for MySQL interactions
├── client.py           # Test client to verify the web service
├── timer.py            # Utility for scheduling tasks
└── README.md           # Documentation
```

## 💻 Getting Started

### 1\. Prerequisites

  * **Python 3.x** installed.
  * **MySQL Server** running locally (Port 3306).
  * Database credentials configured in `mysqldb.py` (Default: `root` / `MyNewPass`).

### 2\. Database Setup

Before running the application, ensure your MySQL database is initialized:

```sql
CREATE DATABASE MBTAdb;
USE MBTAdb;

CREATE TABLE mbta_buses (
    id VARCHAR(50),
    latitude DECIMAL(10, 6),
    longitude DECIMAL(10, 6),
    occupancy_status VARCHAR(50),
    current_stop_sequence INT,
    direction_id INT,
    current_status VARCHAR(50),
    speed DECIMAL(10, 2),
    updated_at VARCHAR(50)
);
```

### 3\. Running the Service

Start the Flask server. This triggers the background loop that fetches data from MBTA and inserts it into MySQL.

```bash
python server.py
```

*The server will start on `http://localhost:3000`.*

### 4\. Verification

You can check the data in two ways:

1.  **Browser:** Visit `http://localhost:3000/location` to see the JSON feed.
2.  **Client Script:** Run the included test client:
    ```bash
    python client.py
    ```

## 👤 Author

**José Antonio Morfín Guerrero**

  * Digital Transformation Leader | Data Engineer
  * [LinkedIn](https://www.google.com/search?q=https://linkedin.com/in/joseantoniomorfinguerrero)
  * [Portfolio](https://joseantoniomorfin.name)

<!-- end list -->

```
```
