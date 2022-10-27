
# Data Visualization using Redash on the Cloud

## Free and simple DataViz Tool

![](https://miro.medium.com/max/700/1*RbUJhqA26hy59LPTTA86Hw.png)

Data visualization is a very important step to better understand the data and monitor a company’s activity.

> **Redash**  is a very powerful Data Viz tool to  **connect**  to any data source (PostgreSQL, MySQL, Redshift, BigQuery, MongoDB, Databricks, and many others),  **query**,  **visualize**  and  **share**  your data to make your company data-driven —  [Redash](https://redash.io/).

It is the official DataViz tool integrated inside  [Databricks](https://www.databricks.com/blog/2020/06/24/welcoming-redash-to-databricks.html), the unified platform for data science, data engineering, data analyst, ML engineer

![](https://miro.medium.com/max/543/1*1POd7Ss2bgpdSNEoCi4f4w.png)

Running Redash for the first time may require some experience, depending on where you would like to deploy it.

In this beginner-friendly article, I will cover step-by-step how two deploy it on AWS Cloud.

# Supported Data Source:

-   Amazon Athena,
-   Amazon CloudWatch
-   Amazon Redshift
-   Axibase Time Series Database
-   BigQuery
-   CSV Files
-   CSV Files (Deprecated)
-   Databricks
-   DynamoDB
-   Elasticsearch
-   Google Analytics
-   Google Sheets
-   JIRA
-   JSON API’s
-   Microsoft Azure SQL Database & Synapse
-   MongoDB
-   Python

# 5 Simple Steps

Using Redash is very simple, here are the 5 steps in order to achieve a stunning result for DataViz

-   Add a data source
-   Write a query
-   Add a visualization
-   Create a Dashboard
-   Invite colleagues

More details can be found  [here](https://redash.io/help/user-guide/getting-started#5-Invite-Colleagues).

Redash uses technologies such as  `postgres`  ,  `Redis`  and  `NGIX`.

# Setup Redash on the AWS Cloud

# Create an EC2 instance

Head to the  [setup page](https://redash.io/help/open-source/setup#-AWS)  and click on one AMI link on the left column of the table as shown in the image below

![](https://miro.medium.com/max/700/1*0jVCuSjev5q1MffYw9o_2A.png)

-   In my case, I chose the one inside the purple rectangle ([AWS EC2 EU-West-3](https://eu-west-3.console.aws.amazon.com/ec2/home?region=eu-west-3#LaunchInstances:ami=ami-041bf9180061ce7ea))

Once on the AWS EC2 webpage, you must sign in, then, you will be walked through different configurations to set up the instance.

-   An ubuntu image with Redash installed is preselected from the catalog

![](https://miro.medium.com/max/700/1*EkVqD6r6VW2BpaoqsmuSAw.png)

-   For the  **instance type**, make sure you select at least  `t2.small`  as shown in the image below

![](https://miro.medium.com/max/700/1*PxzuCqdsr-EUBYYLvBleKg.png)

-   For the  **Network setting**, here is my configuration, of course, you can create a new VPC and Security Group or just reuse existing ones.

![](https://miro.medium.com/max/700/1*UwI2yhfdzBQvprXOk1Lssw.png)

_Note_: Make sure  `Auto-assign public IP`  is enabled

-   The rest of the steps are pretty straightforward, so you should be able to follow along
-   In the end, you should validate the instance summary and proceed to the creation of the instance

_Note_: Do not forget to keep the key pair file you created to connect later to the instance locally from an SSH client

# Connect to AWS EC2 instance

After the creation of the instance, comes the time to connect to it in order to interact with the machine and launch the Redash dashboard. I chose to connect via SSH client (e.g. OpenSSH in my Ubuntu). Head over to the  `connect to instance`  then  `SSH client`  to see the steps to follow to establish a connection

![](https://miro.medium.com/max/700/1*GqP73sWtqG7RKohf-qWABg.png)

-   The details from the screenshot above are straightforward. From the terminal (Ubuntu in my case), run the following commands, and make sure the user name is  `ubuntu`  (and not  `root`)

chmod 400 Redash_key_pair.pem     
ssh -v -i "Redash_key_pair.pem" [ubuntu@ec2-xx-xxx-xxx-xxx.eu-west-3.compute.amazonaws.com](mailto:ubuntu@ec2-xx-xxx-xxx-xxx.eu-west-3.compute.amazonaws.com)

-   After login, you should get a similar log

![](https://miro.medium.com/max/700/1*13KLFfts9bRJ3RTQqbINDw.png)

# Access Redash UI

-   First, note the Public IP address as shown in the image below

![](https://miro.medium.com/max/608/1*Y9TfeS9g8qJdWeqE08lDmg.png)

-   From here, you just have to open a browser on your local machine and insert the public IP address (`Xx.xx.xxx.xxx`) of the AWS EC2 instance followed by port 5000 (i.e,  `http://Xx.xx.xxx.xxx:5000/`). If everything is correct, you should get a similar result

![](https://miro.medium.com/max/700/1*uQfQ2aiaFW9LmWv-fr8Iqw.png)

-   After filling out the form and clicking on  `Setup`  you should land on the home page 🚀🚀🚀

![](https://miro.medium.com/max/700/1*X9Cwt9Q3z7ZW01UUGEsxsw.png)

🥳️  **Congratulations**  if you made it this far 🥳️

-   Now that Redash is up and running, you just need to follow the steps highlighted by the purple rectangle in the previous image, connect a data source, create a query, create a dashboard, and Voila!

# Dashboard Example

Now it is time to  [get creative](https://redash.io/help/user-guide/visualizations/visualizations-how-to), here is an example I made using structured numerical data

![](https://miro.medium.com/max/700/1*RbUJhqA26hy59LPTTA86Hw.png)

# Conclusion

Data visualization is a very important step to better understand the data and monitor a company’s activity. Every organization operating in the data space should have one.

Rethdash is a very interesting solution that allows the user to connect to the data using different connectors, create interactive visualizations, and set up alerts, all that for FREE 🤯🤯🤯

When you think about it, you realized that you do not necessarily have to pay for a Microsoft Power BI license to achieve a similar result, I have used both of them.

If you are interested in contributing to Redash, find the project on Github:  [https://github.com/getredash](https://github.com/getredash)

Hopefully this was helpful.

Peace✌️