# deleteKey<a name="key_mgmt_util-deleteKey"></a>

The deleteKey command in key\_mgmt\_util deletes a key from the HSM\. You can only delete one key at a time\. Deleting one key in a key pair has no effect on the other key in the pair\.

Only the key owner can delete a key\. Users who share the key can use it in cryptographic operations, but not delete it\. 

Before you run any key\_mgmt\_util command, you must [start key\_mgmt\_util](key_mgmt_util-getting-started.md#key_mgmt_util-start) and [login](key_mgmt_util-getting-started.md#key_mgmt_util-log-in) to the HSM as a crypto user \(CU\)\. 

## Syntax<a name="deleteKey-syntax"></a>

```
deleteKey -h 

deleteKey -k
```

## Examples<a name="deleteKey-examples"></a>

These examples show how to use deleteKey to delete keys from your HSMs\.

**Example : Delete a Key**  
This command deletes the key with key handle `6`\. When the command succeeds, deleteKey returns success messages from each HSM in the cluster\.  

```
Command: deleteKey -k 6

        Cfm3DeleteKey returned: 0x00 : HSM Return: SUCCESS

        Cluster Error Status
        Node id 1 and err state 0x00000000 : HSM Return: SUCCESS
        Node id 2 and err state 0x00000000 : HSM Return: SUCCESS
```

**Example : Delete a Key \(Failure\)**  
When the command fails because no key has the specified key handle, deleteKey returns an invalid object handle error message\.  

```
Command: deleteKey -k 252126

        Cfm3FindKey returned: 0xa8 : HSM Error: Invalid object handle is passed to this operation

        Cluster Error Status
        Node id 1 and err state 0x000000a8 : HSM Error: Invalid object handle is passed to this operation
        Node id 2 and err state 0x000000a8 : HSM Error: Invalid object handle is passed to this operation
```
When the command fails because the current user is not the owner of the key, the command returns an access denied error\.  

```
Command:  deleteKey -k 262152

Cfm3DeleteKey returned: 0xc6 : HSM Error: Key Access is denied.
```

## Parameters<a name="deleteKey-parameters"></a>

**\-h**  
Displays command line help for the command\.   
Required: Yes

**\-k**  
Specifies the key handle of the key to delete\. To find the key handles of keys in the HSM, use [findKey](key_mgmt_util-findKey.md)\.  
Required: Yes

## Related Topics<a name="deleteKey-seealso"></a>

+ [findKey](key_mgmt_util-findKey.md)