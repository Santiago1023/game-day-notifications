## DevOps Challenge: Day 2 (Game Day Notification)
Challenge: Create a game notification app.


### Project Overview
Create a system that will be able to send us notifications about NBA games and stats for sms or email, using an event driven architect.

**Tools:**
- NBA Game API
- Cloud services (AWS SNS, AWS Lambda, Amazon EvenBrigde)
- Python

**Prerequisites:**
- AWS account
- Python installed
- AWS CLI installed
- NBA API key
- Visual Studio Code installed

**Basic concept of the aws services used:**
- [AWS SNS](https://docs.aws.amazon.com/sns/ "AWS SNS") works as pub/sub service, let me explain that with an example:
You want to buy a shoes in your favorite online store, when you enter to the webpage, you see there are not shoes available in stock, and you do not deal with review the webpage every day until the stock will be available. So, how can the store notify you when the shoes are available?
This is possible with a pub/sub solution, the publisher always notifies to subscribers each time that has new information. Then, when the shoes are in stock, the publisher knows that, and sends a notification to all subscribers.
- With [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/ "Amazon EventBridge") we can setup cron jobs called as Scheduled Rules, but what is that? Well, a cron jobs is a task that it is executed for a period of time that you set up, it can repeated as you want, for example: every day at 8:00 am for 3 months, or every 2 hours since 1pm to 9pm, or whatever you need.
- With [AWS Lambda](https://docs.aws.amazon.com/lambda/ "AWS Lambda") service we can run our own code. In this case, the lambda function is used to make a request to the NBA API to obtain game days stats. However, the data come in a format that the general user does not understand, but we can handle this, transforming the data in more human readable message.
Also, we can attach to the lambda function a scheduled rule created with Amazon EventBridge, in order to execute the function each time we need.

### Steps

1. Create an AWS account [(create free account)](https://aws.amazon.com/es/free/?nc1=h_ls&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all "(create free account)")
2. Clone the project (Note: this project is based on the original)
`https://github.com/Santiago1023/game-day-notifications`
3. Create an API KEY ([sportsdata.io](https://sportsdata.io/ "here"))
4. Sigin in the aws console, and type SNS:
![sns1](/images/sns1.png)
5. Click in topics, create new topic, select standard type, give gd_topic name, scroll down and click in create topic button
![sns2](/images/sns2.png)
6. Well, we have the topic, but we need a subscriber to send the information.
Click in the create subscription button, select email protocol, and enter an email, then click create subscription button, check your email and accept the subscription
![sns3](/images/sns3.png)
Now we need to create a rule to gives lambda function permissions to be able to do certain things, in this case be able to publish to the SNS topic
7. Type IAM
![iam1](/images/iam1.png)
8. click in policies, create a policy, in the service searchbar type sns, and click in the json button
![iam2](/images/iam2.png)
9. Go to the repo, and copy the gd_sns_policy.json and paste in the console, change the Resource for your own sns arn, and click next button
![iam3](/images/iam3.png)
this policy means: ''Hey, we want to allow whoever this policy is attach to, to publish information in this specific resource (gd-topic)"
10. give a policy name and click in create policy button
![iam4](/images/iam4.png)
Now we need a role to attach to the lambda function, role gives temporary permissions to able to do certain things, in that role will be the policy that already we have created.
11. click in roles, create role, choose aws service, and type lambda, click in the next button
![role1](/images/role1.png)
12. select the gd policy that we have created, and the AWSLambdaBasicExecutionRole, and click in next button
![role2](/images/role2.png)
13. give a role name, scroll down and click in the create role button (be secure of add the 2 policies)
![role3](/images/role3.png)
14. it is time to create our lambda function, type lambda
![lambda1](/images/lambda1.png)
15. click in create a function, select author from scratch, give a function name, choose python 3.13, choose use an existing role, put the role that we already have created, and click in the create function button
![lambda2](/images/lambda2.png)
16. Go to the repo, copy the src/gd_notifications.py code and paste in the code source in your lambda function, and click in the deploy button
![lambda3](/images/lambda3.png)
17. obtain your api key, with a free subscription in the sports portal, and click in the configuration environment and click edit
![lambda4](/images/lambda4.png)
18. click in add environment variable twice, and fill your own information, and click save
![lambda5](/images/lambda5.png)
19. lets save a test to our function, click en test tab, put a test name, click in save button
![lambda6](/images/lambda6.png)
20. click in test button and see if it works
![lambda7](/images/lambda7.png)
21. check your email and see the message
![email](/images/email.png)
That works!
Now we need to create a schedule rule that invokes the lamdba function to obtain the data each we want or we set up
22. type EventBridge
![event1](/images/event1.png)
23. click in create rule, give a name, in rule type click schedule, click continue button
![event2](/images/event2.png)
24. scroll down to schedule pattern, choose recurring schedule, choose time zone, and fill the cron expresion, flexible time window off, scroll down and click next button
![event3](/images/event3.png)
25.choose aws lambda, choose the function and next
![event4](/images/event4.png)
26. click in next, and click in create schedule button
![event5](/images/event5.png)
27. now you can check your email every time that the schedule is set up and you will see the notification.
![event6](/images/event6.png)
28. That is all, study these topics, try it and share with others.

#### With love and grateful to people who creates these projects:
[Day 2 explanation](https://www.youtube.com/watch?app=desktop&v=09WfkKc0x_Q&t=1430s "Day 2 explanation")

[Original code](https://github.com/ifeanyiro9/game-day-notifications/tree/main "Original code")

[LinkedIn publication](https://www.linkedin.com/posts/ifeanyi-otuonye_devopsallstarschallenge-cloud-cloudcomputing-activity-7282383262868463617-brIT?utm_source=share&utm_medium=member_desktop "LinkedIn publication")