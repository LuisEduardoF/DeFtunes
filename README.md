# Deftunes - Data Engineering Capstone Project

<p align="center">
  <img src="logo.png" />
</p>

## Overview
DeFtunes is a music industry company offering a subscription-based app for streaming songs. Recently, they expanded their services to include digital song purchases. This project implements a comprehensive data pipeline to extract purchase data from their API and operational database, process and model this data, and deliver it for analysis and insights.

The project is designed as a complete data engineering solution that demonstrates modern data engineering practices including data extraction, transformation, loading, quality validation, orchestration, and visualization. It's built using a cloud-native approach on AWS, with infrastructure as code principles for reproducibility and maintainability.

## Project Architecture

![Architecture Diagram Part 2](Capstone%20Project%20Part%202/images/Capstone-diagram2.png)

The project implements a medallion architecture with three layers:
1. **Landing Zone**: Raw data extracted from sources
2. **Transform Zone**: Cleansed and processed data in Iceberg format
3. **Serving Zone**: Final modeled data in Redshift for analytics

## Data Sources
- **PostgreSQL RDS Database**: Contains song information in the `songs` table (based on the Million Song Dataset)
- **API Endpoints**:
  - `/sessions`: Contains transaction data for song purchases with session details
  - `/users`: Contains user profile information including location and demographics

## Technologies Used

### Infrastructure and Deployment
- **AWS Services**:
  - **S3**: Data Lake storage for all three data zones
  - **Glue**: ETL processing and job orchestration
  - **Redshift**: Data Warehouse for analytical queries
  - **Glue Data Catalog**: Metadata repository
  - **Redshift Spectrum**: Query data directly from S3 without loading
  - **RDS PostgreSQL**: Source operational database
  - **IAM**: Security and access control
- **Terraform**: Infrastructure as Code for reproducible deployments

### Data Processing and Modeling
- **Apache Iceberg**: Table format for the transform zone providing ACID transactions and schema evolution
- **Python**: ETL scripts and data processing
- **dbt (Data Build Tool)**: SQL-based data modeling and transformation in the serving zone
- **AWS Glue Data Quality**: Data quality validation and monitoring
- **Pandas**: Data exploration and analysis

### Orchestration and Visualization
- **Apache Airflow**: Workflow orchestration, scheduling, and monitoring
- **Apache Superset**: Interactive data visualization and dashboarding
- **SQL**: Data querying and analysis

## Project Implementation

### Part 1: ETL and Data Modeling
1. **Data Extraction**:
   - Extract song data from PostgreSQL RDS database using Glue jobs
   - Extract user and session data from API endpoints via HTTP requests
   - Implement date-based partitioning for efficient data organization
   - Store raw data in the landing zone of S3 in its original format

2. **Data Transformation**:
   - Process raw data with Glue PySpark jobs
   - Perform data type conversions and schema normalization
   - Add metadata including ingestion timestamps and source information
   - Convert to Apache Iceberg format in the transform zone for better data management
   - Implement data quality checks during transformation

3. **Data Modeling**:
   - Use Redshift Spectrum to access Iceberg tables without data movement
   - Create external schemas in Redshift pointing to Glue Data Catalog
   - Implement star schema modeling with dbt for analytical purposes
   - Design fact and dimension tables in Redshift serving zone
   - Optimize query performance through appropriate indexing and distribution keys

![Architecture Diagram Part 1](Capstone%20Project%20Part%201/images/Capstone-diagram.png)

### Part 2: Data Quality and Orchestration
1. **Data Quality Checks**:
   - Implement comprehensive quality rules using AWS Glue Data Quality
   - Validate data completeness, uniqueness, consistency, and referential integrity
   - Define threshold-based quality constraints for automated monitoring
   - Generate data quality metrics and anomaly detection
   - Integrate quality checks into the pipeline workflow

2. **Orchestration with Airflow**:
   - Create separate DAGs for song data pipeline and API data pipeline
   - Implement task dependencies with clear execution order
   - Configure incremental data loading with date-based parameters
   - Schedule automated daily pipeline runs
   - Implement robust error handling, retries, and alerting mechanisms
   - Monitor pipeline performance and execution metrics

3. **Analytics and Visualization**:
   - Create materialized views for common business questions in the BI layer
   - Design specialized views for sales analysis by artist and geography
   - Build interactive dashboards with Apache Superset
   - Implement drill-down capabilities for detailed analysis
   - Monitor sales performance by artist, country, time periods and other dimensions
   - Enable self-service analytics for business users

![Architecture Diagram Part 2](Capstone%20Project%20Part%202/images/Capstone-diagram2.png)

## Project Structure
- **Capstone Project Part 1**: ETL and Data Modeling
  - `terraform/`: Infrastructure as Code files
    - `modules/`: Modular components for different pipeline stages
      - `extract_job/`: Extraction job configurations
      - `transform_job/`: Transformation job configurations
      - `serving/`: Redshift and data serving configurations
    - `assets/`: Glue job scripts
      - `extract_jobs/`: Scripts for extracting data from sources
      - `transform_jobs/`: Scripts for transforming data to Iceberg format
  - `dbt_modeling/`: dbt models for data transformation
    - `models/`: SQL transformations
      - `serving_layer/`: Star schema models
    - `scripts/`: Configuration scripts

- **Capstone Project Part 2**: Orchestration and Visualization
  - `dags/`: Airflow DAG definitions
    - `deftunes_songs_pipeline.py`: Pipeline for song data processing
    - `deftunes_api_pipeline.py`: Pipeline for API data processing
  - `dbt_modeling/`: Enhanced dbt models
    - `models/`: SQL transformations
      - `serving_layer/`: Dimension and fact tables
      - `bi_views/`: Business intelligence views
        - `sales_per_artist_vw.sql`: Artist sales analysis
        - `sales_per_country_vw.sql`: Geographical sales analysis

## Key Features
- **Modern Data Architecture**
  - Medallion architecture (Bronze/Silver/Gold) for organized data flow
  - Separation of storage and compute with Redshift Spectrum
  - ACID-compliant data lake with Apache Iceberg
  
- **Robust Data Engineering**
  - Incremental data loading capabilities for efficiency
  - Automated data quality validation at multiple stages
  - Type-2 dimension handling for historical tracking
  - Optimized star schema design for analytical queries
  
- **DevOps Best Practices**
  - Infrastructure as Code with Terraform for reproducibility
  - Modular design for independent component development
  - Pipeline version control and configuration management
  - Environment isolation and credential management
  
- **Advanced Analytics Support**
  - Business intelligence views for common analytical patterns
  - Self-service analytics capabilities with Superset
  - End-to-end orchestration with Airflow
  - Comprehensive data lineage and documentation

## Learning Outcomes
This project demonstrates proficiency in:
- Designing and implementing medallion data architectures
- Working with cloud-native data services on AWS
- Infrastructure as Code with Terraform
- ETL development with AWS Glue and PySpark
- Data modeling with dbt and SQL
- Data quality validation and monitoring
- Workflow orchestration with Apache Airflow
- Data visualization with Apache Superset
- End-to-end data pipeline development and management
