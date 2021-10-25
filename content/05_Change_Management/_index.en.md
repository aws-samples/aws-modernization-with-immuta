+++
title = "Module 4 - Change Management"
chapter = true
weight = 6
+++

## **Module 4: How Immuta makes Exceptions,  Reporting, and Compliance easy**
<img src="https://thumbs.dreamstime.com/b/calculator-statistics-judge-s-hammer-magnifying-glass-calculator-statistic-judge-s-hammer-magnifying-glass-table-216703507.jpg" alt="892 Judge Magnifying Glass Photos - Free &amp; Royalty-Free Stock Photos from  Dreamstime" style="zoom:25%;" />

 **Owen**,  owner@immuta.com has successfully protected access to credit card transactions utilizing **Masking** and **Row Filtering**. 

In this final module, we explore how Immuta streamlines work when there are Exceptions, Reporting, and Auditability requirements. 

ABC Corp needs to be able to clearly understand how data sources, user attributes, and policies change over time. Also, since Anne will occasionally need to investigate a specific transaction, we will configure immuta to make the credit card number masked, but reversible by Manny.  



Does Immuta support all of these scenarios?  Let's find out...

In this module you will:

- Login to Immuta as **Owen** and run a report on who is subscribed to the credit_card_transaction table

- While logged in as **Owen**, view the Audit log and see when the user Anne last queried credit_card_transactions.

- While logged in as **Owen** modify the "**Mask-cc-number Global Data Policy**" so that masking can be reversible. Login as **Anne** and submit an ***unmask request*** that a particular value be unMasked by **Owen**.

- Add a column to the table and view how this is masked by default.



| 4.0 Login to Immuta as **Owen** and run a report on who is subscribed to the credit_card_transaction table |
| ------------------------------------------------------------ |

Immuta Reports allow Data Governors to use a natural language builder to instantly create reports that detail user activity across Immuta.

Click **select entity** and choose the option you would like the report based on from the dropdown menu. Next choose **Data Source** and select the 'Public Credit Card Transactions' data source.

<img src="https://documentation.immuta.com/2021.3/4-connecting-data/managing-data-sources/user-guides/img/reports-select-entity.png" alt="Select Entity" style="zoom:50%;" />



The Report shows the users who are currently subscribed to the data source.<img src="/images/gifs/image-20211015140914822.png" alt="image-20211015140914822" style="zoom:50%;" />


| 4.1 While logged in as **Owen**, view the Audit log and see when the user Anne last queried credit_card_transactions. |
| ------------------------------------------------------------ |

Immuta provides access to all of the audit logs via the Audit page.

<img src="/images/gifs/image-20211015141142155.png" alt="image-20211015141142155" style="zoom:50%;" />

If we use the filters, we can find the set of records that represent the activity of Manny (manager@immuta.com) against the Public Credit Card Transactions Data Source. 

Are there any Failures?

| Successful Activity                                          | Failed Activities                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="/images/gifs/image-20211015141527522.png" alt="image-20211015141527522" style="zoom:25%;" /> | <img src="/images/gifs/image-20211015141601016.png" alt="image-20211015141601016" style="zoom:25%;" /> |

Click on the record and you will see the full details:

<img src="/images/gifs/image-20211015141711964.png" alt="image-20211015141711964" style="zoom:25%;" />

| 4.2 While logged in as **Owen** modify the "**Mask-cc-number Global Data Policy**" so that masking can be reversible. Login as **Anne** and submit an ***unmask request*** that a particular value be unMasked by **Manny**. |
| ------------------------------------------------------------ |

1. Edit the Mask-CC-Number Policy:

   <img src="/images/gifs/image-20211015141857740.png" alt="image-20211015141857740" style="zoom:33%;" />


   Change the Masking Type to "with reversibility"
   <img src="/images/gifs/2021-10-15 14.20.44.gif" alt="2021-10-15 14.20.44" style="zoom:50%;" />

2. Log in as Anne and find a masked value and request that it be unmasked by Manny. Log in as Manny and view the request and the task on the data source. Click the eye icon.
   <img src="/images/gifs/2021-10-15 14.54.51.gif" alt="2021-10-15 14.54.51" style="zoom:33%;" />




| 4.3  Add a column to the table and view how this is masked by default. |
| ------------------------------------------------------------ |

1. Using the Amazon Redshift Query Editor, Alter the credit card table and add a new column:
   ( Use the following Alter Table statement)

   ```sql
   alter table public.credit_card_transactions add column New_column varchar default 'secret'
   ```
   <img src="/images/gifs/2021-10-15 15.10.23.gif" alt="2021-10-15 15.10.23" style="zoom:50%;" />


2. As **Owen** run "Column Detection " on the Credit Card Transaction Data Source. Confirm that the new columns "secret data" is automatically masked by Immuta by running a query as **Anne**.  

   <img src="/images/gifs/2021-10-15 15.18.53.gif" alt="2021-10-15 15.18.53" style="zoom:50%;" />

3. **Owen** removes the New Tag from that new column, how does that affect Anne's Query?

   

   <img src="/images/gifs/image-20211015152935973.png" alt="image-20211015152935973" style="zoom:33%;" />





