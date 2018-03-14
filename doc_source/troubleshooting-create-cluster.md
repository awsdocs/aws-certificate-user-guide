# Resolving Cluster Creation Failures<a name="troubleshooting-create-cluster"></a>

When you create a cluster, AWS CloudHSM creates the AWSServiceRoleForCloudHSM service\-linked role, if the role does not already exist\. If AWS CloudHSM cannot create the service\-linked role, your attempt to create a cluster might fail\.

This topic explains how to resolve the most common problems so you can create a cluster successfully\. You need to create this role only one time\. Once the service\-linked role is created in your account, you can use any of the supported methods to create additional clusters and to manage them\.

The following sections offer suggestions to troubleshoot cluster creation failures that are related to the service\-linked role\. If you try them but are still unable to create a cluster, contact [AWS Support](https://aws.amazon.com/contact-us/)\. For more information about the AWSServiceRoleForCloudHSM service\-linked role, see [Understanding Service–Linked Roles](create-iam-user.md#service-linked-roles)\. 


+ [Add the Missing Permission](#missing-permission)
+ [Create the Service\-Linked Role Manually](#api-call-failure)
+ [Use a Nonfederated User](#non-federated-user)

## Add the Missing Permission<a name="missing-permission"></a>

To create a service\-linked role, the user must have the `iam:CreateServiceLinkedRole` permission\. If the IAM user who is creating the cluster does not have this permission, the cluster creation process fails when it tries to create the service\-linked role in your AWS account\.

When a missing permission causes the failure, the error message includes the following text\.

```
This operation requires that the caller have permission to call iam:CreateServiceLinkedRole to create the CloudHSM Service Linked Role.
```

To resolve this error, give the IAM user who is creating the cluster the `AdministratorAccess` permission or add the `iam:CreateServiceLinkedRole` permission to the user's IAM policy\. For instructions, see [Adding Permissions to a New or Existing User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#w2ab1c19c19c26b9)\. 

Then try to [create the cluster](create-cluster.md) again\. 

## Create the Service\-Linked Role Manually<a name="api-call-failure"></a>

You can use the IAM console, CLI, or API to create the AWSServiceRoleForCloudHSM service\-linked role\. For more information, see [Creating a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. 

## Use a Nonfederated User<a name="non-federated-user"></a>

Federated users, whose credentials originate outside of AWS, can perform many of the tasks of a nonfederated user\. However, AWS does not allow users to make the API calls to create a service\-linked role from a federated endpoint\. 

To resolve this problem, [create a non\-federated user](create-iam-user.md) with the `iam:CreateServiceLinkedRole` permission, or give an existing non\-federated user the `iam:CreateServiceLinkedRole` permission\. Then have that user [create a cluster](create-cluster.md) from the AWS CLI\. This creates the service\-linked role in your account\.

Once the service\-linked role is created, if you prefer, you can delete the cluster that the nonfederated user created\. Deleting the cluster does not affect the role\. Thereafter, any user with the required permissions, included federated users, can create AWS CloudHSM clusters in your account\.

To verify that the role was created, open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) and choose **Roles**\. Or use the IAM [get\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/get-role.html) command in the AWS CLI\.

```
$  aws iam get-role --role-name AWSServiceRoleForCloudHSM
{
    "Role": {
        "Description": "Role for CloudHSM service operations",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Action": "sts:AssumeRole",
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "cloudhsm.amazonaws.com"
                    }
                }
            ]
        },
        "RoleId": "AROAJ4I6WN5QVGG5G7CBY",
        "CreateDate": "2017-12-19T20:53:12Z",
        "RoleName": "AWSServiceRoleForCloudHSM",
        "Path": "/aws-service-role/cloudhsm.amazonaws.com/",
        "Arn": "arn:aws:iam::111122223333:role/aws-service-role/cloudhsm.amazonaws.com/AWSServiceRoleForCloudHSM"
    }
}
```