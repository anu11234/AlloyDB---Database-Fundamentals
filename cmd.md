# AlloyDB - Database Fundamentals || **GSP1083**

**Command:**

```bash
export ALLOYDB="<REPLACE_WITH_ALLOYDB_PRIVATE_IP>"
echo $ALLOYDB > alloydbip.txt

# Create table and insert initial data
PGPASSWORD=Change3Me psql -h $ALLOYDB -U postgres -c "
CREATE TABLE regions (
    region_id bigint NOT NULL,
    region_name varchar(25)
);
ALTER TABLE regions ADD PRIMARY KEY (region_id);

INSERT INTO regions VALUES (1, 'Europe');
INSERT INTO regions VALUES (2, 'Americas');
INSERT INTO regions VALUES (3, 'Asia');
INSERT INTO regions VALUES (4, 'Middle East and Africa');
"

# Download and execute the external SQL script
gcloud storage cp gs://spls/gsp1083/hrm_load.sql hrm_load.sql
PGPASSWORD=Change3Me psql -h $ALLOYDB -U postgres -f hrm_load.sql

echo "Task 2 completed successfully!"
```

```bash
# Set required variables
export PROJECT_ID=$(gcloud config get-value project)
export REGION="<REPLACE_WITH_YOUR_LAB_REGION>"

# Task 3: Create cluster and instance via CLI
gcloud alloydb clusters create gcloud-lab-cluster \
    --password=Change3Me \
    --network=peering-network \
    --region=$REGION \
    --project=$PROJECT_ID

gcloud alloydb instances create gcloud-lab-instance \
    --instance-type=PRIMARY \
    --cpu-count=2 \
    --region=$REGION \
    --cluster=gcloud-lab-cluster \
    --project=$PROJECT_ID

gcloud alloydb clusters list

# Task 4: Delete the cluster via CLI
gcloud alloydb clusters delete gcloud-lab-cluster \
    --force \
    --region=$REGION \
    --project=$PROJECT_ID \
    --quiet

echo "Tasks 3 and 4 completed successfully!"
```

