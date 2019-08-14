# Libraries

## Fallback function defined in Library

|Heading|Description|
|-|-|
|**Title**|Fallback function defined in library|
|**Type**|`typeError`|
|**Message**|```Libraries cannot have fallback functions.```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, [Line 386 - 387](https://github.com/ethereum/solidity/blob/4f7fec6911482c9c3f8fbf2e3fa5874597648fc6/libsolidity/analysis/ContractLevelChecker.cpp#L388-L394)|
|**Contributors**|CJ42|


**Description**

A fallback function is declared within a library, which is not allowed (library can't accept ether).

**Example**

```
pragma solidity ^0.5.10;

library TokenUtils {
    
    function() external {
        
    }
    

}
```

**Solution**

Remove the fallback function from your library.

---

## Library not allowed to inherit

|Heading|Description|
|-|-|
|**Title**|Library not allowed to inherit|
|**Type**|`typeError`|
|**Message**|```Library is not allowed to inherit.```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, [Lines 459 - 460](https://github.com/ethereum/solidity/blob/4f7fec6911482c9c3f8fbf2e3fa5874597648fc6/libsolidity/analysis/ContractLevelChecker.cpp#L459-L460), also [Line 476](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/ContractLevelChecker.cpp#L476)|
|**Contributors**|CJ42|


**Description**

A library is declared of inheriting from a contract, interface or other library. This is not allowed. Libraries can't inherit in Solidity.

**Example**

```solidity
pragma solidity ^0.5.10;

contract Token {
   // could be an abstract contract
}

library SafeTokenUtils is Token {
    
    // library is not allowed to inherit
}
```

**Solution**



---

## Library not allowed to inherit

|Heading|Description|
|-|-|
|**Title**|Library not allowed to inherit|
|**Type**|`typeError`|
|**Message**|```Libraries cannot be inherited from.```|
|**Solidity version**||
|**Reference**|[TypeChecker.cpp, Lines...](#)|
|**Contributors**|CJ42|


**Description**

A contract attempts to inherit from a `library` contract via the `is` keyword. In Solidity, a `library` cannot be inherited from.

**Example**

```solidity
pragma solidity ^0.5.10;

library MyLibrary {

    // library code here

}

contract MyContract is MyLibrary {
    
    // Libraries cannot be inherited from

}
```

**Solution**

Include the library inside your contract by using the declaration: `using LibraryName for VariableType`. 
You can also specify the wildcard symbol `*` if you want to use the `library` that you are importing for any type.

```solidity
pragma solidity ^0.5.0;

library MyLibrary {

    // library code here
    
}

contract MyContract {
    
    using MyLibrary for *;

}
```


---


## State variable declared inside library

|Heading|Description|
|-|-|
|**Title**|State variable declared inside library|
|**Type**|`typeError`|
|**Message**|```Library cannot have non-constant state variables```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, [Lines 463 - 464](https://github.com/ethereum/solidity/blob/4f7fec6911482c9c3f8fbf2e3fa5874597648fc6/libsolidity/analysis/ContractLevelChecker.cpp#L463-L464)|
|**Contributors**|CJ42|


**Description**

State variables are declared inside the variable. A library can only hold **constant** state variables.

**Example**

```solidity
pragma solidity ^0.5.10;

library TokenUtils {
    
    uint total_supply;

    // your library code here
    
}
```

**Solution**

You can only declared `constant` variables in a library. Here is an abstract example.

```solidity
pragma solidity ^0.5.10;

library TokenUtils {
    
    // If it would be a library to create a "Bitcoin-like" Token
    uint constant total_supply = 21000000;
    
}
```

---

## Libraries cannot call their own functions externally

|Heading|Description|
|-|-|
|**Title**|Libraries cannot call their own functions externally|
|**Type**|`TypeError`|
|**Message**|```Libraries cannot call their own functions externally```|
|**Solidity version**|since 0.5.9|
|**Reference**|[StaticAnalyzer.cpp, Lines 312 - 324](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L312-L324)|
|**Contributors**||


**Description**



**Example**

```solidity
pragma solidity 0.5.9;

library L {

    using L for *;

    function f() public pure returns (uint r) {
        return r.g();
    }

    function g(uint) public pure returns (uint) {
        return 2;
    }
}
```

**Solution**



## References

- https://github.com/ethereum/solidity/issues/6451
- https://github.com/ethereum/solidity/pull/6604/files