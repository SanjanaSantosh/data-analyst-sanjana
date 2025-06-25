# data-analyst-sanjana
# UCW Dataset to AWS Cloud
# Descriptive Analysis 

**Project Description** : Descriptive analysis of Employee performance after training

**Project Title** : Improvement in Employee performance

**Objective** : The Primary goal of this project is to conduct a descriptive analysis to find the most effective training method to improve the performance of employees

**Dataset 1 (Training Attendance Records)** : It included the completetion status of each employees in different training topics

TrainingID: The unique identifier for training an employee

EmployeeID: The unique identifier of an employee

TrainingDate: The date on which training was done

TrainingTopic: The topic on which traing was done for the employees

CompletionStatus : Status of Training

**Dataset 2 (Employee Performance Metrics) :** 

MetricID : Unique Identifier for tracking employee performance metrics

EmployeeID: The unique identifier of an employee

MetricDate: The date on which the performance was noted

PerformanceScore: The performance score of each employees

**Methodology**

**Data Ingestion**

I created and implemented a DAP system in AWS cloud to answer these questions.

For solving the descriptive question we initially analysed the data to find the cause and effects

Figure 1 

_FIshbone Design_

![image](https://github.com/user-attachments/assets/bbc8924c-41d3-4f78-b2c1-f268a95d7fff)

Then we designed the datalake it helps to assign the folder in which our data is to be placed and also it describes the region and storageclass we are planning to assign for our storage bucket.

Figure 2

_Datalake_

![image](https://github.com/user-attachments/assets/3fd70593-c35e-42a9-9bfc-92ac04927491)

Figure 3

_DataLake Desgin_

![image](https://github.com/user-attachments/assets/51a53de8-5b8d-4857-a66c-33d120f13f7e)

This shows the operation environment and where exactly our dataset is located. Initially data is sent from the UCW HR team and then it is stored to a virtual server by uploading the data to a S3 Bucket in cloud.

Initially we have to sign in to the AWS account and then we have to create a VPC. It provides a private network environment for UCW's cloud resources.

Figure 4

_VPC_

![image](https://github.com/user-attachments/assets/53476a46-d67b-425a-873d-d13cc50805fd)

Figure 5

_Security Groups_

![image](https://github.com/user-attachments/assets/c2cd341d-2bce-498f-bf68-005126cdd52c)

Then security networks where set as it act as virtual firewalls that control network traffic to and from your instances, providing a crucial layer of security for your cloud environment.

Figure 6

_Instances_

![image](https://github.com/user-attachments/assets/03bf21e9-0f3a-41e1-9324-bf000163465b)

Then we set up instances in cloud ie a virtual machine to implemen tasks

Then in the storage ie S3 we created a raw bucket(hr-raw-sanj) to store the data. Then the datasets were stored in this folder as per the datalake desgin

**Dataset Profiling and Cleaning** 

**Analysis & Design**

After the data is stored in Cloud the next step is to Clean the datasets. For cleaning dataset we used AWS databrew. We initially did data profiling to check the irregularities in the datasets. As per the profiling both datasets were clean and had no missing values or outliers

Figure 7

_Data Profile for Employee Performance Metrics_

![image](https://github.com/user-attachments/assets/e14cfabb-c8ea-4ec7-8790-d58e0e2d673c)

Figure 8

_Data Profile for Training Attendance Records_

![image](https://github.com/user-attachments/assets/9c5270ef-b58a-4c78-87cc-b4a61ca9af6c)

**Dataset Cleaning Implementation**

Then we saved the clean dataset to a new S3 bucket called hr-cln-san. This was saved in two formats one user friendly format ie CSV and other in snappy compression for the systems friendly format. 

Figure 9

_Training Attendence Records: User Friendly(csv)_

![image](https://github.com/user-attachments/assets/4b717533-2649-428d-a72a-02da73d80038)

Figure 10

_Training Attendence Records: System Friendly (snappy)_

![image](https://github.com/user-attachments/assets/fe94e6bd-e547-4052-85e2-9530c1b9d418)

Figure 11

_Employee Performance Metrics:  User Friendly(csv)_

![image](https://github.com/user-attachments/assets/c4837338-54c4-4085-9642-6bb5a50fb946)

Figure 12

_Employee Performance Metrics:  System Friendly (snappy)_

![image](https://github.com/user-attachments/assets/1a870e7f-a30b-4282-ae8a-95552278c030)

**Evaluvation**

We also evaluvated the cost of Implementing S3 storage and Glue Databrew services in AWS

Figure 13

_Cost Estimation for S3 storage_

![image](https://github.com/user-attachments/assets/7c5daf9b-1064-424c-8ae5-b07723dcfba1)

_Cost Estimation for Glue DataBrew_

![image](https://github.com/user-attachments/assets/49d86ce4-8969-4be9-8b6a-37bc38970385)

**Data Catalog**

A third bucket hr-cur-sanj was created to store the curated dataset. AWS Glue was used for this.
The crawler is used to convert all semi-structured and unstructured datasets to table format. This tables are then stored in the data catalog folder (hr-data-catalog-san) as databases.

Figure 14

_Data catalog_

![image](https://github.com/user-attachments/assets/a43b24d8-71be-415c-aba6-6f413935c7fb)

Figure 15

_Curated- User Friendly_

![image](https://github.com/user-attachments/assets/e454b4b0-ab0d-4e09-a46b-572f112eed3c)

Figure 16

_Curated - Systems Friendly_

![image](https://github.com/user-attachments/assets/daabaa01-95de-4f5f-8054-d0bb06ffba99)



**Dataset Enriching and Summarization**

Our main aim is to create a dataset that can answer the business questions we have. This dataset is called as Single source of truth(SSoT). So to attain it we might have to join 2 or more datsets together. This process is called enriching. We follow an ETL pipeline to achive this in AWS.

Figure 17

_ETL Design_

![image](https://github.com/user-attachments/assets/64624629-f112-4152-b909-2ffa246821de)
![image](https://github.com/user-attachments/assets/d79ae27e-e91b-48a6-a06b-c29ae4b9b589)

Figure 18

_ETL Desgin in DAP_

![image](https://github.com/user-attachments/assets/6bd01adc-efa0-48cc-9821-1b2f6270e323)

After desging the DAP we initially set an alarm in AWS to notify us if the Cost exceeds a certain limit. CloudWatch serve was user for setting up this alarm.

Figure 19

_ALarm_

![image](https://github.com/user-attachments/assets/71f455a1-518a-4b32-84a5-83f1dd4b5912)

Then we prepared a Visual ETL to join the two dataset using inner joint to create the single source of truth. A common column is required for merging two datasets. In our datasetthe common column is employee id

Figure 20

_VIsual ETL_

![image](https://github.com/user-attachments/assets/45e65775-f572-4ada-9b3a-c32691cfbdae)

Finally the single source of truth from the ETL pipeline is used to answer the Business questions. AThena ia used for this. In Athena we use Sql 

Figure 21

_Athena_

![image](https://github.com/user-attachments/assets/598ed210-48f4-46b2-a0d7-9976e8a5b143)

**Tools and Technologies:**
Excel for initial data storage and analysis

Draw.io for designing

AWS Services Such as S3 , Glue, GlueBrew athena, Pricing Calculator was used for implementation

# AWS Deployment and Service Models 

Figure 22

![image](https://github.com/user-attachments/assets/4aa28153-b725-4f31-aa8b-47963d9f8bf4)


Figure 23

![image](https://github.com/user-attachments/assets/e40a8ae9-9345-4786-9dd8-7223248f81f7)  ![image](https://github.com/user-attachments/assets/3a9d2576-ea70-4911-8617-3e9a95a91619)

Figure 24

![image](https://github.com/user-attachments/assets/4ae31c62-3b3f-415e-ba8b-00e69228d15f) ![image](https://github.com/user-attachments/assets/07fa13d6-47b7-4e67-93b0-3f132e4f5d05)

Figure 25

![image](https://github.com/user-attachments/assets/cd6026aa-475c-4ed6-af98-4b9b1bf815c0)

# AWS Cost Analysis 

Figure 26

![image](https://github.com/user-attachments/assets/8e76e089-1ec5-48f4-91ef-672c12c3b845)

Figure 27

_Case Study #5: AWS Pricing Calculator_

![image](https://github.com/user-attachments/assets/dbf93716-e3b3-498b-ba82-1d0a1fecde58)

![image](https://github.com/user-attachments/assets/7ea91305-4dbb-475b-8558-007e725bb18a)

![image](https://github.com/user-attachments/assets/f9994e2e-6fbe-4087-b513-cc6296bc2959)

Figure 28

![image](https://github.com/user-attachments/assets/fa1fa337-4f5a-498a-8192-fe7a057d2528)

Figure 29

![image](https://github.com/user-attachments/assets/d0bf2e13-0248-4585-b542-b179d33d544b)

# AWS Global infrastructure 

Figure 30

![image](https://github.com/user-attachments/assets/1e629900-13be-4149-b0fe-dc8eb6def02d)

Figure 31

![image](https://github.com/user-attachments/assets/3d9c03e4-e7d2-43e0-b922-b3ee4917933c)

# AWS IAM 

Figure 32

![image](https://github.com/user-attachments/assets/a512c58e-9fb0-499d-acfe-993a36539247)

Figure 33

![image](https://github.com/user-attachments/assets/fb18faa1-afbb-4706-ba5a-47797b2388d1)

Figure 34 

![image](https://github.com/user-attachments/assets/83087d3a-5cf1-4e5f-92fd-c39216e44875)


# AWS VPC

# AWS Lambda 

# AWS EBS


