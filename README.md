# Reddit-Data-Engineering-Pipeline
In this project, we will use airflow to pull Reddit data and push the data to S3 bucket.
We will connect S3 to AWS glue to perform transformations on data and push the transformed data back to S3.
We will use AWS Crawer to schedule the AWS Glue Data Catalog 
We will use AWS Athena to query the data.

# Techstack Used ::
1. Docker - To create dependencies for python & airflow to run
2. Python==3.9
3. Airflow - to schedule the job
4. AWS S3 - to store the input and output process data
5. AWS Athena - to view the data
6. AWS Glue - To transform the data
7. AWS Crawler - To populate AWS Glue Data Catalog with tables.

# Prerequisites
1. AWS Account with appropriate permissions for S3, Glue, Athena, and Redshift.
2. Reddit API credentials.
3. Docker Installation

# Project Architecture
![alt text](image.png)

## Steps followed ::
    1. Files and directory creation ::
        mkdir config dags data etls logs pipelines tests utils
        touch airflow.env docker-compose.yml Dockerfile
    2. After creating docker-compose.yml & Dockerfile run the follwoing command to activate the master and worker nodes ::
        docker compose up -d --build
    3. Create operators in dags/reddit_dag.py for airflow :: PythonOperator
    4. Create Credentials for Reddit API.
    5. Create a reddit_pipeline.py file for ETL steps
        #Following methods are created in etls/reddit_etl.py file
      - connect_reddit - to connect to reddit instance
      - extract_post - to extract data from reddit
      - transform_data - to transform data
    6. Creat AWS aws_access_key_id & aws_secret_access_key
    7. Create AWS compononents 
        - DAG for s3 upload (creatd in reddit_dag.py)
        #Following methods are created in etls/aws_etl.py file
        - s3 connection
        - create s3 bucket
        - upload to s3
    8. On AWS console ,using create visual ETL job in AWS glue create a job ::
        - data source - S3 bucket
        - data target - S3 bucket
    9. Infering schema from the required bucket.
    10. Select format & compression type for data target & select the target s3 bucket
    11. Update the script name in Job details tab of the Glue Job
    12. Make the changes in the script to transform the raw data in the S3 bucket(data source) as per your choice.
    13. Create a crawler to store the data in data target(s3 bucket) to a table in a database. This table is created in Athena. Crawler determines the schema of the data, this schema can be used in various datawarehousing solutions like AWS redshift
    14. The output of any query in athena can be stored in the S3 bucket.
    15. The data stored in Athena can also be viewed in AWS Redshift.We can also directly import data into AWS Redshift from S3 bucket without having to create a crawler.
