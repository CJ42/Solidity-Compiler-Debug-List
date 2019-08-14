# Array

## Invalid array length

|Heading|Description|
|-|-|
|**Title**|Invalid array length|
|**Type**|`fatalTypeError`|
|**Message**|```Invalid array length, expected integer literal or constant expression.```|
|**Solidity version**||
|**Reference**|ReferencesResolver.cpp, [Lines 256 - 257](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L256-L257)|
|**Contributors**|CJ42|


**Description**

This error occur when you specify the length of a fixed size array with a value that is not constant (Like a variable of `uint` type that can be modified);

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {
    
    uint _length = 10;
    
    uint[_length] my_array;
}
```

**Solution**

Specify the length of your array with a literal number (5, 10, 238...) or a constant variable.

---

## Array with negative length specified

|Heading|Description|
|-|-|
|**Title**|Array with negative length specified|
|**Type**|`fatalTypeError`|
|**Message**|```Array with negative length specified.```|
|**Solidity version**||
|**Reference**|[ReferencesResolver.cpp, Lines 262 - 263](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L262-L263)|
|**Contributors**|CJ42|


**Description**

An variable of type fixed-size array is declared with a length being a negative number.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {
    
    uint[-5] my_array;
    
}
```

**Solution**

Use a positive number to specify the length of your fixed-size array.

---

## Array with zero length specified

|Heading|Description|
|-|-|
|**Title**|Array with zero length specified|
|**Type**|`fatalTypeError`|
|**Message**|```Array with zero length specified.```|
|**Solidity version**||
|**Reference**|[ReferencesResolver.cpp, Lines 258 - 259](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L258-L259)|
|**Contributors**|CJ42|


**Description**

An variable of type fixed-size array is declared with a length being 0.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {
    
    uint[0] my_array;
    
}
```

**Solution**

Use a positive number to specify the length of your fixed-size array.

---

## Array to large to be encoded

|Heading|Description|
|-|-|
|**Title**|Array to large to be encoded|
|**Type**|`TypeError`|
|**Message**|```Array is too large to be encoded.```|
|**Solidity version**||
|**Reference**|[TypeChecker.cpp, Lines...](#)|
|**Contributors**|CJ42|


**Description**

This error occurs when you define a fixed-size array that contains more than 134,217,727 indexes. This appear to be the maximum limit of a working storage. See the links below for related topic

- https://www-01.ibm.com/support/docview.wss?uid=swg21220835
- https://www.ibm.com/support/knowledgecenter/en/SS6SG3_4.2.0/com.ibm.entcobol.doc_4.2/MG/igymapxg001.htm
- http://ibmmainframes.com/about56564.html


**Example**

pragma solidity ^0.5.0;

contract HugeArray {

    function notPossibleToEncode() public pure returns (bytes memory) {
        uint[134_217_728] memory large_array;
        return abi.encode(large_array); 
    }

}


**Solution**

Use a smaller value...