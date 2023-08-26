# Cloud-Project
Luxury Hotels &amp; Resorts company migrating applications and data to the cloud.

## Steps to implement Hands-on Project- Mission 1

### Amazon Web Services
•	 Access the AWS console and go to the IAM service
•	 Under Access management, Click on "Users", then "Add users". Insert the User name **terraform-en-1** and click in **Next** to create a programmatic user.

![Untitled](https://github.com/sgummal2/Cloud-Project/assets/140002588/37149595-52e3-4b70-91e7-8611363e42df)

•	On Set permissions, Permissions options, click on the "Attach policies directly" button.

<img width="1000" alt="image" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/be01e136-c14d-4924-80f8-b3331d4bd964">

•	 Type **AmazonS3FullAccess** in **Search**.
•	 Select **AmazonS3FullAccess**

![Untitled (1)](https://github.com/sgummal2/Cloud-Project/assets/140002588/d61869b5-2672-4438-9a83-b11ec6cdedee)

•	 Click in **Next**
•	 Review all details, then click **Create user.**

<img width="1112" alt="Screenshot 2023-02-03 at 09 32 38" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/0a953753-f554-4aa6-a5de-db927a9d8ee3">

**AWS has recently changed the way to download the key. Follow the new steps:**

- Click on the user you have created.
- After this, click on the **Security credentials** tab.
- Scroll down and go to the Access keys section.
- Click on **Create access key**

- <img width="1130" alt="Screenshot 2023-02-03 at 09 48 38" src="https://github.com/sgummal2/Cloud-Project/assets/140002588/c45191a4-a984-4a12-b565-467b9bad4c14">

•	 Select **Command Line Interface (CLI)** and **I understand the above recommendation and want to proceed to create an access key** checkbox.
•	 Click **Next**.
•	 Click on **Create access key**
•	 Click on **Download .csv file**
•	 After the download is finished, click on **Done**

### [TIP] **Access key best practices**
- Never store your access key in plain text, in a code repository, or in code.
- Disable or delete the access key when no longer needed.
- Enable least-privilege permissions.
- Rotate access keys regularly.

•	 After download, click **Done**.
•	 Now, rename .csv file downloaded to **accessKeys.csv**

### Google Cloud Platform (GCP)

•	download the mission1.zip hands-on files.
•	Access GCP Console and open Cloud Shell
•	Upload **accessKeys.csv** and **mission1.zip** hands-on file to GCP Cloud Shell
• Check if the upload has been successfully completed using the command **ls -la**
• Hands-on files preparation

hlt "mkdir mission1_en
mv mission1.zip mission1_en
cd mission1_en
unzip mission1.zip
mv ~/accessKeys.csv mission1/en
cd mission1/en
chmod +x *.sh" -c yellow










