##Introduction
AXCiS is using AWS as Cloud Service. Every server configuration including SSL service is based on AWS. Only domain name that manage by ARI. Asia Pacific (Tokyo) is the server region used by AXCiS.

##AWS Service using by AXCiS Project is listed below.
### EC2
VM from AWS, using for `Source` (Instance for Master Configuration) and `Service Instance` for each `Auto Scaling Group`

### CodeDeploy
Deploying to AXCiS Server can do it by AWS `CodeDeploy` and manually build and deploy directly on each instance. But we have more than one instance in each `Auto Scaling Group` so using `CodeDeploy` is ideally way to do this task.  

### S3
A file server from AWS, using for uploading any files from AXCiS Project. We can upload `Application Source` here and deploy via `CodeDeploy`. But currently situation we use `GitHub` as our source control and `CodeDeploy` has option to deploy from `GitHub` so we using this method.

### RDS
Database server from AWS, it include varieties of database server. We are using MySQL for AXCiS Project. RDS include logging system, backup and much more. It's like `MySQL` + `Management Features`. 

### Certificate Manager
Basically `SSL` service if we buying from other `SSL` Service. We need to manage it like `Domain name`. It will has expire date, need to renew and some micro management. But on AWS we can get Free `SSL` including auto renew by itself. `SSL` individually from AWS is free, but require to use additional service such as `Load Balancer`. Anyway we need to use `Load Balancer` as well for best performance.

##Servers Configuration
MySQL
##Keywords
AWS - Amazon Web Service

##AWS Features

### EC2
EC2 is the most features that we need to work on it. AXCiS Infrastructure is show as figure below.
(img)

As we seen from the figure. Basically we need at least one `instance` to act as web server. But from AXCiS Infrastructure, what you can see is we are using `Auto Scaling Group`. Which is a group of `instance` that can scale up and down _(Add `instance` automatically when the usage is high, remove `instance` automatically when the usage is low.)_ and those `instance` generate follow by the `Launch Configuration `. To have `instance` able to generate, we need a source for it. Typically source is an single `instance` that we completely configure every thing in it and it is ready to export in to a VM image call `AMI` in AWS. Then `Load Balancers` will take care of traffic to each `instance`.

#### Instances
As you understand from small introduction in EC2 section above. AXCiS is using a lot of instance categorize by it's own role.

AXCiS
#### AMIs
#### Security Groups
#### Elastic IPs
#### Key Pairs
#### Load Balancers
#### Launch Configurations
#### Auto Scaling Groups

### CodeDeploy
AXCiS Project has 4 Application name based on server.

1. AXCiS_Only_Development
2. AXCiS_Only_Staging
3. AXCiS_Production
4. AXCiS_Thai

### S3
### RDS
### IAM
### Certificate Manager
### VPC

##AWS in practical for AXCiS

###Deployment

> To deploy application please follow by these steps.

1. Login to AWS Console
2. Click on `CodeDeploy` from dashboard.
3. To select server to deploy, in AXCiS configuration will show as `Application Name`. Click on `Application Name` you wish to deploy.
4. Select on `Deployment Group`.
5. Click on `Actions` button on top.
6. Select on `Deploy New Revision`.
7. Select `Application Name` from drop down list again.
8. Select on the `Deployment Group`.
9. On `Revision Type` AXCiS use GitHub.
10. Input `arms-air/axcis_spring_boot` Repository Name.
11. Input `Commit ID` you wish to deploy.
12. On Deployment Config select on `CodeDeployDefault.OneAtATime`.
13. Click on `Deploy Now`.

>After deploy, don't forget to report to PM

###Setting Server into maintenance mode
> To set server into maintenance mode please follow these steps.

1. Login to AWS Console.
2. From dashboard click on `EC2`.
3. From `EC2` Dashboard click on `Load Balancers` from left side.
4. In `Load Balancer` page select on server you would like to set into maintenance mode. It will pop the server detail below.
5. Click on `Listeners` tab.
6. Click on `Edit` and change `Instance Port` for `Load Balancer Protocol` name `HTTP`. From `80` to `9001`.
7. Click on `Add` and add `Load Balancer Port` to `9002` and `Instance Port` to `9002`. (We call this dev_access port).
8. Click on `save`.
9. Click on `Description` tab.
10. Look for `Edit stickiness` for port number `9002` and click on it.
11. Select `Enable load balancer generated cookie stickiness`.
12. In `Expiration Period` leave this input as `blank` and then click `save`.

> When you follow these 12 steps above right now your selected server will be on maintenance mode.
> Any `http_request` from Port number 80 will redirect to `Maintenance Page`.
> if we need to access to application (Mostly maintenance is to deploy new version of application). We can do the http_request from port number 9002 to test our application.

###Set server from maintenance mode back to live service

1. Login to AWS Console.
2. From dashboard click on `EC2`.
3. From `EC2` Dashboard click on `Load Balancers` from left side.
4. In `Load Balancer` page select on server you would like to set back to live service. It will pop the server detail below.
5. Click on `Listeners` tab.
6. Click on `Edit` and change `Instance Port` for `Load Balancer Protocol` name `HTTP`. From `9001` to `80`.
7. Click `x` button to remove `dev_access_port` (Port number 9002).
8. Click `Save`.

> Right now the server will back to live service. Please test `http_request` on 80 and 9002 to confirm your configuration.

###



<img src="img/asset.png"/>