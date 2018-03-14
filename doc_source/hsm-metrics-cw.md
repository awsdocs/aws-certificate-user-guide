# Getting CloudWatch Metrics<a name="hsm-metrics-cw"></a>

AWS CloudHSM publishes metrics about your HSM instances to your CloudWatch dashboard\. The metrics can be grouped by region, by cluster ID, and by HSM ID\. Note, however, that the HSM ID will change if AWS CloudHSM replaces a failed HSM\. We therefore recommend that you alarm and measure on the regional or cluster ID level rather than on the HSM ID\. The following metrics are available: 

+ **HsmUnhealthy: **The HSM instance is not performing properly\. AWS CloudHSM automatically replaces unhealthy instances for you\. However, you can proactively expand cluster size to avoid HSM replacement\. 

+ **HsmTemperature: **Junction temperature of the hardware processor\. The system shuts down if temperature reaches 110 degrees Centigrade\. 

+ **HsmKeysSessionOccupied: **Number of session keys being used by the HSM instance\.

+ **HsmKeysTokenOccupied: **Number of token keys being used by the HSM instance and the cluster\. 

+ **HsmSslCtxsOccupied: **Number of end\-to\-end encrypted channels currently established for the HSM instance\. Up to 2048 channels are allowed\. 

+ **HsmSessionCount: **Number of open connections to the HSM instance\. Up to 2048 are allowed\. By default, the client daemon is configured to open two sessions with each HSM instance under one end\-to\-end encrypted channel\. 

+ **HsmUsersAvailable: **Number of additional users that can be created\. This equals the maximum number of users, **HsmUsersMax**, minus the users created to date\. 

+ **HsmUsersMax: **Maximum number of users that can be created on the HSM instance\. Currently this is 1024\.

+ **InterfaceEth2OctetsInput : **Cumulative sum of traffic to the HSM to date\. We recommend that you also examine Amazon EC2 instance metrics\.

+ **InterfaceEth2OctetsOutput : ** see the preceding metric \- **InterfaceEth2OctetsInput**\.