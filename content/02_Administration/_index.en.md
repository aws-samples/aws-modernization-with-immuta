+++
title = "Module 1 - Administration"
chapter = true
weight = 3

+++

## **Module 1: How Administrators configure Immuta to *Work Natively* with Amazon Redshift**

<img src="https://www.flightsafetyaustralia.com/wp-content/uploads/2018/11/WEB-AdobeStock_85669666.jpg" alt="One thing at a time: a brief history of the checklist | Flight Safety  Australia" style="zoom:20%;" />

ABC Corp currently uses Amazon Redshift and there is already well-defined roles for controlling access to Data. Can Immuta work with this existing structure? Let's find out...

In this module you will:

- Access and validate the Lab Environment

- Create three Redshift Users (for ***Anne Analyst, Manny Manager,*** and ***Owen Owner***)
  - Create and Populate the ***credit_card_transactions*** table in Redshift;

- Configure Immuta's ***Native Integration with Redshift***

---



| 1.0 Access and Validate the Lab Environment |
| ------------------------------------------- |


In order to complete this Lab, AWS will provision a dedicated account that will be setup with the following services:

- Redshift Cluster (Empty)

- Immuta Instance (Partially-Configured)

Since this is a temporary on-demand environment you will be given access when it is provisioned via a hashcode that will be emailed to you.   ***You will have 4 hours*** to complete the lab before the environment will be deleted. 

---



| Steps: | Instructions: |
| ------------------------------------------------------------ | :----------------------------------------------------------- |
| 1. Get your "Event Engine" Hashcode <br />(e.g. **8291-1a5f66a314-93**) | **Hashcode** will be sent to your Email from AWS.            |
| 2. Use the Hashcode on the Dashboard to login<br /><br />3. Choose Email a One-Time Password (OTP) | https://dashboard.eventengine.run/login  <img src="/images/gifs/2021-10-07 11.00.36.gif" alt="2021-10-07 11.00.36" style="zoom:30%;" /> |
|                                                              |                                                              |
| 4. a **One-Time passcode** is sent to your Email<br />5. Enter the One-Time Passcode  and open the AWS Console<br /> | <br /><img src="/images/gifs/2021-10-07 11.01.36.gif" alt="2021-10-07 11.01.36" style="zoom:30%;" /><br /> |
|                                                              |                                                              |
| 6. Login to Immuta<br /><br />7. Login to Redshift />        | **Immuta** is a publicly exposed EC2 Instance,<br /> Login with ImmutaAdmin@immuta.com and 'password'<br /><br /><img src="/images/gifs/2021-10-14 22.39.48.gif" alt="2021-10-14 22.39.48" style="zoom:33%;" /><br />**Redshift** is a publicly exposed cluster,<br /> -- Login with "immuta" to database "demodb" <img src="/images/gifs/image-20211014150241867.png" alt="image-20211014150241867" style="zoom:20%;" /><br /><br />-- select * from pg_catalog.pg_user <br />to validate connectivity  and that all **5 users** exist<img src="/images/gifs/2021-10-14 22.27.37.gif" alt="2021-10-14 22.27.37" style="zoom:33%;" /> |
|                                                              |                                                              |
|                                                              |                                                              |

**Copy this config info into a Text Editor :**

```yaml
###Lab Scratchpad
Immuta Url:  [PASTE your immuta url here]

Amazon-Resource-Name for Redshift S3 Role(ARN): [PASTE your ARN here]

---

Redshift Host: [PASTE immuta url here]

Redshift port: 5439

Redshift db: demodb

Super-user[/ password]: immuta / AW$D3VdAy$

---

Pre-Configured Immuta Admin Username [/ password]: ImmutaAdmin@immuta.com / password
Corresponding Redshift Username [/ Password ]:  immuta / AW$D3VdAy$

Pre-Configured Immuta Analyst Username [/ password]:  analyst@immuta.com / password
Corresponding Redshift Username [/ Password ]:  analyst@immuta.com / DontGuessM3

Pre-Configured Immuta Data Owner Username [/ password]:  owner@immuta.com / password
Corresponding Redshift Username [/ Password ]:  owner@immuta.com / DontGuessM3

Pre-Configured Immuta Data Owner Username [/ password]:  manager@immuta.com / password
Corresponding Redshift Username [/ Password ]:  manager@immuta.com / DontGuessM3

For Module 4 -Add a new column: alter table public.credit_card_transactions add column New_column varchar default 'secret';

```

Find and populate the scratchpad with the following 3 values based on your environment :

- Immuta Url

- Redshift Host

- Role ARN for RedshiftFor one of the statements we must reference an ARN Role. This is to allow access to our source data in S3.

  | Steps: | Instructions: |
  | -----------------------------------------------------------: | ------------------------------------------------------------ |
  |                                           Find the ***ARN*** | Navigate to the Redshift Cluster page of the AWS console and view the **Properties tab**.<br /><br /><img src="/images/gifs/image-20211014150611815.png"  style="zoom:30%;" /> <br /> |
  |                                                              |                                                              |
  |                             Copy the ARN to  your Scratchpad | Click on the **copy icon** <img src="/images/gifs/image-20211014151248968.png" style="zoom:25%;" />  in the Cluster Permissions section ( use the ARN in the SQL script below.)<br /><img src="/images/gifs/image-20211014151136477.png"  style="zoom:30%;" /> |
  |                                                              |                                                              |

  

**Filling in your Scratchpad information:**<img src="/images/gifs/2021-10-15 10.31.49.gif" alt="2021-10-15 10.31.49" style="zoom:33%;" />



| 1.1 Create and Populate the ***credit_card_transactions*** table in Redshift; |
| ------------------------------------------------------------ |


