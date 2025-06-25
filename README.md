# data-analyst-sanjana
Overview of Cloud Computing Class

# Project 

I worked on a project to implement a DAP system for the City of Vanvouver in AWS Cloud. I worked on the current rental issues dataset. The main questions to be answered through this project was as follows:

Which neighbourhoods in Vancouver have the highest number of Average total outstanding rental issues?

Which Vancouver neighbourhoods have the largest number of rental units involved in reported issues?
 ![image](https://github.com/user-attachments/assets/3520aaf6-5977-43c6-9bac-d07b9f719f56)


Figure 1

The raw data is initially ingested to S3 bucket (prj-raw-sanj). Then this data is transferred to Glue DataDrew for profiling and cleaning. The cleaned data is then saved in S3 bucket (prj-cln-sanj). Then crawler is used to scan and create tables in data catalog. This tables are then used for visual ETL. The results of these are then stored in the curated S3 bucket(prj-cur-sanj). Finally this dataset ie the single source of truth is used for answering the descriptive analysis questions using Athena.

# Step 1: Data Ingestion 

I have selected the “Rental standards-Current issue” dataset from the City of Vancouver website. This dataset contains data of licensed rental properties with 5 or more units that have current (unresolved) by-law issues. These issues relate to the enforcement of the Building By-law, Electrical By-law ,Gas Fitting By-law, Plumbing Services (Building By-law - Part 7), Sewer and Watercourse By-law, Sign By-law, Standards of Maintenance By-law, Tree Protection By-law, Untidy Premise By-law, Zoning and Development Bylaw (Rental Standards - Current Issues, 2025). In this assignment, our aim is to transfer this data to the AWS platform for further cleaning and analysis. The aim is to attain the following analysis through AWS platform

 ![image](https://github.com/user-attachments/assets/63acc5f9-1a22-4a82-924b-95696e2394ee)

Figure 2

Initially, the “Rental standards-Current issue” dataset from the city of Vancouver is ingested to the AWS platform. For this we created a raw bucket (prj-raw-sanj) in the AWS platform and a subfolders was created to upload the csv file.

**Analysis**

The goal, objective and cause factors of each descriptive question was analysed.
 ![image](https://github.com/user-attachments/assets/8145de60-5a34-4724-a2cd-5bf82bc4ebfe)

Figure 3

**Design**

	 The fishbone structure for anaylsing the causes was designed
  ![image](https://github.com/user-attachments/assets/9cf34f0f-3e59-47ec-8bfc-7ad94c6ad7a4)

 
Figure 4

The solution architecture for the whole analysis was designed in draw.io. This enables us to know the overall pathway and procedures to be done for implementing the data analystic platform.

 ![image](https://github.com/user-attachments/assets/445261bd-9b64-4c0a-baa4-521c8fcad253)

Figure 5

 ![image](https://github.com/user-attachments/assets/f5a37c5a-0c6a-427c-bfc7-5b4c217375b8)

Figure 6

The datalake design describes the location of the dataset and the Storage class used for storing the dataset. It also provides the tags to be used for the dataset such as read frequency(readfreq), owner, structure of the dataset and write frequency(writefreq).

**Implementation**

	The raw bucket named prj-raw-sanj was created for storing the raw data.
 ![image](https://github.com/user-attachments/assets/3fd5b2e7-3a94-41f5-a6b2-016b0358f85c)

Figure 7
 
 ![image](https://github.com/user-attachments/assets/044bc3e3-896c-40a4-ae3d-15dec3690c2e)

Figure 8

The dataset was stored based on the Datalake design in the subfolder csv.
The Standard storage class was selected so that the data can be accessed in a fast manner as it is used frequently.

 ![image](https://github.com/user-attachments/assets/63e32e22-58cd-4457-8b83-3aa04e30b44e)

Figure 9

# Step 2: Data Profiling

**Analysis, design and Implementation**

The Glue Databrew was used for profiling. Profiling is done to understand the dataset better and to identify the issues and suitable cleaning techniques (Creating and Working With AWS Glue DataBrew Profile Jobs - AWS Glue DataBrew, n.d.).
A project is created in Glue Databrew to analyse our raw dataset. This enables us to check the quality, structure and irregularities in the dataset.
 ![image](https://github.com/user-attachments/assets/13b2ac53-2e0f-4e0a-81b8-0ad68bb65915)

Figure 10

 ![image](https://github.com/user-attachments/assets/e9677b9d-15af-45ae-8935-ba8ff6e8707b)
 
Figure 11

There are some missing values and outliers found in some columns. These should be removed for further processing. A second bucket called prj-cln-sanj was created to store the result of the profiling job. 

 ![image](https://github.com/user-attachments/assets/ba776a9e-e3e3-4335-a77e-a8331d6f4c26)

Figure 12

 ![image](https://github.com/user-attachments/assets/a068fbfb-72ab-4565-b8eb-ae97a625ff71)

Figure 13

	The profiling job is successfully done which is indicated by the json file as a result of profiling.


# Step 3: Data Cleaning

**Analysis**

The profiling job indicated the columns with issues which are listed below. Some columns had missing values we plan on rectifying them by removing the rows as only less than 5 values where missing. 
 ![image](https://github.com/user-attachments/assets/cbeb418b-8f1b-445e-8090-3fd5b7893db3)

Figure 14

**Implementation**

The rows containing missing values in business operater and Geom was removed. As a result all the null values from the columns where removed. The following screenshots shows the recipe used for cleaning and the completed job.
 ![image](https://github.com/user-attachments/assets/2f7a1d27-01d1-4056-9a9a-b669c291ee73)

Figure 15

 ![image](https://github.com/user-attachments/assets/cf85f282-fc7d-4598-8d82-103805be075d)

Figure 16

The results of the cleaning job are stored in two formats, one user-friendly format (CSV) and the other is system-friendly format (Snappy), in the prj-cln-sanj bucket. 
 ![image](https://github.com/user-attachments/assets/4330a8d4-1b96-4217-b415-084b8198f410)

Figure 17

 ![image](https://github.com/user-attachments/assets/2a24b276-54ca-4197-8f5c-0fe69fe4ded7)

Figure 18

# Step 4: Data Cataloging

Analysis

	The descriptive questions where analysed further and the metric for analysis is found.
 
 ![image](https://github.com/user-attachments/assets/51455f3c-44d1-4eef-b597-8253a2a94ff3)

Figure 19

Design

![image](https://github.com/user-attachments/assets/7cb7f10e-e5d4-4fa1-b841-7505b32e2011)

 
Figure 20

Implementation

A third bucket prj-cur-sanj was created to store the curated dataset.
The crawler is used to convert all semi-structured and unstructured datasets to table format. This tables are then stored in the data catalog folder (prj-data-catalog-san) as databases.

 ![image](https://github.com/user-attachments/assets/8ed8d6b2-8978-4c2e-8a98-fa6762db237c)

Figure 21

 ![image](https://github.com/user-attachments/assets/d7e777ff-ed71-4916-b161-aa9c8749c068)

Figure 22

	AWS Glue was used for ETL pipeline. That is to extract, transform and load the curated dataset. The ETL along with datacatalog is used for the generation of single source of truth (SSOT). 
The dataset from the data catalog is initially source for Visual ETL. We then changed the schema of the dataset in the transformation phase by dropping unnecessary columns. Then the curated bucket is used as target generate the system friendy and user friendly outputs of the SSOT. In this case SSOT is jobrenstd. This SSOT is used for the analysis of descriptive questions. 

 ![image](https://github.com/user-attachments/assets/55f59bb2-d006-49a1-8f41-ac0bf0fa97ff)

Figure 23

![image](https://github.com/user-attachments/assets/925568ad-3334-4721-8ee8-a03211b41801)

 
Figure 24

# Step 5: Data Summarization 

**Analysis**

![image](https://github.com/user-attachments/assets/57c1003e-729b-4200-8dbf-e5d5ff6d5271)

Figure 25

Implementation

	AWS Athena is used for summarization. And the sql queries are used in athena to get the results for our descriptive questions. 
The First descriptive question 

Which neighbourhoods in Vancouver have the highest number of Average total outstanding rental issues? 

As per the analysis from athena we can see that Killarney is the neighbourhood with highest number of Average total outstanding rental issues

![image](https://github.com/user-attachments/assets/50db40e8-b2ee-4edc-bbfe-50890e8c8260)

Figure 26

Which Vancouver neighbourhoods have the largest number of rental units involved in reported issues?

As per our analysis the same neighbourhood ie killarney, is found to have the largest number of rental units involved in reported issues.

![image](https://github.com/user-attachments/assets/28d777f5-b099-4ad4-8e99-95d45f1bcd57)

 Figure 27


# Step 6: Data Security

For the security of the data from the city of Vancouver in AWS we have used encryption and decryption process using Key Management Service in AWS. This service enables us to create a key that can be used for the encryption and decryption process. The figure below shows the Key that we have created. This key that we have created can be used to encrypt and decrypt our data during uploading and downloading, respectively, in different buckets such as raw, clean, and curated buckets.

Figure 28

_Key Management Service_

![image](https://github.com/user-attachments/assets/6e33db09-23d3-4d2f-abe7-60a36d0bd72e)

 
We have edited the Encryption in the raw bucket to the key we have created so that the data is encrypted during upload to the bucket and is decrypted when it is downloaded by the user with the encryption key.
We have also enabled bucket versioning. This creates multiple versions of the file ie, after each change a new file is created rather than deleting the old file. This provides multiple copies of the file. The same procedure of Data encryption and Bucket versioning is repeated for the clean bucket as well as the curated bucket.

Figure 29

_Raw Bucket: Data Encryption and Bucket Versioning_

![image](https://github.com/user-attachments/assets/5a202371-b3f3-46fd-9a71-869e080fde8b)

 
All the objects in the raw bucket are replicated to a backup bucket (prj-raw-bac-sanj). This is done to protect the data from being lost or accidentally deleted. This backup bucket is also encrypted. For this, we use the replication rule. 
This is also done for the clean and curated bucket.

Figure 35

_Raw Bucket: Replication Technique_

![image](https://github.com/user-attachments/assets/18a99d4d-e622-49b8-95f1-3d61c8c14305)

 
Figure 30

_Clean Bucket: Data Encryption and Bucket Versioning_

![image](https://github.com/user-attachments/assets/b93bae79-3c75-4065-84df-a2cc021924ef)


Figure 31
  
_Clean Bucket: Replication Technique_

![image](https://github.com/user-attachments/assets/e0f84abf-cada-4724-8f2d-5e3527846e30)

 
Figure 32

_Curated Bucket: Data Encryption and Bucket Versioning_

![image](https://github.com/user-attachments/assets/7224fc33-3621-4f17-a66f-e4e93639fdc0)

 
Figure 33

_Curated Bucket: Replication Technique_

![image](https://github.com/user-attachments/assets/4f21b0e7-3073-4cd5-931e-e87b211a58bb)

 
# Step 7: Data Governance

We have implemented data quality in the data governance section. For this, I have used the AWS Glue service. 
In AWS Glue, Visual ETL was used to create the pipeline. Initially, the data from the raw bucket was extracted, and it was used as the source. Then, the data quality of the CSV file in the raw bucket was checked. For this, we mainly considered parameters such as completeness, uniqueness, and freshness of data. Then, a new column was added to check which columns passed the quality check and which didn’t. A conditional router was then used to split the data into two groups, with one containing the passed rows and the other containing the failed rows. We had around 30 rows that failed the quality check. The extra columns we added during quality check are then removed using the change schema option available in the transform. Then, for just creating one file rather than multiple small files, we used the Autobalance processing option. Finally, this file is saved in the transform bucket we created (pro-trf-sanj). The failed is stored in the failed folder within the Quality_check folder in the transform bucket, and the passed rows are in the passed bucket.

Figure 34

_Visual ETL: Quality Control_

![image](https://github.com/user-attachments/assets/7076e2ea-f0d3-4682-8658-03fc0290b825)

 
Figure 35

_Passed Folder: Run output_

![image](https://github.com/user-attachments/assets/bcb45dd8-bcf0-4033-8bc5-3d4e313fef36)

Figure 36

_Failed Folder: Run output_

![image](https://github.com/user-attachments/assets/e30864e3-082b-4cd2-93ff-85ec628a9d42)

 
# Step 8: Data Monitoring

For data monitoring, AWS CloudWatch was used. A monitoring dashboard(prj-ren-MCR-san) was created. The raw bucket bytes used within the last 3 months were tracked using the widget. Then we also created a widget for job run time in the Glue to track the performance. An alarm was also created to control the bucket byte used to not exceed a threshold of 300 KB, since we have already used around 150 KB. To get notified when it is exceeded, we created a notification topic and provided the email ID to which AWS will notify.

Figure 37

_CloudWatch: Dashboard_

![image](https://github.com/user-attachments/assets/8c8c0dc6-9730-48c7-8891-7a3f425ddbbf)

 
For tracking user activity and creating a trail, CloudTrail was used. This helps us gather all activities within our account. If we check the event history option in CloudTrail, all actions done to date in the account can be tracked. This CloudTrail creates a log file of the activities done.

Figure 38

__CloudTrail_

![image](https://github.com/user-attachments/assets/63fadf5a-6855-45ba-8304-80f34ae203c6)

 

 

