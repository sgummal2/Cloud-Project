# Cloud-Project
Luxury Hotels &amp; Resorts company migrating applications and data to the cloud.

## Solution Architecture:

<img width="491" alt="Architecture1" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/7b5ed54e-9c0a-4cc4-b50c-77265c4f0be7">


## Steps to implement Hands-on Project: 
## Mission 1: Enabling a MultiCloud architecture deployment through Terraform, with resources running in AWS and Google ﻿Cloud Platform

### Amazon Web Services

-	 Access the AWS console and go to the IAM service
-	 Under Access management, Click on "Users", then "Add users". Insert the User name **terraform-en-1** and click in **Next** to create a programmatic user.

![Untitled](https://github.com/sgummal2/Cloud-Project/assets/140002588/37149595-52e3-4b70-91e7-8611363e42df)

-	On Set permissions, Permissions options, click on the "Attach policies directly" button.

<img width="1000" alt="image" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/be01e136-c14d-4924-80f8-b3331d4bd964">

-	 Type **AmazonS3FullAccess** in **Search**
-	 Select **AmazonS3FullAccess**

![Untitled (1)](https://github.com/sgummal2/Cloud-Project/assets/140002588/d61869b5-2672-4438-9a83-b11ec6cdedee)

-	 Click in **Next**
-	 Review all details, then click **Create user**

<img width="1112" alt="Screenshot 2023-02-03 at 09 32 38" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/0a953753-f554-4aa6-a5de-db927a9d8ee3">

**AWS has recently changed the way to download the key. Follow the new steps:**

-	 Click on the user you have created.
-	 After this, click on the **Security credentials** tab.
-	 Scroll down and go to the Access keys section.
-	 Click on **Create access key**

<img width="1130" alt="Screenshot 2023-02-03 at 09 48 38" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/c45191a4-a984-4a12-b565-467b9bad4c14">

-	 Select **Command Line Interface (CLI)** and **I understand the above recommendation and want to proceed to create an access key** checkbox.
-	 Click **Next**
-	 Click on **Create access key**
-	 Click on **Download .csv file**
-	 After the download is finished, click on **Done**

<details> <summary> <bold> [TIP] Access key best practices </bold> </summary>
  
    • Never store your access key in plain text, in a code repository, or in code.
    • Disable or delete the access key when no longer needed.
    • Enable least-privilege permissions.
    • Rotate access keys regularly. 
    
</details>

-	 After download, click **Done**
-	 Now, rename .csv file downloaded to **accessKeys.csv**

### Google Cloud Platform (GCP)

-	Download the mission1.zip hands-on files.
-	Access GCP Console and open Cloud Shell.
-	Upload **accessKeys.csv** and **mission1.zip** hands-on file to GCP Cloud Shell.
- Check if the upload has been successfully completed using the command **ls -la**
- Hands-on files preparation

~~~
mkdir mission1_en
mv mission1.zip mission1_en
cd mission1_en
unzip mission1.zip
mv ~/accessKeys.csv mission1/en
cd mission1/en
chmod +x *.sh
~~~

- Run the following commands to prepare the AWS and GCP environment. Authorize when asked.

~~~
./aws_set_credentials.sh accessKeys.csv
gcloud config set project <project_id>
~~~

- Execute the command below:

~~~
./gcp_set_project.sh
~~~

- Enable the Container Registry API, Kubernetes Engine API, and the Cloud SQL API.

~~~
gcloud services enable containerregistry.googleapis.com 
gcloud services enable container.googleapis.com 
gcloud services enable sqladmin.googleapis.com
~~~

**IMPORTANT (DO NOT SKIP):**

- **Before executing the Terraform commands, open the Google Editor and update the file tcb_aws_storage.tf replacing the bucket name with a unique name (AWS requires unique bucket names).**
    - Open the **tcb_aws_storage.tf** using Google Editor.
    - On **line 4** of the file **tcb_aws_storage.tf**:
        - Replace **xxxx** with your name initials, using **5 letters** plus **5 random numbers**:
        - Example: **luxxy-covid-testing-system-pdf-en-spran12345**
     
- Run the following commands to finish provision infrastructure steps

~~~
cd ~/mission1_en/mission1/en/terraform/

terraform init
terraform plan
terraform apply
	Type Yes and go ahead.
~~~

**Attention**: The Cloud SQL database may take **15 to 25 minutes** to create, always check the **CloudShell** and click **Reconnect** when the session expires (the session expires after **5 minutes** of inactivity by default)

- The **warning** message at the end of **terraform apply** command execution is not a problem, please go ahead:

![Untitled (2)](https://github.com/sgummal2/Cloud-Project/assets/140002588/93b1fe39-56a5-4860-9ea7-589f0ca71b9e)

💡 After finished, access the [click link](https://cloud.google.com/kubernetes-engine/docs/resources/autopilot-standard-feature-comparison) to Compare GKE Autopilot and Standard.

## SQL Network Configuration

- Once the Cloud SQL instance is provisioned, access the Cloud SQL service
- Click on your Cloud SQL instance.
- On the left side, under Primary Instance, click on **Connections**
- Go to **Networking** tab.
- Under **Instance IP assignment**, select Private IP to enable.
    - Under **Associated networking**, select "Default".
    - Click **Set up Connection**
    - Click on **Enable API**, to enable Service Networking API (if asked).
    - Select **Use an automatically allocated IP range in your network**
    - Click **Continue**
    - Click **Create Connection** and **wait a minutes until conclude.** You will see the message: “*Private services access connection for network default
     has been successfully created.”*
- Under **Authorized Networks**, click "Add Network".
- Under **New Network**, enter the following information:
    - **Name:** Public Access (For testing purposes only)
    - **Network**: 0.0.0.0/0
    - Click **Done**.
    - Click **Save** and wait to finish the update.
    This update may take from **10 to 20 minutes** to finish

PS: For production environments, it is recommended to use only the Private Network for database 
access.

⚠️  Never grant public network access (0.0.0.0/0) to production databases.

## Mission 2: Converting a database and an application to run on the MultiCloud architecture (AWS ﻿and ﻿Google Cloud), including Docker and Kubernetes on this path

### Amazon Web Services

- Access the AWS console and go to the IAM service
- Under Access management, Click on "Users", then "Add users". Insert the User name **luxxy-covid-testing-system-en-app1** and click in **Next** to create a programmatic user.

<img width="1046" alt="Screenshot 2023-02-03 at 09 19 48" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/b292de71-8f13-41a7-928f-26f784493149">

- On Set permissions, Permissions options, click on the "Attach policies directly" button.

<img width="1112" alt="Screenshot 2023-02-03 at 09 27 44" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/d5246f06-a7cb-48b4-88d2-b77c10a3c70b">

- Type **AmazonS3FullAccess** in **Search**
- Select **AmazonS3FullAccess**

<img width="1112" alt="Screenshot 2023-02-03 at 09 31 36" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/6621cc7c-bcb2-4efe-b170-d3742895d8f1">

- Click in **Next**
- Review all details and click on Create user.

<img width="1112" alt="Screenshot 2023-02-03 at 09 32 38 (1)" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/d9a2a926-44ce-4a4c-96e2-820b07bac34e">

### **Steps to create access key:**

- Click on the user you have created.
- Go to the Security credentials tab.
- Scroll down and go to the Access keys section.
- Click on Create access key.

<img width="1130" alt="Screenshot 2023-02-03 at 09 48 38 (1)" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/73aeacbc-4836-4113-a184-055ed39f2525">

- Select **Command Line Interface (CLI)** and **I understand the above recommendation and want to proceed to create an access key** checkbox.
- Click Next.
- Click on Create access key.
- Click on Download .csv file.
- After download, click Done.
- Now, rename .csv file downloaded to **luxxy-covid-testing-system-en-app1.csv**

### Google Cloud Platform (GCP)

- Navigate to Cloud SQL instance and create a new user **app** with password **welcome123456** on Cloud SQL MySQL database.

![SQL User Creation_EDITED](https://github.com/sgummal2/Cloud-Project/assets/140002588/8c898d52-ff61-4c65-a64e-2830fe262e9e)

- Connect to Google Cloud Shell.
- **Download** the mission2 files to Google Cloud Shell using the wget command as shown below:

~~~
cd ~
mkdir mission2_en
cd mission2_en
wget https://tcb-public-events.s3.amazonaws.com/icp/mission2.zip
unzip mission2.zip
~~~

- Connect to MySQL DB running on Cloud SQL (once it prompts for the password, provide welcome123456).

~~~
mysql --host=<public_ip_cloudsql> --port=3306 -u app -p
~~~

- Once you're connected to the database instance, create the products table for testing purposes.

~~~
use dbcovidtesting;
source ~/mission2_en/mission2/en/db/create_table.sql
show tables;
exit;
~~~

- Enable Cloud Build API via Cloud Shell.

~~~
# Command to enable Cloud Build API

gcloud services enable cloudbuild.googleapis.com
~~~

**Known issue during this step**

If you see the error below, please follow the steps to fix it:

~~~
ERROR: (gcloud.builds.submit) INVALID_ARGUMENT: could not resolve source: googleapi: Error 403: 989404026119@cloudbuild.gserviceaccount.com does not have storage.objects.get access to the Google Cloud Storage object., forbidden

To solve it:

1. Access IAM & Admin;
2. Click on check-box Include Google-provided role grants;
3. Click and select your Cloud Build Service Account.

Example: 989404026119@cloudbuild.gserviceaccount.com Cloud Build Service Account

4. On your Cloud Build Service Account, right side, click on Edit principal
5. Click on Add another role
6. Click on Select Role, and filter by Storage Admin or gcs. Select Storage Admin (Full control of GCS resources).
7. Click on Save and go to Cloud Shell.
~~~

- Build the Docker image and push it to Google Container Registry. Please replace the <PROJECT_ID> with your My First Project ID.

~~~
cd ~/mission2_en/mission2/en/app
gcloud builds submit --tag gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-en
~~~

- Open the Cloud Editor and edit the Kubernetes deployment file (luxxy-covid-testing-system.yaml) and update the variables below on line 33 in red with your <PROJECT_ID> on the Google Container Registry path, on line 42 AWS Bucket name, AWS Keys (open file luxxy-covid-testing-system-en-app1.csv and use Access key ID on line 44 and Secret access key on line 46)  and Cloud SQL Database Private IP on line 48.

~~~
cd ~/mission2/en/kubernetes
luxxy-covid-testing-system.yaml

        image: gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-en:latest
...
        - name: AWS_BUCKET
          value: "luxxy-covid-testing-system-pdf-en-xxxx"
        - name: S3_ACCESS_KEY
          value: "xxxxxxxxxxxxxxxxxxxxx"
        - name: S3_SECRET_ACCESS_KEY
          value: "xxxxxxxxxxxxxxxxxxxx"
        - name: DB_HOST_NAME
          value: "172.21.0.3"
~~~

- Connect to the GKE (Google Kubernetes Engine) cluster via Console (follow the video).
- Deploy the application Luxxy in the Cluster.

~~~
cd ~/mission2_en/mission2/en/kubernetes
kubectl apply -f luxxy-covid-testing-system.yaml
~~~

- Get the Public IP and test the application ([CLICK HERE to download COVID-19 Testing result sample](https://tcb-public-events.s3.amazonaws.com/icp/mission2.zip)). Search for **GKE**, click on **Services & Ingress**, and then on **Endpoints** address:port
- You should see the app up & running!

![Screen Shot 2022-02-07 at 11 45 31 AM](https://github.com/sgummal2/Cloud-Project/assets/140002588/f7f7ab3c-ab2b-4ec2-93be-3ae57bd5fa4f)


## Mission 3: Professionally migrating the application files and data from a database

### Google Cloud Platform - Database Migration steps

- Connect to Google Cloud Shell.
- **Download** the dump using wget.

~~~
cd ~
mkdir mission3_en
cd mission3_en
wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
unzip mission3.zip
~~~

- Connect to Cloud SQL MySQL database instance.

~~~
mysql --host=<public_ip_address> --port=3306 -u app -p
~~~

- Import the dump on Cloud SQL.

~~~
use dbcovidtesting;
source ~/mission3_en/mission3/en/db/db_dump.sql
~~~

- Check if the data got imported correctly.

~~~
select * from records;
exit;
~~~


### Amazon Web Services - PDF Files Migration steps

- Connect to the AWS Cloud Shell.
- Download the PDF files.

~~~
mkdir mission3_en
cd mission3_en
wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
unzip mission3.zip
~~~

- Sync PDF Files with your AWS S3 used for the COVID-19 Testing Status System. Replace the bucket name with yours.

~~~
cd mission3/en/pdf_files
aws s3 sync . s3://luxxy-covid-testing-system-pdf-en-xxxx
~~~

- Test the application. Upon migrating the data and files, you should be able to see the entries under the “View Guest Results” page.

![Screen Shot 2022-02-07 at 4 41 37 PM](https://github.com/sgummal2/Cloud-Project/assets/140002588/d25dd697-3f47-478b-8de1-3cbca50b38e8)

**We have successfully migrated an "on-premises" application & database to a MultiCloud Architecture!**















