# Creating an AWS CloudHSM Cluster from a Previous Backup<a name="create-cluster-from-backup"></a>

To restore an AWS CloudHSM cluster from a previous backup, you create a new cluster, specifying the backup to restore\. After you create the cluster, you don't need to initialize or activate it\. You can just add an HSM to the cluster; this HSM contains the same users, key material, certificates, configuration, and policies that were in the backup that you restored\. For more information about backups, see [Backups](backups.md)\.

You can restore a cluster from a backup from the [AWS CloudHSM console](https://console.aws.amazon.com/cloudhsm/), the [AWS Command Line Interface \(AWS CLI\)](https://aws.amazon.com/cli/), or the AWS CloudHSM API\.

**To create a cluster from a previous backup \(console\)**

1. Open the AWS CloudHSM console at [https://console\.aws\.amazon\.com/cloudhsm/](https://console.aws.amazon.com/cloudhsm/)\.

1. Choose **Create cluster**\.

1. In the **Cluster configuration** section, do the following:

   1. For **VPC**, choose a VPC for the cluster that you are creating\.

   1. For **AZ\(s\)**, choose a private subnet for each Availability Zone that you are adding to the cluster\.

1. In the **Cluster source** section, do the following:

   1. Choose **Restore cluster from existing backup**\.

   1. Choose the backup that you are restoring\.

1. Choose **Next: Review**\.

1. Review your cluster configuration, then choose **Create cluster**\.

**To create a cluster from a previous backup \(AWS CLI\)**

+ At a command prompt, issue the [create\-cluster](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/create-cluster.html) command\. Specify the HSM instance type, the subnet IDs of the subnets where you plan to create HSMs, and the backup ID of the backup that you are restoring\. If you don't know the backup ID, issue the [describe\-backups](http://docs.aws.amazon.com/cli/latest/reference/cloudhsmv2/describe-backups.html) command\.

  ```
  $ aws cloudhsmv2 create-cluster --hsm-type hsm1.medium \
                                  --subnet-ids <subnet ID 1> <subnet ID 2> <subnet ID N> \
                                  --source-backup-id <backup ID>
  {
      "Cluster": {
          "HsmType": "hsm1.medium",
          "VpcId": "vpc-641d3c0d",
          "Hsms": [],
          "State": "CREATE_IN_PROGRESS",
          "SourceBackupId": "backup-rtq2dwi2gq6",
          "BackupPolicy": "DEFAULT",
          "SecurityGroup": "sg-640fab0c",
          "CreateTimestamp": 1504907311.112,
          "SubnetMapping": {
              "us-east-2c": "subnet-0e358c43",
              "us-east-2a": "subnet-f1d6e798",
              "us-east-2b": "subnet-40ed9d3b"
          },
          "Certificates": {
              "ClusterCertificate": "<certificate string>"
          },
          "ClusterId": "cluster-jxhlf7644ne"
      }
  }
  ```

**To create a cluster from a previous backup \(AWS CloudHSM API\)**

+ Send a [http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html](http://docs.aws.amazon.com/cloudhsm/latest/APIReference/API_CreateCluster.html) request\. Specify the HSM instance type, the subnet IDs of the subnets where you plan to create HSMs, and the backup ID of the backup that you are restoring\.

To create an HSM that contains the same users, key material, certificates, configuration, and policies that were in the backup that you restored, [add an HSM](add-remove-hsm.md#add-hsm) to the cluster\.