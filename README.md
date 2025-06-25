# data-analyst-sanjana
Overview of Cloud Computing Class
# **Project** 
 Which neighbourhoods in Vancouver have the highest number of Average total outstanding rental issues? 
Which Vancouver neighbourhoods have the largest number of rental units involved in reported issues?
 ![image](https://github.com/user-attachments/assets/3520aaf6-5977-43c6-9bac-d07b9f719f56)


Figure 1
The raw data is initially ingested to S3 bucket (prj-raw-sanj). Then this data is transferred to Glue DataDrew for profiling and cleaning. The cleaned data is then saved in S3 bucket (prj-cln-sanj). Then crawler is used to scan and create tables in data catalog. This tables are then used for visual ETL. The results of these are then stored in the curated S3 bucket(prj-cur-sanj). Finally this dataset ie the single source of truth is used for answering the descriptive analysis questions using Athena.

Step 1: Data Ingestion 
I have selected the “Rental standards-Current issue” dataset from the City of Vancouver website. This dataset contains data of licensed rental properties with 5 or more units that have current (unresolved) by-law issues. These issues relate to the enforcement of the Building By-law, Electrical By-law ,Gas Fitting By-law, Plumbing Services (Building By-law - Part 7), Sewer and Watercourse By-law, Sign By-law, Standards of Maintenance By-law, Tree Protection By-law, Untidy Premise By-law, Zoning and Development Bylaw (Rental Standards - Current Issues, 2025). In this assignment, our aim is to transfer this data to the AWS platform for further cleaning and analysis. The aim is to attain the following analysis through AWS platform
 ![image](https://github.com/user-attachments/assets/63acc5f9-1a22-4a82-924b-95696e2394ee)

Figure 2
Initially, the “Rental standards-Current issue” dataset from the city of Vancouver is ingested to the AWS platform. For this we created a raw bucket (prj-raw-sanj) in the AWS platform and a subfolders was created to upload the csv file.
Analysis
The goal, objective and cause factors of each descriptive question was analysed.
 ![image](https://github.com/user-attachments/assets/8145de60-5a34-4724-a2cd-5bf82bc4ebfe)

Figure 3

Design
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
Step 2: Data Profiling
Analysis, design and Implementation
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


Step 3: Data Cleaning
Analysis
The profiling job indicated the columns with issues which are listed below. Some columns had missing values we plan on rectifying them by removing the rows as only less than 5 values where missing. 
 
Figure 14
Implementation
The rows containing missing values in business operater and Geom was removed. As a result all the null values from the columns where removed. The following screenshots shows the recipe used for cleaning and the completed job.
 
Figure 15

 
Figure 16

The results of the cleaning job are stored in two formats, one user-friendly format (CSV) and the other is system-friendly format (Snappy), in the prj-cln-sanj bucket. 
 
Figure 17
 
Figure 18

Step 4: Data Cataloging
Analysis
	The descriptive questions where analysed further and the metric for analysis is found.
 
Figure 19
Design
 
Figure 20
Implementation
A third bucket prj-cur-sanj was created to store the curated dataset.
The crawler is used to convert all semi-structured and unstructured datasets to table format. This tables are then stored in the data catalog folder (prj-data-catalog-san) as databases.

 
Figure 21

 
Figure 22
	AWS Glue was used for ETL pipeline. That is to extract, transform and load the curated dataset. The ETL along with datacatalog is used for the generation of single source of truth (SSOT). 
The dataset from the data catalog is initially source for Visual ETL. We then changed the schema of the dataset in the transformation phase by dropping unnecessary columns. Then the curated bucket is used as target generate the system friendy and user friendly outputs of the SSOT. In this case SSOT is jobrenstd. This SSOT is used for the analysis of descriptive questions. 

 
Figure 23
 
Figure 24
Step 5: Data Summarization 
Analysis
 
Figure 25
Implementation
	AWS Athena is used for summarization. And the sql queries are used in athena to get the results for our descriptive questions. 
The First descriptive question 
Which neighbourhoods in Vancouver have the highest number of Average total outstanding rental issues? 
As per the analysis from athena we can see that Killarney is the neighbourhood with highest number of Average total outstanding rental issues
 
Figure 26
Which Vancouver neighbourhoods have the largest number of rental units involved in reported issues?
As per our analysis the same neighbourhood ie killarney, is found to have the largest number of rental units involved in reported issues.
 Figure 27

