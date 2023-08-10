# Process Mining : Order-to-Cash Process Discovery and Analysis

Most companies have information systems that record activities of interests, such as the registration of a new customer, the sale of a product, the approval of a purchase system, the processing of a payment system, etc. All of these activities result in one or more events being recorded in some information system. These events are usually used for record-keeping, accounting, auditing, etc.

Process mining is concerned with using these recorded activities in order to understand how an organisation works. Using process mining, actual sequence of tasks (events) that are performed can be automatically discovered, revealing the behaviour of the recorded process execution. It is therefore possible to compare the actual process with the expected behaviour and deviations can be detected. This can lead to identification of process diagnostics and preventive action for potential risks and fraud.

Order-to-cash process is the process starts when a company receiving a customer's order up until those goods are issued and payment is received. The outstanding amount receivable is what is regularly known as trade receivables. The order-to-cash process is recognised as one of the most important processes within a company because it provides core resources for running a business on a daily basis and strongly influences the availability of cash for daily operations.

This notebook will look at how process mining can be used to understand the order-to-cash process of a company. This was done using python and various libraries such as pandas (for analysing the data) and graphviz (for visualizing the discovered process).

The dataset was gotten from https://github.com/IBM/processmining. IBM Github repository for process mining.

The order request (event log) is structured as follows: The dataset comprise of 45825 order request (cases) and 251478 events. After analysing it and eliminating the incomplete order request and those order requests that do not have the appropriate start event, there were a total of 33,196 order requests (cases) https://github.com/nkwachiabel/Process-Mining-Order-to-cash-IBM/blob/main/Order%20to%20cash%20IBM.ipynb. In these cases, there are 196,832 events relating to 18 activities performed by 74 users (72 human users and 2 system automatic job).

A different notebook was then used to analyse this dataset showing the process graph, hand over of work, segregation of duties and variant analysis https://github.com/nkwachiabel/Process-Mining-Order-to-cash-IBM/blob/main/Order%20to%20cash%20IBM.ipynb
