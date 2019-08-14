# Events

## Event declared twice

|Heading|Description|
|-|-|
|**Title**|Event declared twice|
|**Type**|`declarationError`|
|**Message**|```Event with same name and arguments defined twice.```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, |
|**Contributors**|CJ42|


**Description**

The contrat declares an `event` with the same name and argument twice.

In Solidity, **event overloading** is allowed.
Two events can share the same name as long as their topics are differents (the same way that duplicate function names are allowed as long as their argument types differ). 

**Example**

```solidity
pragma solidity ^0.5.10;

contract Example {
    
    event Deposit(address _address, uint _deposit);
    event Deposit(address _address, uint _deposit);
    
}
```

**Solution**

Change the name of your event, or specifies different topics.