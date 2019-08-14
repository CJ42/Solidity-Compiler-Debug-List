# Contracts

## Precedence order in Contract inheritance

|Heading|Description|
|-|-|
|**Title**|Precedence order in Contract inheritance|
|**Type**|`fatalTypeError`|
|**Message**|```Definition of base has to precede definition of derived contract```|
|**Solidity version**||
|**Reference**|NameAndTypeResolver.cpp, [Lines 388 - 389](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L388-L389)|
|**Contributors**|CJ42|


**Description**

The base contract is defined after the derived contract.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Person is Greetings {
    
   // ...
   
}

contract Greetings {
    
    // ...
    
}
```

**Solution**

Define the base contract first, then the derived contract.

```solidity
pragma solidity ^0.5.0;

contract Greetings {
    
    // ...
    
}

contract Person is Greetings {
    
   // ...
   
}
```