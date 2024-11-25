# YouTube Data Pipeline with Airflow

This project implements an Airflow DAG to fetch YouTube channel and video data using the YouTube Data API, transform it, and load it into Google Cloud Storage (GCS) and BigQuery. The DAG also includes Terraform tasks for infrastructure setup and dbt transformations for marts and reports model.

## Architecture

![Pipeline Flow](/images/pipeline_architecture.png "YouTube Pipeline Flow")

## Features

- Fetch YouTube channel statistics and video details using the YouTube Data API.
- Save the data as a CSV file locally.
- Upload the CSV file to Google Cloud Storage (GCS).
- Load the data from GCS into BigQuery.
- Perform transformations using dbt to create marts and reports.

## Prerequisites

Ensure you have the following configured:

- **YouTube Data API Key**: Add the API key as an Airflow Variable (`YT_API_KEY`).
- **Channel IDs**: Add channel IDs as Airflow Variables (`MiawAug`, `WindahB`).
- **Google Cloud Platform**:
  - GCS bucket and BigQuery dataset/table set up.
  - GCP connection configured in Airflow (`gcp`).
- **Terraform**:
  - Terraform configurations for resource provisioning.
  - Mounted Terraform directory in the Airflow container.

## DAG Structure

### Tasks

1. **Fetch YouTube Data**:  
   Fetches channel statistics and video details, filters the data, and saves it to a local CSV file.
   
2. **Terraform Tasks**:
   - `terraform_init`: Initializes the Terraform working directory.
   - `terraform_apply`: Applies the Terraform configuration.

3. **Upload to GCS**:  
   Uploads the generated CSV file to a specified GCS bucket.

4. **Load to BigQuery**:  
   Loads the CSV data from GCS into a BigQuery table.

5. **dbt Transformations**:  
   - `transform_marts`: dbt transformations for marts.
   - `transform_report`: dbt transformations for reports.

### Dependencies

```plaintext
get_youtube_data -> terraform_tasks -> upload_to_gcs -> load_data_to_bq -> transform_marts -> transform_report
