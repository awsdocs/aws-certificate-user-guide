# Error2String<a name="key_mgmt_util-Error2String"></a>

The Error2String helper command in key\_mgmt\_util returns the error that corresponds to a key\_mgmt\_util hexadecimal error code\. You can use this command when troubleshooting your commands and scripts\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="Error2String-syntax"></a>

```
Error2String -h

Error2String -r <response-code>
```

## Examples<a name="Error2String-examples"></a>

These examples show how to use Error2String to get the error string for a key\_mgmt\_util error code\. 

**Example : Get an Error Description**  
This command gets the error description for the `0xdb` error code\. The description explains that an attempt to log in to key\_mgmt\_util failed because the user has the wrong user type\. Only crypto users \(CU\) can log in to key\_mgmt\_util\.  

```
        Command:  Error2String -r 0xdb
        
        Error Code db maps to HSM Error: Invalid User Type.
```

**Example : Find the Error Code**  
This example shows where to find the error code in a key\_mgmt\_util error\. The error code, `0xc6`, appears after the string: `Cfm3command-name returned: `\.  
In this example, [getKeyInfo](key_mgmt_util-getKeyInfo.md) indicates that the current user \(user 4\) can use the key in cryptographic operations\. Nevertheless, when the user tries to use [deleteKey](key_mgmt_util-deleteKey.md) to delete the key, the command returns error code `0xc6`\.   

```
        Command:  deleteKey -k 262162

        Cfm3DeleteKey returned: 0xc6 : HSM Error: Key Access is denied

        Cluster Error Status

        Command:  getKeyInfo -k 262162
        
        Cfm3GetKey returned: 0x00 : HSM Return: SUCCESS

       Owned by user 3

       also, shared to following 1 user(s):

                4
```
If the `0xc6` error is reported to you, you can use an Error2String command like this one to look up the error\. In this case, the `deleteKey` command failed with an access denied error because the key is shared with the current user but owned by a different user\. Only key owners have permission to delete a key\.  

```
        Command:  Error2String -r 0xa8
        
        Error Code c6 maps to HSM Error: Key Access is denied
```

## Parameters<a name="Error2String-parameters"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-r**  
Specifies a hexadecimal error code\. The `0x` hexadecimal indicator is required\.  
Required: Yes