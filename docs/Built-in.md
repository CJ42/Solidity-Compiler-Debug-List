# Built-in Solidity Functions

---

# Deprecated functions

## gasleft() instead of msg.gas

|Heading|Description|
|-|-|
|**Title**|gasleft() instead of msg.gas|
|**Type**|`TypeError`|
|**Message**|```"msg.gas" has been deprecated in favor of "gasleft()"```|
|**Solidity version**||
|**Reference**|[StaticAnalyzer.cpp, Lines 195 - 199](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L195-L199)|
|**Contributors**||


**Description**

**Example**

```solidity
msg.gas
```

**Solution**



---

## blockhash() instead of block.blockhash()

|Heading|Description|
|-|-|
|**Title**|blockhash() instead of block.blockhash()|
|**Type**|`TypeError`|
|**Message**|```"block.blockhash()" has been deprecated in favor of "blockhash()"```|
|**Solidity version**||
|**Reference**|[StaticAnalyzer.cpp, Lines 200 - 204](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L200-L204)|
|**Contributors**||


**Description**

**Example**

```solidity
block.blockhash()
```

**Solution**



---

## delegatecall() instead of callcode()

|Heading|Description|
|-|-|
|**Title**|delegatecall() instead of callcode()|
|**Type**|`TypeError`|
|**Message**|```"callcode" has been deprecated in favour of "delegatecall".```|
|**Solidity version**||
|**Reference**|[StaticAnalyzer.cpp, Lines 219 - 225](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L219-L225))|
|**Contributors**||


**Description**

**Example**

```solidity
callcode()
```

**Solution**



---

## "Throw" keyword deprecated

|Heading|Description|
|-|-|
|**Title**|Throw deprecated in favour of revert()|
|**Type**|`SyntaxError`|
|**Message**|```"throw" is deprecated in favour of "revert()", "require()" and "assert()"```|
|**Solidity version**||
|**Reference**|[SyntaxChecker.cpp, Lines...](#)|
|**Contributors**||


**Description**

**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {

    function sendMoney(address payable _recipient) public payable {
        if (msg.sender.balance > msg.value) {
            throw;
        }
        _recipient.transfer(msg.value);
    }

}
```
**Solution**