# Generic errors

## Badly declared keyword

|Heading|Description|
|-|-|
|**Title**|Badly declared identifier|
|**Type**|`declaration error`|
|**Message**|```Undeclared identifier. Did you mean...```|
|**Solidity version**||
|**Reference**|ReferencesResolver.cpp, [Lines 99 - 111](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L99-L111)|
|**Contributors**|CJ42|


**Description**

This error happen when a keyword is badly written.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {
    
    address owner;
    
    modifier isOwner {
        requore(owner == msg.sender);
        _;
    }
}
```

**Solution**

Correct the error using the correct keyword suggestion given by the Solidity compiler.

---

## Statement without effect

|Heading|Description|
|-|-|
|**Title**|Statement without effect|
|**Type**|`warning`|
|**Message**|```Statement has no effect```|
|**Solidity version**||
|**Reference**|[StaticAnalyzer.cpp, Line 185](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/StaticAnalyzer.cpp#L185)|
|**Contributors**|CJ42|


**Description**

This Solidity warning specifies that the indicated statement does not do anything.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {
    
    function test2() pure internal {
        5;
    }
    
}
```

**Solution**


---