For our Lab we are going to create and load 2 tables.

We will use the Redshift Query Editor to execute these SQL statements.

| If your Redshift Cluster is in ***US-EAST1*** <br /><img src="/images/gifs/image-20211014234237850.png" alt="image-20211014234237850" style="zoom:25%;" /><br />Run this Statement sequence in the SQL Editor |
| ------------------------------------------------------------ |

```sql
--we will create two users. one is an analyst, the other is a finance user:
create user "analyst@immuta.com" password 'DontGuessM3';
create user "owner@immuta.com" password 'DontGuessM3';
create user "manager@immuta.com" password 'DontGuessM3';

grant usage on schema public to "owner@immuta.com";

CREATE TABLE public.stage_credit_card_transactions ( id character varying(256) ENCODE lzo, customer_last_name character varying(256) ENCODE lzo, credit_card_number character varying(256) ENCODE lzo, transaction_location character varying(256) ENCODE lzo, transaction_time character varying(256) ENCODE lzo, transaction_amount character varying(256) ENCODE lzo, transaction_country character varying(256) ENCODE lzo ) DISTSTYLE AUTO;

--this is a public s3 bucket where the raw data lives. Make sure you register an IAM role to your Redshift cluster and then use that arn id in the below copy from:


COPY public.stage_credit_card_transactions
 FROM 's3://immuta-sa-team/public/aws_dev_day/datasets/credit_card_transactions.snappy.parq' FORMAT as PARQUET
 IAM_ROLE 'PASTE_YOUR_ARN_HERE' ;

CREATE TABLE public.credit_card_transactions as (select cast(id as bigint) id, customer_last_name , credit_card_number, transaction_location , cast(transaction_time as timestamp) transaction_time, cast(transaction_amount as decimal(18,2)) transaction_amount , transaction_country from  public.stage_credit_card_transactions );

GRANT ALL PRIVILEGES ON TABLE public.credit_card_transactions to "owner@immuta.com";
```



| If your Redshift Cluster is in ***US-EAST2*** <br /><img src="/images/gifs/image-20211014234118120.png" alt="image-20211014234118120" style="zoom:25%;" /><br />Run this Statement sequence in the SQL Editor |
| ------------------------------------------------------------ |

```sql
--we will create two users. one is an analyst, the other is a finance user:
create user "analyst@immuta.com" password 'DontGuessM3';
create user "owner@immuta.com" password 'DontGuessM3';
create user "manager@immuta.com" password 'DontGuessM3';

grant usage on schema public to "owner@immuta.com";

CREATE TABLE public.stage_credit_card_transactions ( id character varying(256) ENCODE lzo, customer_last_name character varying(256) ENCODE lzo, credit_card_number character varying(256) ENCODE lzo, transaction_location character varying(256) ENCODE lzo, transaction_time character varying(256) ENCODE lzo, transaction_amount character varying(256) ENCODE lzo, transaction_country character varying(256) ENCODE lzo ) DISTSTYLE AUTO;

--this is a public s3 bucket where the raw data lives. Make sure you register an IAM role to your Redshift cluster and then use that arn id in the below copy from:


COPY public.stage_credit_card_transactions
 FROM 's3://immuta-sa-public/credit_card_transactions.snappy.parq' FORMAT as PARQUET
 IAM_ROLE 'PASTE_YOUR_ARN_HERE' ;

CREATE TABLE public.credit_card_transactions as (select cast(id as bigint) id, customer_last_name , credit_card_number, transaction_location , cast(transaction_time as timestamp) transaction_time, cast(transaction_amount as decimal(18,2)) transaction_amount , transaction_country from  public.stage_credit_card_transactions );

GRANT ALL PRIVILEGES ON TABLE public.credit_card_transactions to "owner@immuta.com";
```

---



Use the SQL Editor to validate that the data was loaded Successfully

```sql

select * from public.credit_card_transactions;

```

Use the SQL Editor to validate that the users were created Successfully

```sql
select * from pg_catalog.pg_user;

```

---

| 1.3 Configure Immuta's ***Native Integration with Redshift*** |
| ------------------------------------------------------------ |

<img src="https://m.media-amazon.com/images/I/71BSNy9PN-L._AC_SX450_.jpg" alt="Amazon.com: Michelangelo: Hands of God and Adam, Detail from The Creation  of Adam, from The Sistine Chapel. Fine Art Print/Poster. Size A2 (59.4cm x  42cm): Posters &amp; Prints" style="zoom:25%;" />
In order for Immuta to Control Access to Redshift, an integration step is required. 

To complete this configuration you will need the Redshift Host, Port and Database. You

1. Log in to Immuta as **Immuta@admin**.

2. Click the **App Settings** icon in the left sidebar (the wrench).

3. Under the Configuration menu on the left, click **Native integrations**.

4. Click the **+ Add Native Integration** button and select **Redshift**.

5. Enter the **Redshift host**.

6. Enter the **Redshift port**.

7. Enter the **initial database**. We can use **demodb**;  (it doesn’t really matter which. Immuta simply needs this because you must include a database when connecting to Redshift.)

8. Enter the Immuta Database: **immuta**. This is the database name where all the secure schemas and views Immuta creates will be stored. 

9. Immuta will automatically install the necessary procedures, functions, and system accounts into your Redshift account.

10. Select **Automatic** and Enter

    1. Enter **Immuta**
    2. Enter 'AW$D3VdAy$'

    

**Immuta Native Integration Recap:**

<img src="/images/gifs/2021-10-15 10.54.41.gif" alt="2021-10-15 10.54.41" style="zoom:50%;" />

---

