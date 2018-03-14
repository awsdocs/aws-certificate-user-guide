# Create a Cluster<a name="create-cluster"></a>

A cluster is a collection of individual HSMs\. AWS CloudHSM synchronizes the HSMs in each cluster so that they function as a logical unit\.

**Important**  
When you create a cluster, AWS CloudHSM creates a [service\-linked role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) named AWSServiceRoleForCloudHSM\. If AWS CloudHSM cannot create the role or the role does not already exist, you may not be able to create a cluster\. For more information, see [Resolving Cluster Creation Failures](troubleshooting-create-cluster.md)\. For more information about service–linked roles, see [Understanding Service–Linked Roles](create-iam-user.md#service-linked-roles)\. 

When you create a cluster, AWS CloudHSM creates a security group for the cluster on your behalf\. This security group controls network access to the HSMs in the cluster\. It allows inbound connections only from Amazon Elastic Compute Cloud \(Amazon EC2\) instances that are in the security group\. By default, the security group doesn't contain any instances\. Later, you [launch a client instance](launch-client-instance.md) and add it to this security group\. 

**Warning**  
The cluster's security group prevents unauthorized access to your HSMs\. Anyone that can access instances in the security group can access your HSMs\. Most operations require a user to log in to the HSM, but it's possible to zeroize HSMs without authentication, which destroys the key material, certificates, and other data\. If this happens, data created or modified after the most recent backup is lost and unrecoverable\. To prevent this, ensure that only trusted administrators can access the instances in the cluster's security group or modify the security group\. 

You can create a cluster from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), or the AWS CloudHSM API\. 

**To create a cluster \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. On the navigation bar, use the region selector to choose one of the [AWS Regions where AWS CloudHSM is currently supported](http://docs.aws.amazon.com/general/latest/gr/rande.html#cloudhsm_region)\. 

1. Choose **Create cluster**\.

1. In the **Cluster configuration** section, do the following:

   1. For **VPC**, select the VPC that you created\.

   1. For **AZ\(s\)**, next to each Availability Zone, choose the private subnet that you created\. 

1. Choose **Next: Review**\.

1. Review your cluster configuration, then choose **Create cluster**\.

**To create a cluster \([AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/)\)**

+ At a command prompt, run the [create\-cluster](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-cluster.html) command\. Specify the HSM instance type and the subnet IDs of the subnets where you plan to create HSMs\. Use the subnet IDs of the private subnets that you created\. Specify only one subnet per Availability Zone\. 

  ```
  $ aws cloudhsmv2 create-cluster --hsm-type hsm1.medium --subnet-ids <subnet ID 1> <subnet ID 2> <subnet ID N>
  
  {
      "Cluster": {
          "BackupPolicy": "DEFAULT",
          "VpcId": "vpc-50ae0636",
          "SubnetMapping": {
              "us-west-2b": "subnet-49a1bc00",
              "us-west-2c": "subnet-6f950334",
              "us-west-2a": "subnet-fd54af9b"
          },
          "SecurityGroup": "sg-6cb2c216",
          "HsmType": "hsm1.medium",
          "Certificates": {},
          "State": "CREATE_IN_PROGRESS",
          "Hsms": [],
          "ClusterId": "cluster-igklspoyj5v",
          "CreateTimestamp": 1502423370.069
      }
  }
  ```

**To create a cluster \(AWS CloudHSM API\)**

+ Send a [http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html) request\. Specify the HSM instance type and the subnet IDs of the subnets where you plan to create HSMs\. Use the subnet IDs of the private subnets that you created\. Specify only one subnet per Availability Zone\.

If your attempts to create a cluster fail, it might be related to problems with the AWS CloudHSM service\-linked roles\. For help to resolve the failure, see [Resolving Cluster Creation Failures](troubleshooting-create-cluster.md)\.