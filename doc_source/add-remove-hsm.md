# Adding or Removing HSMs in an AWS CloudHSM Cluster<a name="add-remove-hsm"></a>

To scale up or down your AWS CloudHSM cluster, add or remove HSMs by using the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/) or one of the [AWS SDKs or command line tools](https://aws.amazon.com/tools/)\.


+ [Adding an HSM](#add-hsm)
+ [Removing an HSM](#remove-hsm)

## Adding an HSM<a name="add-hsm"></a>

The following figure illustrates the events that occur when you add an HSM to a cluster\.

![\[Animation showing the events that occur when you add an HSM to a cluster.\]](http://docs.aws.amazon.com/cloudhsm/latest/userguide/images/add-hsm.gif)

1. You add a new HSM to a cluster\. The following procedures explain how to do this from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), and the [AWS CloudHSM API](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/)\.

   This is the only action that you take\. The remaining events occur automatically\.

1. AWS CloudHSM makes a backup copy of an existing HSM in the cluster\. For more information, see [Backups](backups.md)\.

1. AWS CloudHSM restores the backup onto the new HSM\. This ensures that the HSM is in sync with the others in the cluster\.

1. The existing HSMs in the cluster notify the AWS CloudHSM client that there's a new HSM in the cluster\.

1. The client establishes a connection to the new HSM\.

**To add an HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. Choose a cluster for the HSM that you are adding\.

1. On the **HSMs** tab, choose **Create HSM**\.

1. Choose an Availability Zone \(AZ\) for the HSM that you are creating\. Then choose **Create**\.

**To add an HSM \(AWS CLI\)**

+ At a command prompt, issue the [create\-hsm](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-hsm.html) command, specifying a cluster ID and an Availability Zone for the HSM that you are creating\. If you don't know the cluster ID of your preferred cluster, issue the [describe\-clusters](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command\. Specify the Availability Zone in the form of `us-east-2a`, `us-east-2b`, etc\.

  ```
  $ aws cloudhsmv2 create-hsm --cluster-id <cluster ID> --availability-zone <Availability Zone>
  {
      "Hsm": {
          "State": "CREATE_IN_PROGRESS",
          "ClusterId": "cluster-5a73d5qzrdh",
          "HsmId": "hsm-lgavqitns2a",
          "SubnetId": "subnet-0e358c43",
          "AvailabilityZone": "us-east-2c",
          "EniId": "eni-bab18892",
          "EniIp": "10.0.3.10"
      }
  }
  ```

**To add an HSM \(AWS CloudHSM API\)**

+ Send a [http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateHsm.html) request, specifying the cluster ID and an Availability Zone for the HSM that you are creating\.

## Removing an HSM<a name="remove-hsm"></a>

You can remove an HSM by using the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS CLI](https://aws.amazon.com/cli/), or the AWS CloudHSM API\.

**To remove an HSM \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. Choose the cluster that contains the HSM that you are removing\.

1. On the **HSMs** tab, choose the HSM that you are removing\. Then choose **Delete HSM**\.

1. Confirm that you want to delete the HSM\. Then choose **Delete**\.

**To remove an HSM \(AWS CLI\)**

+ At a command prompt, issue the [delete\-hsm](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/delete-hsm.html) command\. Pass the ID of the cluster that contains the HSM that you are deleting and one of the following HSM identifiers:

  + The HSM ID \(`--hsm-id`\)

  + The HSM IP address \(`--eni-ip`\)

  + The HSM's elastic network interface ID \(`--eni-id`\)

  If you don't know the values for these identifiers, issue the [describe\-clusters](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-clusters.html) command\.

  ```
  $ aws cloudhsmv2 delete-hsm --cluster-id <cluster ID> --eni-ip <HSM IP address>
  {
      "HsmId": "hsm-lgavqitns2a"
  }
  ```

**To remove an HSM \(AWS CloudHSM API\)**

+ Send a [http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DeleteHsm.html](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_DeleteHsm.html) request, specifying the cluster ID and an identifier for the HSM that you are deleting\.