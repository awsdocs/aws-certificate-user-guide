# findSingleKey<a name="key_mgmt_util-findSingleKey"></a>

The findSingleKey command in the key\_mgmt\_util tool verifies that a key exists on all HSMs in the cluster\.

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="findSingleKey-syntax"></a>

```
findSingleKey -h

findSingleKey -k <key-handle>
```

## Example<a name="findSingleKey-examples"></a>

**Example**  
This command verifies that key `252136` exists on all three HSMs in the cluster\.  

```
Command: findSingleKey -k 252136
Cfm3FindKey returned: 0x00 : HSM Return: SUCCESS

        Cluster Error Status
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 0 and err state 0x00000000 : HSM Return: SUCCESS
```

## Parameters<a name="findSingleKey-parameters"></a>

**\-h**  
Displays help for the command\.   
Required: Yes

**\-k**  
Specifies the key handle of one key in the HSM\. This parameter is required\.   
To find key handles, use the [findKey](key_mgmt_util-listUsers.md) command\.  
Required: Yes

## Related Topics<a name="findSingleKey-seealso"></a>

+ [findKey](key_mgmt_util-listUsers.md)

+ [getKeyInfo](key_mgmt_util-listUsers.md)

+ [getAttribute](key_mgmt_util-findKey.md)