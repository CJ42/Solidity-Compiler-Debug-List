# Modifiers

## No `_;` specified in Modifier body

|Heading|Description|
|-|-|
|**Title**|No `_;` specified in Modifier body|
|**Type**|`SyntaxError`|
|**Message**|```Modifier body does not contain '_'```|
|**Solidity version**||
|**Reference**|[SyntaxChecker.cpp, Line 143 - 144](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L143-L144)|
|**Contributors**|CJ42|


**Description**

The modifier does not contain the placeholder `_;`

**Example**

```solidity
pragma solidity ^0.5.9;

contract MyContract {

    modifier Fee {
        require (msg.value != 0);
    }
}

```

**Solution**

Insert the placeholder `_;` inside your modifier body to specify where the function body that the modifier applies to will be placed.


---

## Modifier applied to abstract function

|Heading|Description|
|-|-|
|**Title**|Modifier applied to abstract function|
|**Type**|`SyntaxError`|
|**Message**|```Functions without implementation cannot have modifiers.```|
|**Solidity version**||
|**Reference**|[SyntaxChecker.cpp, Lines...](#)|
|**Contributors**|CJ42|


**Description**

This error defines an attempt to apply a `modifier` to an abstract function (function without a body). This is not possible in Solidity.

**Example**

```solidity
pragma solidity ^0.5.0;

contact Example {

    modifier TxFee(uint _fee) {
        msg.value + _fee;
        _;
    }

    function sendEther() public TxFee;

}
```

**Solution**



---

## Modifier including msg.value applied to non-payable function

|Heading|Description|
|-|-|
|**Title**|Modifier including msg.value applied to non-payable function|
|**Type**|`TypeError`|
|**Message**|```This modifier uses "msg.value" or "callvalue()" and thus the function has to be payable or internal```|
|**Solidity version**||
|**Reference**|[ViewPureChecker.cpp, Line 283 - 288](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ViewPureChecker.cpp#L283-L288)|
|**Contributors**|CJ42|


**Description**

A non-`payable` function applies a `modifier` that handle ethers.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {
    
    modifier TxFee(uint _fee) {
        msg.value + _fee;
        _;
    }
    
    function sendEther(address payable _recipient, uint _ether) public TxFee(10) {
        _recipient.transfer(_ether);
    }

    
}
```

**Solution**



---