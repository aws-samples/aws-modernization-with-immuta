+++
title = "Module 2 - Subscription Policy"
chapter = true
weight = 4
+++

## **Module 2: How Immuta *Global Subscription Policies* control user-access to sensitive data**

<img src="https://m.media-amazon.com/images/I/71KhOOAHBmL._AC_SY550_.jpg" alt="Amazon.com: Pen Sketch Buff Security Bouncer Cartoon Vinyl Decal Sticker  (4&quot; Tall) : Automotive" style="zoom:25%;" />

The Immuta Administrator (ImmutaAdmin@immuta.com) has successfully configured Immuta to connect with Redshift and have setup 3 users (*This was completed in Module 1*):

***Data Owner***, Owen Owner (owner@immuta.com) - Owen has a technology background and reports to the CDO (who reports to the COO). All policy decisions for the use of credit card transaction data is coordinated through Owen.  

***Manager***, Manny Manager(manager@immuta.com) - Manny is a Manager in finance that overseas credit card fraud investigations and must approve any requests to access customer credit card numbers.

***Analyst***, Anne Analyst (analyst@immuta.com) - Anne works on a team of 15 that is focused on designing automated processes and analytical reports that utilize finance, hr, and credit card transaction data. Because all analysts work with all types of data, their default access to sensitive data is minimized. 
 **When additional access is needed, all analysts must escalate through Manny.** 

---



The credit card transactions we will be working with looks as follows:

<u>credit_card_transaction table:</u>

<img src="/images/gifs/image-20211008091626951.png" alt="image-20211008091626951" style="zoom:67%;" />



In our Immuta Instance, all access is blocked unless specifically granted to a user via a "Subscription Policy". 

Can we allow Owen to create a Subscription Policy for Manny and Anne to see the above data? 
Let's find out...

In this module you will:

- Login to Immuta as **ImmutaAdmin** and give Owen permission to create datasources and create access policies

- Login to Immuta as **Owen** and create a "**Global Subscription Policy**" that allows Manny and Anne to have access to the above table

- Login to Immuta as **Anne** and **Manny** and confirm access

**NOTE: For this Lab, Anne and Manny will use the Immuta console to query data, but they could also use their tool of choice and connect directly to Redshift with their Redshift credentials.**

| 2.0 Login to Immuta as ***ImmutaAdmin*** and give Owen permission to create datasources and create access policies |
| ------------------------------------------------------------ |

For **Owen** to create a datasource and policies, he needs the following Permissions:

- Governance
- Create Datasource
- User Admin

Log in to Immuta as **ImmutaAdmin**:

<img src="/images/gifs/2021-10-15 10.36.32.gif" alt="2021-10-15 10.36.32" style="zoom:33%;" />

Granting those Permissions is simple for the ImmutaAdmin to do:
<img src="/images/gifs/2021-10-08%2014.08.07.gif" alt="img" style="zoom:30%;" />




| 2.1 Login to Immuta as **Owen** and create a "***Global Subscription Policy***" that allows Manny and Anne to have access to the above table |
| ------------------------------------------------------------ |

In order for data to be available in the Immuta platform, a Data Owner — the individual or team responsible for the data — needs to connect their data to Immuta. Once data is connected to Immuta, that data is called a data source. 

Data Owners can also decide whether to make their data source public, which makes it available for discovery to all users in the Immuta Web UI, or make it private, so that only the Data Owner and its assigned subscribers know it exists.

Owen must take the following three steps for Anne and Manny to have access to the table:

(1.) Create a Data Source ( an Immuta concept that maps to a table)

(2.) Confirm the Data Source columns are Tagged appropriately and that Manny and Anne have the expected Attributes. (Manny and Anne are both in the "Fraud Department" and have a role attribute of "Analyst" )

(3.) Design a Subscription Policy for "Fraud Analysts" based on the above tags and attributes.

(4.) Create the Policy in Immuta and confirm the desired behavior.



---



These steps are described in detail below:

(1.) Create one or more Datasources
<img src="/images/gifs/2021-10-08%2014.40.20.gif" alt="img" style="zoom:50%;" />
(2.) Confirm Tags and Attributes
![img](/images/gifs/2021-10-08%2014.42.16.gif)
(3.) The following is the type of subscription policy we want to create. <img src="/images/gifs/image-20211008111624506.png" alt="image-20211008111624506" style="zoom:30%;" />It allows users to "subsribe" only if they are in the "Fraud Department" and if they also have a role of "Analyst".  We will test this with the Credit_Card_Transactions table, but it grants access to any datasource that has a column tagged  "Discovered.Entity.Credit Card Number". This is a classification tag automatically added by our sensitive data detection algorithm(SDD) which was automatically ran when the datasource was created by **Owen**.

(4.) The process of creating this Policy is shown here:
<img src="/images/gifs/2021-10-08%2015.14.49.gif" alt="img" style="zoom:33%;" />





| 2.2  Login to Immuta as ***Anne*** and ***Manny*** and confirm access |
| ------------------------------------------------------------ |

Immuta's query tool allows you to preview the data sources available to a user. Given the above subscription policy, Mann and Anne are able to query the credit card transactions table in the public schema as shown below:

<img src="/images/gifs/2021-10-08%2015.44.17.gif" alt="img" style="zoom:50%;" />

---

**Troubleshooting:**

***If the data sources do not show up for Manny and Anne***, confirm the Subscription Policy has been defined precisely as follows:

<img src="/images/gifs/image-20211015124613100.png" alt="image-20211015124613100" style="zoom:50%;" />



***If the data source shows up on the data source tab, but not in the query editor***, confirm the subscription by clicking "Get Access"

<img src="/images/gifs/image-20211015125141952.png" alt="image-20211015125141952" style="zoom:50%;" />

