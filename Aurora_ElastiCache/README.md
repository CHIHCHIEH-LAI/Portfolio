# Aurora and ElastiCache

## Overview
This MP explores AWS RDS, ElastiCache, S3 and how using ElastiCache can boost RDS performance. Specifically, you will build a storage service to support read and write queries on data stored in a relational database. The read and write APIs are built using Lambda functions. Your goal is to demonstrate the performance benefits of using caching to improve the performance of database queries.
![MP6_Aurora_ElastiCache/images/diagram.jpg](https://github.com/CHIHCHIEH-LAI/Project-Portfolio-Collection/blob/main/Aurora_ElastiCache/images/diagram.jpg)

## Populate a database and set up a Redis cluster
### Amazon S3 setup
First, we create an S3 bucket and upload the content from the following file to populate the database.

Step 1: Set up an Amazon S3 bucket and upload mp6input.csv into the bucket. 
You can reference this [tutorial](https://aws.amazon.com/s3/getting-started/?nc=sn&loc=6&dn=1) to get started with Amazon S3.

File containing the data: [mp6input.csv](MP6_Aurora_ElastiCache/mp6input.csv)

### Setup and populate Amazon Aurora
Step 2: Create an IAM policy and IAM role for Amazon Aurora to access Amazon S3
Next, create an IAM policy and an IAM role for the Amazon Aurora database.

Note: Refer to the following tutorials for reference to learn more about IAM policies and roles.
https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.S3CreatePolicy.html  

https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.CreateRole.htm

Step 3: Setup Amazon Aurora database
In this step, you will create an Amazon Aurora database. Reference the two tutorials listed below, and use the following setting: Make sure you use standard create and use "DB instance class": Burstable classes (includes t classes). This configuration is to ensure you use the small database to avoid large charges ($$$). Ensure "publicly accessible" is selected. Create a custom DB cluster parameter group and assign the IAM role you created to the parameters that allow Aurora to access S3. Attach the IAM role created in step 2 to the database just created.  

https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_GettingStartedAurora.CreatingConnecting.Aurora.html

https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Authorizing.IAM.AddRoleToDBCluster.html

Step 4: Populate database using data in Amazon S3
Next, load the data stored in the S3 bucket (created in step 1) into the database. Reference the tutorial:

https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.LoadFromS3.htm

To access aurora, you can use MySQL from an EC2 instance described in this tutorial. 

https://aws.amazon.com/getting-started/hands-on/boosting-mysql-database-performance-with-amazon-elasticache-for-redis/

First, create an EC2 instance and ssh into that instance. Then, type the following command:

mysql -h rds_writer_endpoint -P 3306 -u admin -p

Before loading the csv file into Aurora, you should create a database and a table. The table should have the following fields: id, hero, power, name, xp, color. Don't change these field names as the autograder is hardcoded with these names. 

For basic MySQL operations, this tutorial should help.

https://dev.mysql.com/doc/mysql-getting-started/en/
```
mysql> CREATE DATABASE CCAMP6;

mysql> USE CCAMP6;
Database changed

mysql> SHOW TABLES;

mysql> CREATE TABLE roles;

mysql> LOAD DATA FROM S3 's3://mp6storagebucket/mp6input.csv' INTO TABLE roles_temp FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' IGNORE 1 ROWS (id, hero, power, name, xp, color);

mysql> DESCRIBE roles;

mysql> DROP TABLE roles;
```

3.3 Create a Redis Cluster
To create a Redis cluster, you can refer to the following tutorial:
https://aws.amazon.com/getting-started/hands-on/boosting-mysql-database-performance-with-amazon-elasticache-for-redis/module-one/

Unfortunately, AWS Educate accounts cannot launch Redis. You need to use a personal account to do that.  To save money, you should scale down the instance for both RDS and ElastiCache. 

### Create a Lambda function 
4.1 Setting up Lambda function
You can refer to MP2 for creating Lambda functions. Unlike some previous MPs, you can import third-party libraries in this MP. If you choose to use Python for this MP, you will need to refer to the following tutorial for importing two modules pymysql and redis:

https://docs.aws.amazon.com/lambda/latest/dg/python-package.html

If you decide to use other languages, please refer to the AWS official documentation to figure out how to import third-party libraries:   https://docs.aws.amazon.com/lambda/latest/dg/welcome.html.

You can refer to this template when writing your Lambda function.

https://github.com/UIUC-CS498-Cloud/MP6_PublicFiles/blob/main/mp6_template.py

The following example might also help you.

https://github.com/aws-samples/amazon-elasticache-samples/blob/master/database-caching/example.py

Here is the template file.

[mp6_template.py](https://github.com/UIUC-CS498-Cloud/MP6_PublicFiles/blob/main/mp6_template.py)

4.2 Write/Read API
There are different strategies for using caches. Here, we utilize write-through and lazy-loading strategies. You can refer to this document for more details:

https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html. 

You must insert new superhero characters in the Aurora database for write requests. An example of a write request API call is as follows:
```
{
  "USE_CACHE": "True",
  "REQUEST": "write",
  "SQLS": [
    {
      "hero": "yes",
      "name": "fireman",
      "power": "fire",
      "color": "red",
      "xp": "10"
    },
    {
      "hero": "no",
      "name": "dogman",
      "power": "bark",
      "color": "brown",
      "xp": "50"
    }
  ]
}
```

Insert the data according to the order of the "SQLS" parameter. If USE_CACHE is True, you should use a write-through strategy. Our autograder will check whether your write-through implementation improves the read performance. For write API calls, the result should be "write success".

The autograder will send read requests in the following format:
```
{
  "USE_CACHE": "True",
  "REQUEST": "read",
  "SQLS": [1,2,3]
}
```

To prepare the response to a read API call, you should create a list and append the corresponding rows from the database, with the ids in the SQLS list selecting the rows into it. Your Lambda function will return this list. If USE_CACHE is true, check if Redis already contains the id. If not, read the row from RDS, store it in Redis, and return the result. Note that the choice of key and value in Redis is totally up to you. The autograder will only ask you for the corresponding rows of the ids. Make sure your response is in JSON format. 

At the end of your lambda handler function, the return statement should be as follows:
```
return {
    "statusCode": 200,
    "body": result
}
```

the result format is as follows: each row of data is represented as a dictionary. You should put all the result rows in a list; therefore, you will need to return a list of dictionaries as string format. Finally, the result should be in JSON format. An example is shown below. 
```
return {
    "statusCode": 200,
    "body": [
        {
            "id": 1,
            "hero": "yes",
            "power": "fly",
            "name": "batman",
            "xp": 100,
            "color": "black"
        },
        ...
    ]
}
```

If you get an "unable to jsonify response or no 'body' key in json" error response from the autograder, you can try replicating the problem using the following code. Copy this code in a new file, replace "api" with your POST method URL and run it. 
```
payload = {
        "USE_CACHE": "True",
        "REQUEST": "read",
        "SQLS": [1,2,3]
    }
api = "your dbApi"
r = requests.post(api, data=json.dumps(payload))
res = ""
res = r.json()['body']
```

Our lazy loading test will check if your implementation takes advantage of Redis and boosts the performance of querying data. You can test this on your own before submitting your solution.

You can refer to https://aws.amazon.com/getting-started/hands-on/boosting-mysql-database-performance-with-amazon-elasticache-for-redis/module-four/ to get started with the implementation. Refer to

https://pymysql.readthedocs.io/en/latest/

https://redis-py.readthedocs.io/en/latest/

for documentation of pymysql and redis.

### API setup
Set up an API endpoint for your Lambda function. Refer to MP2 for more details.

### Notes
1. Make sure you ```DELETE FROM herostb WHERE  id >= 26``` every time you make a submission.
2. The communication overhead sometimes outweighs the performance boost. If you are sure your implementation of write-through strategy is correct but the test4 fails, or autograder reports no performance boost, reboot your Redis and submit again.

