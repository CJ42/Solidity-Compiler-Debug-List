# ABI encoding & decoding

This section covers all the errors messages related to the use of `abi` encoder and decoder.

## Decoding: Invalid tuple of types declared

|Heading|Description|
|-|-|
|**Title**|Decoding: Invalid tuple of types declared|
|**Type**|`TypeError`|
|**Message**|```The second argument to "abi.decode" has to be a tuple of types.```|
|**Solidity version**||
|**Reference**|[TypeChecker.cpp, Lines]()|
|**Contributors**|CJ42|


**Description**

This error occurs when you do not declare appropriately the type returned by the abi decoding function.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {

    // Decode a number represented in bytes to an uint
    function decodeNumber(bytes memory data) public pure returns (uint8) {
        uint8 nb;
        nb = abi.decode(data, uint8);
        return nb;
    }

}
```

**Solution**

Even if you want to decode one value, or return one type, **you still have to specify it between parentheses**.

```solidity
pragma solidity ^0.5.0;

contract Example {

    // Decode a number represented in bytes to an uint
    function decodeNumber(bytes memory data) public pure returns (uint8) {
        uint8 nb;
        nb = abi.decode(data, (uint8));
        return nb;
    }

}
```