# data-engineering-bootcamp-homework
## Module 1 Homework: Docker-Terraform

### Question 1. Understanding Docker First Run
Command used:
```bash
"docker run -it --entrypoint bash python:3.12.8"

#Output of pip --version
pip --version
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)

### Question 2. Understanding Docker networking and docker-compose
Commands used:
"docker pull postgres"

"docker run --name postgres-container -e POSTGRES_USER=your_user -e POSTGRES_PASSWORD=your_password -e POSTGRES_DB=your_database -p 5432:5432 -d postgres"

"docker run --name my_postgres -e POSTGRES_USER=your_user -e POSTGRES_PASSWORD=your_password -e POSTGRES_DB=your_database -p 5432:5432 -d postgres"

"docker ps -a"

"docker start <container_name_or_id>"

"docker exec -it postgres-container psql -U your_user -d your_database"

To find the process using port 5432

"sudo lsof -i :5432"

"netstat -an | grep 5432"

*To kill the command running

"sudo kill -9 806"

To remove the already created postgresql

"docker rm postgres-container"

"docker stop postgres"

"docker rm postgres"

"docker run --name postgres-container -e POSTGRES_PASSWORD=mysecretpassword -d postgres"

—To create docker volume 

"docker volume create --driver local --name my_postgres_volume"

—— to create Postgres container 

"docker run -d \
  -v "/Users/tomilolaadeyinka/Data Engineering:/mnt/data" \
  -p 5432:5432 \
  --name my_postgres \
  postgres"

—to load psql from docker

"docker exec -it my_postgres psql -U postgres"

### Question 3. Trip Segmentation Count

SELECT COUNT(*) AS up_to_1_mile
FROM green_tripdata
WHERE trip_distance <= 1
  AND lpep_pickup_datetime >= '2019-10-01'
  AND lpep_pickup_datetime < '2019-11-01';

### Question 4. Longest trip for each day

SELECT 
    DATE(lpep_pickup_datetime) AS pickup_day,
    MAX(trip_distance) AS longest_trip_distance
FROM 
    green_tripdata
GROUP BY 
    DATE(lpep_pickup_datetime)
ORDER BY 
    longest_trip_distance DESC
LIMIT 1;
 
### Question 5. Three biggest pickup zones

SELECT 
    PULocationID AS pickup_location, 
    SUM(total_amount) AS total_amount
FROM 
    green_tripdata
WHERE 
    DATE(lpep_pickup_datetime) = '2019-10-18'
GROUP BY 
    PULocationID
HAVING 
    SUM(total_amount) > 13000
ORDER BY 
    total_amount DESC;

### Question 6. Largest tip

SELECT 
    z_drop.zone AS dropoff_zone,
    MAX(t.tip_amount) AS largest_tip
FROM 
    green_tripdata t
JOIN 
    taxi_zone_lookup z_pickup
    ON t.pulocationid = z_pickup.locationid
JOIN 
    taxi_zone_lookup z_drop
    ON t.dolocationid = z_drop.locationid
WHERE 
    z_pickup.zone = 'East Harlem North'
    AND t.lpep_pickup_datetime BETWEEN '2019-10-01' AND '2019-10-31 23:59:59'
GROUP BY 
    z_drop.zone
ORDER BY 
    largest_tip DESC
LIMIT 1;

### Question 7. Terraform Workflow

—Install google cloud
"curl https://sdk.cloud.google.com | bash"

"gcloud init"

"touch variables.tf"

"variable "project" {
  description = "Google Cloud Project ID"
  type        = string
}"

"touch main.tf"

"provider "google" {
  project = var.project
  region  = "us-central1"  # Change to your region
}"

"resource "google_storage_bucket" "my_bucket" {
  name     = "my-unique-bucket-name"  # Make sure this is a unique bucket name
  location = "US"
}"

"gcloud auth login"

"gcloud config set project seraphic-port-448222-i9"

"terraform init"

"terraform plan -var="project=seraphic-port-448222-i9"

"terraform destroy"


