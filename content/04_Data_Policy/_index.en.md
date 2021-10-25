+++
title = "Module 3- Data Policy"
chapter = true
weight = 5
+++

## **Module 3: How Immuta Data Policies can Balance *access* with *privacy* using column masking and row filtering**

   <img src="https://lh6.googleusercontent.com/QgqI3RqIAcAS5nMW5nBkQNI1OFRlKsczrnJ88IlnnQBwflJEXaI8lml-pERVg2q0x9LOM9XMx4boqktxl1CCjVwbhvx5subaDbz21X6MpTRX5M0UbV31tulNCIDzlhS7E4UZnmsyEiM=s0" alt="img" style="zoom:20%;" />

Anne and Manny are now able to access the credit card data. However, *<u>none of the table information</u>* is masked or filtered based on the user's "need to know". 



The CDO wants to know if Owen can create a Data Policy so that Anne is not able to see the Credit Card Number, but Manny can see them.  Also, Analysts have a Country attribute which specifies which countries they can see data for. 

How can Immuta enforce these policies? Let's find out...

In this module you will:

- Login to Immuta as **Owen** and inspect the existing Attribute Names and values

- Use the Attributes to Create a "**Global Data Policy**" that carefullly controls how Manny and Anne see the credit card transaction data.

- Login to Immuta as **Anne** and **Manny** and confirm policy behavior



| 3.0 Login to Immuta as ***Owen*** and inspect the existing Attribute Names and values |
| ------------------------------------------------------------ |

Inspect Manny and Anne's Attributes. Note the ***Country Attribute*** and that  Manny has the values 'US' and 'CA' (for Canada) and Anne has only the value 'US':

| <img src="/images/gifs/image-20211009111444991.png" alt="image-20211009111444991" style="zoom:50%;" /> | <img src="/images/gifs/image-20211009111627710.png" alt="image-20211009111627710" style="zoom:50%;" /> |
| ------------------------------------------------------------ | ------------------------------------------------------------ |

(**Note**: If Manny's Country Attribute does not contain the "US" and "CA", then fix that by editing his Country Attribute List.)



| 3.1 Login to Immuta as **Owen** and create a "**Global Data Policy**" that controls how Manny and Anne see the credit card transactions |
| ------------------------------------------------------------ |

The first Global Data Policy we will create is shown below. 

*It masks all columns Tagged as Credit Card Numbers with Null for everyone except for Managers in the Fraud Department :*

<img src="/images/gifs/image-20211009121347031.png" alt="image-20211009121347031" style="zoom:50%;" />



The second Global Data Policy we will create is shown below. 

*It filters rows for data sources with columns Tagged as Credit Card Numbers by joining the transaction_country column with the User's Country Attribute:*



<img src="/images/gifs/image-20211009122639911.png" alt="image-20211009122639911" style="zoom:50%;" />

The following clips show how Owen created these two policies in the Immuta policy editor:

| Mask-CC-NUMBER<br /><img src="/images/gifs/2021-10-09%2012.33.22.gif" alt="img" style="zoom:50%;" /> |      |
| ------------------------------------------------------------ | ---- |
| **Limit-by-Country**<br /><img src="/images/gifs/2021-10-09%2012.34.48.gif" alt="img" style="zoom:50%;" /> |      |



| 3.2  Login to Immuta as **Anne** and **Manny** and confirm policy behavior |
| ------------------------------------------------------------ |

The following clip shows how Manny and Anne see this table after the above policies are activated:

<img src="/images/gifs/2021-10-09 12.49.48.gif" alt="img" style="zoom:50%;" />

---

# 

**Troubleshooting:**

If the masking or filtering effect does not show up for Manny and Anne, confirm the Data Policy has been applied precisely as follows:

<img src="/images/gifs/image-20211015124613100.png" alt="image-20211015124613100" style="zoom:50%;" />

