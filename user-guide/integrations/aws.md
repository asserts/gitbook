# AWS

For monitoring AWS Serverless like AWS Lambda, ECS Fargate, and other services like AWS Kinesis, etc, the AWS Exporter needs to be installed.&#x20;

## Install AWS Exporter on ECS

The Asserts AWS Exporter can be set up by using a [CloudFormation template](https://s3.us-west-2.amazonaws.com/downloads.asserts.ai/aws-integration/ecs/v3/aws-integration-https-without-dns.yaml). The following inputs would need to be provided -

**VPC and Subnets**

1. **VPC Id** The VPC in which the ECS Services and the Application Load Balancer need to be installed.&#x20;
2. **Subnets** The subnets for the service instances. At least 2 subnets in different availability zones need to be provided

**ECS Cluster and Log Group Settings**

1. **ECS Cluster Name** The ECS Cluster name under which the asserts exporter services will be created. Defaults to **asserts-aws-integration**
2. **Log Group Name** The CloudWatch log group name under which the logs for the services will be **** stored. Defaults to **asserts-aws-integration**
3. **Log Group Retention Period** The log retention. Defaults to **7 days**
4. **Enable Container Insights** This is an ECS Cluster level setting that will determine if ECS resource utilization metrics need to be captured at the container level. This defaults to **enabled**. This has cost implications so if cost is a concern, it can be set to **disabled**

**Asserts TSDB and API Server Credentials**

1. **Asserts Tenant Name** If your asserts application URL is https://acme.app.asserts.ai, your tenant name is **acme**
2. **Asserts Remote Write URL** For e.g. https://acme.tsdb.asserts.ai
3. **Asserts Remote Write Password** The Asserts remote write password
4. **Asserts API Server URL** For e.g. https://acme.app.asserts.ai
5. **Asserts API Server API User Name** An API key can be created in Asserts. In the Asserts product, Go to your Profile -> Settings -> Credentials and create a new credential. Copy the **Id** and **Secret** for use later. Specify the **Id** as the User name
6. **Asserts API Server API Password** Specify the **Secret** as the password

![](../../.gitbook/assets/Asserts\_API\_Credential\_v2.gif)

**Configure AWS API Key in Asserts**

![](../../.gitbook/assets/Save\_AWS\_API\_Key\_In\_Asserts.gif)

#### Step 2 Verify the metrics in Asserts

Verify the presence of metrics in Asserts. E.g., for AWS Lambda you can check for the metric `aws_lambda_timeout_seconds`

![Lambda metric in Asserts ](<../../.gitbook/assets/Screen Shot 2021-12-20 at 9.34.15 AM.png>)

**Step 3 Verify that Asserts ECS Services and Application Load Balancer are discovered**

![](<../../.gitbook/assets/Screen Shot 2022-05-25 at 1.18.41 PM.png>)
