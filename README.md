# EtLT-pipeline-in-AWS
Full data load of data from local file into AWS and creating a pipeline to prepare data to analysis by analysts

### Goal:
To improve Data engineering skills in conducting ELT workflow and preparing data for analysis process

### Motivation:
- Getting hands-on experience on cloud solutions using AWS
- Improving understanding and best practices in performing ELT process as a data engineer


### Project idea overview:

![image](https://github.com/user-attachments/assets/f2e633e7-879f-4b38-80f3-7f8156bcecec)



### Process of creating project and challenges:
![image](https://github.com/user-attachments/assets/949c4c27-e5d9-4b43-9fee-592792fa0ff6)

![image](https://github.com/user-attachments/assets/2d1dc610-ae52-480e-b573-0566e9fb538f)


##### Data overview:
In order to get to know data that I will be working on I give them a look using Athena. Here is the result of the query:
![image](https://github.com/user-attachments/assets/f08b0641-cc34-43d4-aebd-ac665b76ab4d)  ![image](https://github.com/user-attachments/assets/64bf7687-99e4-4d84-b163-91e1da104b4b)

At first I noticed some problems occuring like:
- wrong column names (first row should be used as a header),
- duplicated index column (col0 and col1),
- a lot of missing values (col7, col8)
- wrong data types
  
Since dealing with some of these problems does not require any business logic being applied, I decided to perform these minor transformations before loading data into data warehouse therefore final pipeline is EtLT where non-capital "t" stands for applying non business logic transformations.


##### Extract + transform:
To get my desired result before loading data into warehouse I used AWS Glue to orchestrate a simple pipeline:
![image](https://github.com/user-attachments/assets/c2cd978e-82fb-4430-8186-1458a997e871)


##### Load:
Having data after some basic editing I decided to load it into Redshift datawarehouse to perform more transformations (including Business Logic) and model data into schema.

![image](https://github.com/user-attachments/assets/233d840b-b8bb-499d-866b-dc0596c1a75a)

![image](https://github.com/user-attachments/assets/6c6617f1-f41c-4a20-a576-e991ff213b1d)


##### Transform:
![image](https://github.com/user-attachments/assets/43907173-4651-49c2-a6ea-fdeda830585f)

Deleting duplicated rows:
![image](https://github.com/user-attachments/assets/74cec1b9-862b-4baa-adf6-8b0e47c285a3)
![image](https://github.com/user-attachments/assets/dc77504a-d957-4b76-83c1-6e04dd871897)
![image](https://github.com/user-attachments/assets/efbda971-f77b-4248-a387-8a82d5e5094e)


Modeling data:

Modeling data often involves normalization; however, tables normalized to 3NF are not always optimized for OLAP environments. Since this project aims to deliver data to analysts, it is better to use a Kimball schema (star schema) to both address data redundancy and enable effective data analysis.

![image](https://github.com/user-attachments/assets/be9fa040-9f3e-4fbb-8f65-f4f3d218bd9f)


![image](https://github.com/user-attachments/assets/58a2598f-ee55-4c19-82ed-8bb9f6681a97)

![image](https://github.com/user-attachments/assets/dd59b65c-ac48-4615-ad75-9d12d9197298)

![image](https://github.com/user-attachments/assets/d986f786-5c68-408e-a827-25995968059a)

![image](https://github.com/user-attachments/assets/eadd88e2-0034-4a13-8bdb-42a74e1c5fb9)


In both loading data in AWS Glue and Redshift I saved data as Parquet format.
Parquet format is one of the most popular formats for analysis purpose.
- It is columnar format not row based (i.e. CSV) thus analyst can load only selected column and not whole file for analysis purpose if he decides that some columns are unnecessary which can save a lot of time and money if are talking about big data.
- It also compresses files by storing repeated values only once and saving the count of their occurrences as integers. This reduces storage space while preserving the same data on read.

To summarize Parquet format can strongly boost performance, especially for large datasets and complex queries. Even though Data that I am operating on has less than 1MB I still find it as a good practice to always use parquet to store data for analysis purpose.

#### Result:
As the result the data is stored in efficient columnar format in s3 bucket, ready for further analysis process.




 
