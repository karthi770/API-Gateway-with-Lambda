# API Gateway with Lambda, AWS Service and Mock Integrations

# API Gateway with Lambda

## Step 1: Create an SNS Topic

- Create a Topic named “API-messages” and select the type as Standard.

![Fig:1](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled.png)

Fig:1

- Now create a Subscription, Protocol shall be Email and give the e-mail ID. Confirmation mail will be sent to your mail ID.

## Step 2: Create a Lambda function

- Create a Lambda function with the info:

![Fig:2](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%201.png)

Fig:2

- In the function go to code source and enter the code given below:

```python
def lambda_handler(event, context):   
    return {
        'statusCode': 200,
        'headers': {},
        'body': event['requestContext']['identity']['sourceIp'],
        'isBase64Encoded': False
        }
```

## Step 3: API Gateway:

- Select the REST API public:

![Fig 3](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%202.png)

Fig 3

![Fig 4](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%203.png)

Fig 4

- Create a Resource and a method:

![Fig 5](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%204.png)

Fig 5

![Fig 6 Create a method with GET and select ‘Mock’ and save it.](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%205.png)

Fig 6 Create a method with GET and select ‘Mock’ and save it.

![Fig 6 Select the ‘Integration Response’. ](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%206.png)

Fig 6 Select the ‘Integration Response’. 

![Fig 7 In the Integration Response, Select the Mapping Templates and add ‘application/json’. Type the desired message in the template.](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%207.png)

Fig 7 In the Integration Response, Select the Mapping Templates and add ‘application/json’. Type the desired message in the template.

- Create another Resource named ‘Lambda

![Fig 8](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%208.png)

Fig 8

![Fig 9](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%209.png)

Fig 9

# AWS Service Integration

## Step 1: Create an IAM role:

![Fig 10](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2010.png)

Fig 10

![Fig 11 Type API Gateway and select the API gateway.](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2011.png)

Fig 11 Type API Gateway and select the API gateway.

- Select the default permissions shown in the list.
- Give the role name as “api-gw-sns-role” and create the role.

![Fig 12 In order to give additional permissions, select the role and click on in-line policy.](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2012.png)

Fig 12 In order to give additional permissions, select the role and click on in-line policy.

- Select JSON and enter the following code:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sns:Publish",
            "Resource": "*"
        }
    ]
}
```

- Give the policy a name and select Create Policy

## Step 2: Create Resources in the API gateway

- Create a resource named ‘sns’ in the API gateway that we created earlier. Change the type to POST . Enter all the details and select the appropriate options shown in the image below.

![Fig 13](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2013.png)

Fig 13

![Fig 14 The Execution role has the ARN (Amazon Resource Name) of the IAM role that we created. ](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2014.png)

Fig 14 The Execution role has the ARN (Amazon Resource Name) of the IAM role that we created. 

- In the Method Request option, add query strings. One is ‘Message’, other is ‘TopicArn’.

![Fig 15](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2015.png)

Fig 15

- In the integration request, create another query string. In the ‘Mapped from’ option, enter ‘method.request.querystring.Message’. This will give the info about the how the data is mapped between the method request and the integration request. So the same for ‘TopicArn’ - ‘method.request.querystring.TopicArn’

![Fig 16](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2016.png)

Fig 16

- Now deploy the API. Go the Actions dropdown and deploy the API. After there will be an invoke URL that will be generated.

![Fig 17](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2017.png)

Fig 17

## Step 3: Test the API with all the resources that we created.

- With the invoke URL we can access the resources that we want by adding the name of the resources at the end of the URL. For ex: add ‘/mock’ at the end of the URL. Below are the results.

![Fig 18](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2018.png)

Fig 18

![Fig 19 Lambda function is now invoked which will display the IP of the requester in my case its my IP.](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2019.png)

Fig 19 Lambda function is now invoked which will display the IP of the requester in my case its my IP.

- We can test the sns API directly from the API console:

![Fig 20 Click on the Test button to test this API.](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2020.png)

Fig 20 Click on the Test button to test this API.

- On in the Query strings, give the enter the parameters like - “TopicArn=arn:aws:sns:us-east-1:634211996823:API-messages&Message=I+have+mastered+Devops”. Here the TopiArn is the ARN of the SNS topic that we created and the Message is the message that we wish to send. Messages cant have spaces so instead ‘+’ is used.

![Fig 21](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2021.png)

Fig 21

Below message is the Notification received via the SNS to mail ID.

![Fig 22](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2022.png)

Fig 22

# Clearing the AWS

- All the services that were created can be deleted and its straight forward.
- There will be some roles created by the lambda function automatically which should be tracked and deleted.

![Untitled](API%20Gateway%20with%20Lambda,%20AWS%20Service%20and%20Mock%20Inte%206d4454867b7a4455bb48210d42a0c799/Untitled%2023.png)

- Clear the Cloud Watch logs as well.