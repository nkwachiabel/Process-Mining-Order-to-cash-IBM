# Process Mining : Order-to-Cash Process Discovery and Analysis

This repository contains a data analytics project that utilizes process mining methodology to analyse the real-life event log of an order-to-cash process. The dataset was gotten from [IBM Github repository for process mining](https://github.com/IBM/processmining) while studying for IBM Process Mining Business Analyst and IBM Process Mining Data Analyst certifications. It contains order requests created between 2nd January 2016 to 7th December 2017.

### Credits
<b>Publisher</b>: IBM

# Project Overview
The goal of this project is to utilize process mining techniques to:
1. Discover the actual order-to-cash management process.
2. Identify inefficiencies and bottlenecks in the process.
3. Generate insights to improve process efficiency.
4. Develop a dashboard to monitor uncompleted cases

# Methodology
1. Data Collection
2. Data Quality Check
3. Data Transformation
4. Process Discovery
5. Timing Analysis
6. User analysis
7. Suggestions for process improvement

# Technologies Used
1. Python
2. Pandas
3. Graphviz library
4. Jupyter Notebook
5. Microsoft PowerBI

# Data collection
The initial eventlog comprise of 45825 order requests, 19 activities and 251478 events between 2nd January 2016 and 7th December 2017. The <b>Key</b> column was used as the case ID. The <b>Activity</b> column was used as the activities and contained the following 19 activities:
1. Delivery
2. Good Issue
3. Line Creation                       
4. LgstCheckOnConfDat Removed          
5. Header Block Removed                
6. Sched.Line Changed Delivery Date    
7. Document released for credit         
8. Schedule Line Rejected               
9. Address missing Block Set            
10. Address missing Block Removed        
11. LgstCheckOnConfDat Set               
12. Sched.Line Block Removed              
13. CTR Block Removed                     
14. Document blocked for credit           
15. Header Block Set                      
16. Sched.Line Block Set                  
17. Special test Block Set                
18. Special test Block Removed            
19. CTR Block Set 

The dataset contains other attributes such as the User who carried out the activity, the role of the user, the user type (Human or Robot) the product hierarchy being ordered, net value of the order, customer who made the order, type of order made, an indicator to show if the order was delayed or in time, etc.

# Data quality check and transformation
The event log was reviewed to ensure the data contained were okay to use for process mining. The following was discovered:
1. Python did not automatically identify the date column. This was adjusted.
2. On initial check, the event log contained 15 different start activities and 14 different end activities. I believe this was because of the way the eventlog was extracted. The eventlog captured all the order request from the 2nd January 2016 to 7th December 2017 irrespective of where the order request was in the process. To carry out an end to end process analysis, we filtered the event log for completed order request. These were order requests which started from <b>Line creation</b> and ends with either <b>Goods Issue</b> or <b>Schedule Line Rejected</b>. After this, we were left with 21,159 orders, 17 activities and 110,125 events.
3. Some columns had empty values. This was because certain activities require input in those columns while some do not. For example, if an order was rejected (i.e., <b>Schedule Line Rejected</b> activity), the User_Type column is empty. Other columns which had empty rows were not used in this analysis.
4. Since the event log was gotten online, no process owner was contacted to gain further understanding of the process and other columns in the dataset. Therefore, if there are manual events in this process, it is not highlighted here.

# Output and Visualisations
## Data overview
![alt text]()

This page gives an overview of the dataset. It shows the number of orders, activities and events. It also highlights the net value of these order requests and number of process variants. The clustered column chart at the top right shows the number of orders per year and month. We can see that there were many orders in January 2016 and these orders reduced a lot from July 2017. It is also important to mention here that the TLC Connectivity product started in January 2017.

The pie chart shows the number of orders by product hierarchy. TLC Optical Cables was the most ordered, followed by TLC Optical Fibres. TLC Ground Cables and TLC Connectivity have not been ordered that much. While we have product hierarchy, these products are also broken down into order types. The US-Std. Order had the highest cost of order, followed by US-IC Order.

Regarding activities, the Activities by user types table shows the various activities and broken down into Human/Robot. The activity <b>Schedule Line Rejected</b> has no user type. This is because when an order is rejected, the User_Type column is empty. While the net value of the 21,159 orders are 241.83m, they contain 2,488 rejected orders amounting to $62.57m with more rejects coming from TLC Optical Cables and no reject in TLC Connectivity.

## Process discovery based on event log data
![alt text]()

This page helps in analysing the process. It contains various filters including product hierarchy, order type and customer.
The gain a proper understanding of the process, the order was broken down into the 4 distinct product hierarchy. In understanding the process, 3 different analysis was carried out; (i) variant analysis, (ii) process flow and (iii) transition matrix.
* <b>Variant analysis</b>: The variant analysis starts by identifying the trace of activities in sequential order that each order request follows. After this, similar traces were grouped into process variants. This analysis, broken down into the various product hierarchy shows the following:
  * TLC Optical Cables with 12,250 orders has 726 variants. Variant 1 accounts for 38.51% of the orders and follows the following sequence Line creation -> Header Block Removed -> LgstCheckOnConfDat Removed -> Delivery -> Good Issue.
  * TLC Optical Fibres with 8,775 orders has 68 variants. Variant 1 accounts for 71% of the orders and follows the following sequence Line creation -> LgstCheckOnConfDat Removed -> Delivery -> Good Issue. (notice that the Header Block Removed activity is not included here)
  * TLC Optical Ground cables with 49 orders has 15 variants. The top 3 variants account for 55.1% of the orders and are all rejected cases. Variant 4 represents the highest variant with completed cases and follows the following sequence Line creation ->  Header Block Removed -> Address missing block set -> LgstCheckOnConfDat Set -> Sched.Line Block Removed -> Address missing block removed -> LgstCheckOnConfDat Removed -> Delivery -> Good Issue
  * TLC Connectivity with 85 orders has 17 variants. The first two variants accounts for 72.94% of the cases. The first variant is associated with Customer 1.
  
* <b>Process flow visualisation</b>: The process flow graph shows the process flow from the start to finish for the filtered product hierarchy. The process starts from Line creation and either ends with Goods Issue or if the order is rejected, Schedule Line Rejected. Looking at the process flow, it appears that when a line is created, some blocks are set automatically for some customers. They include Header block, LgstCheckOnConfDat, and Credit block. These blocks are removed by the various users in the system.

* <b>Transition matrix</b>: This shows how events transition from one activity to another. The row shows the start activity while the columns shows the preceeding activities. The following were noted:

  - <b>Repeated actvities:</b> There are some activities that are done repeatedly.

| No. | Activity | Occurrence | Product hierarchy | No. of customers | No. of orders |
| :--- | :--- | :---: | :--- | :---: | :---: |
| 1 | Document released for credit | 11 |TLC Optical Cables | 7 | 11 |
| 2 | Address mising block removed | 5 | TLC Optical Cables | 1 | 1 |
| 3 | CTR Block Removed | 58 | TLC Optical Cables | 5 | 14 |
| 4 | Sched.Line Changed Delivery Date | 476 | TLC Optical Cables | 48 | 379 |

From the above, these activities are all repeated on the TLC Optical Cables product. Shows that there are potential areas for improvement

  - <b>Goods Issue</b> 
    * Goods were issued despite Address missing block was set 3x. This occured in 3 orders with 3 different customers. Looking at the process, these orders
    * Goods were issued even when the Document blocked for credit activity was set. This occured in 3 different orders from 1 customer (Customer 206). For these 3 cases, the document was released for credit by User51 (Customer service rep) initially in May, but was later blocked for credit in December of the same year by User33 (Master Scheduler) and the Goods were still issued in December by User66 (Customer service rep). This indicates a lack of control in the system.

  - <b>Delivery</b> There were 5 cases where Delivery activity was done even immediately after the Address missing block was set. A further look into these cases showed that the Address missing block was removed before the Good Issue activity was done. This can be because the Delivery activity was an automatic activity.

* <b>Rejected order</b>: The product with the highest number of rejected order is TLC Optical Ground Cables with 57% of the orders rejected. The reason for the rejections were not stated for root cause analysis, while TLC Connectivity has no rejected order. One notable thing about the rejected order is that none of the orders were rejected after delivery was made.

* Some events are carried out by robots in the process. Asides Delivery and Good Issue, robot can create line, remove header block and set LgstCheckOnConfDat in TLC Optical Cables hierarchy



The Graphviz library was used to automatically generate a visual process model based on the event log data. 
1. <b>Variant analysis</b>: This variant analysis shows how frequently a particular process is followed. There was a total 193 variants with the top 5 variants acconting for 93% of the cases.
2. <b>Events chart</b>: This shows when the events occur by the time of the week. It shows that most fines are created either on a Friday or Saturday (TGIF😄), payments were made on either a Monday or Tuesday and fines are sent for credit collection mostly on Tuesday or Sunday.
3. <b>Process graph</b>: The process graph shows how the activity flows from the start of a procurement process to the end. From the process graph, all cases starts with the Creation of a fine. For Variant 2 cases, the offender pays the fine immediately it is created. Variant 2 accounts for approximately 36% of all cases. This maybe because for cases in Variants 2, these fines are paid immediately they are given. For other variants, after the case is created, it is sent to the offender and they are either paid or sent for credit collection.
4. <b>Events transition matrix</b>: The transition matrix shows how the events are been handed over from one to another and how frequently this occurs.
   
### Process deviation
* <b>Dismissal variables used</b>: The dismissal variable based on research to be used are either NIL (when the fine is created and should bot be changed unless there is a successful appeal), G (when there is a successful appeal and the case is dismissed by the judge), and # (when there is a successful appeal and the case is dismissed by the prefect). However, there were other dismissal variable that were found in the event log. They are detailed below:
  * <b>Dismissal variable 4</b>: This dismissal variable appears in only 2 cases and used in one article (142). Used by 2 users (User 53 and User 11) for fines created in August and December 2004 and occurs in 2 variants (Variant 3 and Variant 4)
  * <b>Dismissal variable @</b>: This dismissal variable appears in 9 cases and two variants (Variant 1 and variant 8), used by 7 users (User 537, 550, 35, 8, 29, 31, and 536). All fines were created between January 3 and June 3 2000. Article 7, 157 & 158.
  * <b>Dismissal variable D</b>: Used once for the case created on 2 June 2011 by User 848. Appears in one case. Aritcle 158.
  * <b>Dismissal variable Z</b>: Used once for the case created on June 16 2000 by user 811.
* <b>Create fine dismissal variable</b>: When a fine is created, the dismissal value is NIL. While the NIL variable is used for almost all cases in this activity, the dismissal variable 4, was used 2x, @ was used 9x, D and Z were used 1x.
* <b>Successful dismissal</b>: As highlighted above, there are some cases where the successful dismissal variable is used (G and #) but those cases do not end with Send appeal to judge or prefecture.
* <b>Receive result appeal from prefecture</b>: This activity is usually done when the appeal to the prefecture was not successful and should have a NIL dismissal variable. Below, there are 24 cases where a # variable was used instead.
* <b>Add penalty to Send for credit collection</b>: For all cases which went transitioned from <i>Add penalty</i> to <i>Send for credit collection</i>, the least duration was 273 days; significantly higher than the expected 180 days. This shows either a lack of monitoring system or the prefects do not monitor the fines if they have been paid or not for onward forwarding to the credit agencies.
* There were two cases (S66168 and N35881) where the fines were fully paid, but it was still sent to the credit agency for collection. Both fines were paid in 2002, but they were sent to the credit collection on 10 January 2004. That's about 2 years later.
### Other findings
* There are some users that carry out particular events in batches. For example, users 538, 550 and 536 performed the Send fine activity for 259, 195 and 176 cases respectively on 19 May 2003. Also, the three users performed the Insert fine notification activity 100, 69 and 65 times respectively on 25 May 2003.

## Timing analysis
![alt text](https://github.com/nkwachiabel/Process-Mining-Road-Traffic-Fine-Management/blob/main/Images/Timing%20analysis.jpg?raw=true)

This analysis was done to confirm if there are deviations regarding the time when the events should be performed. Some performance indicators were developed.
1. From research, each fine created should be completed within six months; they should either be paid, sent for credit collection or dismissed. The first test that was done was relating to the duration of each case. It indicated that out of 128,268 cases, 75,259 (59% of cases) were completed after 180 days.
2. Looking at the individual cases, the send for credit collection event is where more time was spent. It takes an average of 525 days before a fine is sent for credit collection. This is way too long. One reason for this can be that the person who raised the fine is not monitoring the fines to adhere to the time limit.
3. One thing to note is that the Add penalty activity is always followed. Looks like an automatic event that happens when it is up to 60 days since the fine was created but payment has not been made.
4. There are 37,266 cases where the fine was sent after 90 days. Out of these fines which were delayed, 70% of them were sent to the credit collection agency. This might be because the offenders are already aware that these fines were sent out late and there would be no repercussion for not paying. The traffic prefects should be trained/reminded of this rule to avoid offenders getting away with fines. This also incurs cost to the management by paying for postal expenses which would not be recovered.
5. There are 53,309 fines which have not been fully paid, but have not been sent to the credit collection agency.
6. The 60 days time limit for appeals are not been adhered to by the offenders. Of the fines appealed to the judge, 36% of them were done after 60 days, while for those appealed to the prefect, 81% of them were done after 60 days. We are not sure if this time indicated when the actual appeal was made or when the prefect recorded this in the system. Either way, this shows a deviation from the expected process.

## Case details
![alt text](https://github.com/nkwachiabel/Process-Mining-Road-Traffic-Fine-Management/blob/main/Images/Case%20details%20page.jpg?raw=true)

This dashboard shows information relating to a particular case by using the filter at the top left of the screen.

# Process improvement
Based on the analysis, areas for improvement were identified such as:
* Process redesign: One of the areas of improvement can be understanding who does what activity. From the event log, it appears that the prefect does all the activities. This can be misleading as there can be human error or delay in inputing this event into the system. Activities such as Appeal to judge and appeal to prefecture should be done by the offender and the time this is done should be indicated.
* Automating send to credit agency: The send to credit agency can be automated if certain conditions are met. For example, when a fine is creates, sent, no appeal and no payment made after a certain number of days. It makes the process easier and can help in early collection of fines by the collection agents. This will be relevant especially considering the fact that people move houses frequently and leaving it too long can lead to loss of fines.
* Avoid delaying sending fines: A lot of fines were not sent within the 90 days fime frame. This is because while the prefect is outside on their duty post, they might not have the time to carry out other activities. I would suggest that there should be a segregation of duties between the prefects who create the fine and the person who sends the fine to the offender to avoid this delay. 
* Improved communication: There were 3,889 times where a penalty was added after a payment was made and two cases where payment was made but still sent to the credit agencies. I think this either is as a result of delay in communication between the payment receiver and the prefect, or the prefect do not update the case with the payment on time to avoid triggering a penalty.
* Real-time dashboard: A real-time dashboard should be made available for each prefect to monitor those fines issued by the prefect. This dashboard should include key metrics to track their fines and avoid deviating from the expected process. This would help in reducing processing times and improved overall process efficiency.

# Limitation
No process owner was reached out to confirm the validity of the expected process

# Repository structure
* 'Data/': Contains the data used for analysis
* 'Notebook/': Jupyter notebook detailing the data cleaning, process discovery and analysis
* 'Output/': Includes the PowerBI output and a PDF file

# Contributions
Contributions to this repository are welcome! If you encounter any issues or have suggestions for improvement, please open an issue or submit a pull request.

# Contact
For any questions or inquiries, please contact nkwachiabel@gmail.com
