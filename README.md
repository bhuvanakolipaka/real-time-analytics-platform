
# Real-Time Analytics Platform with Kafka, Flink, and BigQuery


This project is a complete, end-to-end demonstration of a modern streaming data pipeline. It captures live Wikipedia edits, processes them in real-time using Apache Flink, and stores the aggregated results in Google BigQuery for analysis and visualization.

## Architecture

The platform follows a classic, scalable streaming architecture that decouples ingestion, processing, and storage.


*(This is where you would insert a screenshot of your architecture diagram. It should show the flow from the Python Producer -> Kafka Topic -> Flink Job -> BigQuery Table -> Looker Studio Dashboard.)*

**Data Flow:**
1.  **Ingestion:** A Python producer script connects to the Wikipedia Server-Sent Events (SSE) stream, captures every new edit, and publishes it as a JSON message to an Apache Kafka topic.
2.  **Processing:** An Apache Flink job, written in Python, subscribes to the Kafka topic. It performs real-time transformations, including data cleaning, schema enforcement, and stateful aggregations (e.g., counting edits by country over a 1-minute tumbling window).
3.  **Storage:** The Flink job sinks the processed, aggregated data directly into a Google BigQuery table using the JDBC connector.
4.  **Visualization:** A dashboard in Google Looker Studio is connected to the BigQuery table, providing a live view of global Wikipedia edit activity.

## Tech Stack

* **Data Ingestion:** Python, `sseclient-py`
* **Messaging System:** Apache Kafka
* **Stream Processing:** Apache Flink (with Python Table API)
* **Data Warehouse:** Google BigQuery
* **BI / Visualization:** Google Looker Studio
* **Containerization:** Docker & Docker Compose

## Key Features

-   **Low-Latency Processing:** Designed to provide insights from raw data to a dashboard in under a minute.
-   **Scalable Components:** Each component (Kafka, Flink) is independently scalable to handle increased data volume.
-   **Stateful Stream Processing:** Leverages Flink's powerful state management to perform complex time-windowed aggregations.
-   **Cloud-Native Warehousing:** Utilizes Google BigQuery for a serverless, highly scalable analytics backend.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

-   [Docker](https://www.docker.com/get-started) and Docker Compose
-   [Google Cloud SDK](https://cloud.google.com/sdk/docs/install) authenticated with your account
-   A Google Cloud Project with the BigQuery API enabled
-   Python 3.8+

### Installation & Setup

1.  **Clone the Repository**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/real-time-analytics-platform.git](https://github.com/YOUR_USERNAME/real-time-analytics-platform.git)
    cd real-time-analytics-platform
    ```

2.  **Configure Google Cloud**
    -   Create a BigQuery dataset (e.g., `wikipedia_analytics`).
    -   Create the target table in your dataset. The required schema is defined in `gcp/schema.json`.
    -   Create a GCP Service Account with BigQuery Data Editor permissions and download the JSON key file.

3.  **Set Up Environment Variables**
    -   Rename the `.env.example` file to `.env`.
    -   Update the `.env` file with your GCP Project ID, dataset ID, table ID, and the path to your service account key file.

4.  **Launch the Pipeline**
    -   Start all the services (Kafka, Flink, and the Python producer) using Docker Compose.
    ```bash
    docker-compose up --build -d
    ```

5.  **Monitor the Pipeline**
    -   Check the Flink UI at `http://localhost:8081` to monitor the running job.
    -   Query your BigQuery table to see the processed data arriving.
    -   Connect Looker Studio to your BigQuery table to build your dashboard.

## To-Do / Future Improvements

-   [ ] Implement anomaly detection on the edit stream (e.g., flagging unusually large edits).
-   [ ] Add more complex data enrichment, such as joining user data.
-   [ ] Deploy the Flink cluster in a standalone configuration for better scalability.

## License

This project is licensed under the MIT License - see the `LICENSE.md` file for details.
