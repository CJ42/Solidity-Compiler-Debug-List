# Solidity Compiler Debug List
A curated list of all the Errors and Warning from the Solidity Compiler.

This repository aims to help Smart Contracts Developers on Ethereum to debug their Solidity files. It offers a list of all the errors and warnings you can possibly encounter when developing with the Remix IDE or during compilation with `solc`.

The list is based on the official C++ source code from the Solidity compiler. The errors and warning messages are all mentioned in the files located in the folder [`libsolidity/analysis`](https://github.com/ethereum/solidity/tree/develop/libsolidity/analysis). 

All the errors and warnings listed below are referenced to the original source file + line number for better analysis. Some explanations and solutions are provided to help debugging.

**NB: ** This might be a good idea to list it link here : https://dotorgstyleguide.wordpress.com/outline/layout/columns/

# The Full List

**All of these Error and Warning messages are visible in the C++ source code of the Solidity compiler, available at** [https://github.com/ethereum/solidity/tree/develop/libsolidity/analysis](https://github.com/ethereum/solidity/tree/develop/libsolidity/analysis)

- StaticAnalyzer : **8** / 12
- SyntaxChecker : /29
- TypeChecker : /192
- ConstantEvaluator : /2
- ContractLevelChecker : / 34
- ContractFlowAnalyzer : / 2
- DeclarationContainer : / 3
- DocStringAnalyser : / 3
- NameAndTypeResolver : / 20
- PostTypeChecker : / 2
- ReferencesResolver : / 20
- ViewPureChecker : / 5


## _StaticAnalyser.cpp_ 

|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
|**warning**|<pre>Unused function parameter. Remove or comment out the variable name to silence this warning</pre>||[line 124](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/StaticAnalyzer.cpp#L124)|

**Solution :**

```solidity
function sayHello(string memory _name, uint8 _age) public pure returns (string memory) {
    string memory myName = _name;
    return myName;
}


```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
|**warning**|<pre>Unused local variable</pre>||[line 127](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/StaticAnalyzer.cpp#L127)|

**Solution :**

```solidity
function birthday(uint8 _age) public pure returns (uint8) {
    uint8 new_age = _age++;
    return _age++;
}

```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
|**warning**|<pre>"this" used in constructor. Note that external functions of a contract. cannot be called while it is being constructed."</pre>||[line 234-240](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/StaticAnalyzer.cpp#L234-L240)|

**Solution :**

```solidity
constructor(uint _ether) public {
    this.sendEther(_ether);
}
    
function sendEther(uint _ether) external {
    address(this).balance + _ether;
    msg.sender.balance - _ether;
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
|**typeError**|<pre>Division by zero</pre><pre>Modulo zero</pre>||[line 284-288](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/StaticAnalyzer.cpp#L284-L288)|

**Solution :**

```solidity
function divide(uint _number) public pure {
    uint result = _number / 0;
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
|**typeError**|<pre>The function declaration is here: [function-name]. Libraries cannot call their own functions externally</pre>|since Solidity 0.5.9|[line 312-324](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L312-L324)|

**Solution :**

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

**References :**

* https://github.com/ethereum/solidity/issues/6451
* https://github.com/ethereum/solidity/pull/6604/files
-----


## _SyntaxChecker.cpp_

|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Source file does not specify required compiler version! Consider adding “pragma solidity”</pre>||_SyntaxChecker.cpp_, [Line 57 - 72](https://github.com/ethereum/solidity/blob/2ee272acf32fbad4efd1da7919c59792597ce9e6/libsolidity/analysis/SyntaxChecker.cpp#L57-L72)|

**Error :**

```solidity
> error occurs here
contract NewHello{
    // contract code here
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Experimental feature name is missing</pre>||_SyntaxChecker.cpp_, [Line 86 - 90](https://github.com/ethereum/solidity/blob/2ee272acf32fbad4efd1da7919c59792597ce9e6/libsolidity/analysis/SyntaxChecker.cpp#L86-L90)|

**Error (example):**

```solidity
pragma experimental;

contract MyContract {
    // Write your code here
}

```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Stray arguments</pre>||_SyntaxChecker.cpp_, [Line 91 - 95](https://github.com/ethereum/solidity/blob/2ee272acf32fbad4efd1da7919c59792597ce9e6/libsolidity/analysis/SyntaxChecker.cpp#L91-L95)|

**Error (example) :**

```solidity
pragma experimental ABIEncoderV2 nextGen;

contract MyContract {
    // Write your code here
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Duplicate experimental feature name</pre>||_SyntaxChecker.cpp_, [Line 103 - 104](https://github.com/ethereum/solidity/blob/2ee272acf32fbad4efd1da7919c59792597ce9e6/libsolidity/analysis/SyntaxChecker.cpp#L103-L104)|

**Error (example) :**

```solidity
pragma experimental SMTChecker;
pragma experimental SMTChecker;

contract MyContract {
    // Write your code here
}

```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Modifier body does not contain '_'</pre>||_SyntaxChecker.cpp_, [Line 143 - 144](https://github.com/ethereum/solidity/blob/2ee272acf32fbad4efd1da7919c59792597ce9e6/libsolidity/analysis/SyntaxChecker.cpp#L143-L144)|

**Error (example) :**

```solidity
pragma solidity ^0.5.9;

contract MyContract {
    
    modifier Fee {
        require (msg.value != 0);
    }
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Variable declarations can only be used inside blocks.</pre>||_SyntaxChecker.cpp_, [Line 151 - 152](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L151-L152)|

**Error (example) :**

```solidity
pragma solidity ^0.5.9;

contract MyContract {
    
    uint a = 3;
    uint b = 2;
    
    function test() public {
        
        for (uint x = 1; x <= 10; x++) uint c = a + b + x;
        
    }
    
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>"continue" has to be in a "for" or "while" loop.</pre>||_SyntaxChecker.cpp_, [Line 189 - 191](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L189-L191)|

**Error (example) :**

```solidity
pragma solidity ^0.5.9;

contract MyContract {
    
    uint a = 3;
    uint b = 2;
    
    function test(uint x) public {
        if ( x < a && x > b) {
            bool result = true;
            continue;
        }
    }
    
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>"break" has to be in a "for" or "while" loop.</pre>||_SyntaxChecker.cpp_, [Line 197 - 199](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L197-L199)|

**Error (example) :**

```solidity
pragma solidity ^0.5.9;

contract MyContract {
    
    uint a = 3;
    uint b = 2;
    
    function test(uint x) public {
        if ( x < a && x > b) {
            bool result = true;
            break;
        }
    }
    
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Invalid use of underscores in number literal. No trailing underscores allowed.</pre>||_SyntaxChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
uint constant bitcoin_supply = 21_000_000_;
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Invalid use of underscores in number literal. Only one consecutive underscores between digits allowed.</pre>||_SyntaxChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
uint constant bitcoin_supply = 21_000__000;
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Invalid use of underscores in number literal. No underscores in front of the fraction part allowed.</pre>||_SyntaxChecker.cpp_, [Line 236 - 240](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L236-L240)|

**Error (example) :**

```solidity
fixed constant test1 = 10_000_.5;
fixed constant test2 = 10_000_.5;
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Functions are not allowed to have the same name as the contract. If you intend this to be a constructor, use "constructor(...) { ... }" to define it.</pre>||_SyntaxChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
pragma solidity ^0.5.9;

contract MyContract {
    
    function MyContract() public {
        // do something
    }
    
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>No visibility specified. Did you intend to add "[suggested-visibility]"?</pre>||_SyntaxChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
function run() {
    // do something
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Functions without implementation can't have modifiers</pre>||_SyntaxChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
modifier TxFee(uint _fee) {
    msg.value + _fee;
    _;
}
    
function sendEther() public TxFee;
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **SyntaxError**|<pre>Defining empty structs is disallowed</pre>||_SyntaxChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
struct EmptyStruct {
    
}
```
-----


## _TypeChecker.cpp_


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>The second argument to "abi.decode" has to be a tuple of types.</pre>||_TypeChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
function decodeNb(bytes memory data) public pure returns (uint8) {
    uint8 nb;
    nb = abi.decode(data, uint8);
    return nb;
}
```

**Solution :**

```solidity
function decodeNb(bytes memory data) public pure returns (uint8) {
    uint8 nb;
    nb = abi.decode(data, (uint8));
    return nb;
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Interfaces cannot inherit.</pre>||_TypeChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
pragma solidity ^0.5.0;

contract MyContract {
    
    function encodeString(string memory sentence) public pure returns (bytes memory) {
        return abi.encode(sentence);
    }
    
    function decodeString(bytes memory data) public pure returns (string memory) {
        return abi.decode(data, (string));
    }
    
}

interface MyInterface is MyContract {
    // interface code here
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Libraries cannot be inherited from.</pre>||_TypeChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
pragma solidity ^0.5.0;

library MyLibrary {
    // interface code here
}

contract MyContract is MyLibrary {
    
    function encodeString(string memory sentence) public pure returns (bytes memory) {
        return abi.encode(sentence);
    }
    
    function decodeString(bytes memory data) public pure returns (string memory) {
        return abi.decode(data, (string));
    }
    
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **fatalTypeError**|<pre>Recursive struct definition.</pre>||_TypeChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
struct FootballPlayer {
    string name;
    uint8 age;
    FootballPlayer mentor;
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Mapping types for parameters or return variables can only be used in internal or library functions.</pre>||_TypeChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
pragma solidity ^0.5.0;

contract myContract {
   
    mapping(address => bool) votes;
    
    function resetVotes(mapping(address => bool) memory _voters) public pure {
        
    }
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Internal library function must be implemented if declared.</pre>||_TypeChecker.cpp_, [Line ...](#)|

**Error (example) :**
```solidity
library Messenger {
    
    function testConnection() internal pure returns(bool);
    
}
```

**Solution :**
```solidity
library Messenger {
    
    function testConnection() public pure returns(bool);
    
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Variables cannot be declared in interfaces</pre>||_TypeChecker.cpp_, [Line ...](#)|

The quickfix for this error is to encapsulate the variables inside a struct. However, this might take up more space in storage.

**Error (example) :**
```solidity
pragma solidity ^0.5.0;

interface LegalContract {
    
    address party1;
    address party2;
    
}
```

**Solution :**
```solidity
pragma solidity ^0.5.0;

interface LegalContract {
     
     struct Parties {
         address party1;
         address party2;
     } 
     
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Uninitialized constant variable.</pre>||_TypeChecker.cpp_, [Line ...](#)|

**Error (example) :**
```solidity
uint constant bitcoin_supply;
```

**Solution :**
```solidity
uint constant bitcoin_supply = 21000000;
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Initial value for constant variable has to be compile-time constant.</pre>||_TypeChecker.cpp_, [Line ...](#)|

You might need to enter a literal value in this context.

**Error (example) :**
```solidity
pragma solidity ^0.5.0;

contract Example {
    
    address constant _address1 = msg.sender;
    
}
```

**Solution :**
```solidity
pragma solidity ^0.5.0;

contract Example {
    
    address constant _address1 = 0xc0ffee254729296a45a3885639AC7E10F9d54979;
    
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Array is too large to be encoded.</pre>||_TypeChecker.cpp_, [Line ...](#)|

This error occurs when you define a fixed-size array that contains more than 134,217,727 indexes. This appear to be the maximum limit of a working storage. See the links below for related topic

- https://www-01.ibm.com/support/docview.wss?uid=swg21220835
- https://www.ibm.com/support/knowledgecenter/en/SS6SG3_4.2.0/com.ibm.entcobol.doc_4.2/MG/igymapxg001.htm
- http://ibmmainframes.com/about56564.html


**Error (example) :**
```solidity
pragma solidity ^0.5.0;

contract HugeArray {
    
    function notPossibleToEncode() public pure returns (bytes memory) {
        uint[134_217_728] memory large_array;
        return abi.encode(large_array); 
    }

}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Wrong argument count for [function-call / constructor-call]. [nb-of-arguments] arguments given but need at least / expected [nb-of-params-expected].</pre>||_TypeChecker.cpp_, [Line ...](#)|


**Error (example) :**
```solidity
function setTwoTx(address payable _address1, address payable _address2) public {
    _address1.transfer(10);
    _address2.transfer(10);
}
    
function sendTx(address _address) public {
    setTwoTx(_address);
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Invalid type for argument in function call. Invalid implicit conversion from [type1] to [type2] requested.</pre>||_TypeChecker.cpp_, [Line ...](#)|


**Error (example) :**
```solidity
pragma solidity ^0.5.0;

contract Example {
    
    string _age_in_string = "25";
    
    function HappyBirthday(uint8 _age) public pure returns (uint8) {
        return _age + 1;
    }
    
    function NotWorking() public pure {
        HappyBirthday(_age_in_string);
    }
}
```
-----



## _ContractLevelChecker.cpp_

|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **declarationError**|<pre>Another declaration is [constructor-location]. More than one constructor defined./pre>||_ContractLevelChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
constructor() public {
    owner = msg.sender;
}
    
constructor() public {
    map1[msg.sender] = 40;
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **findDuplicateDefinitions**|<pre>Function with same name and arguments defined twice.</pre>||_ContractLevelChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
function foo() public pure returns (string memory) {
    return "foo";
}
    
function foo() public pure returns (string memory) {
    return "bar";
}
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **findDuplicateDefinitions**|<pre>Event with same name and arguments defined twice.</pre>||_ContractLevelChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
event Deposit(address _address, uint _deposit);
event Deposit(address _address, uint _deposit);
```
-----


|**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError / overrideError**|<pre>Overriding function return types differ..</pre>||_ContractLevelChecker.cpp_, [Line ...](#)|

**Error (example) :**

```solidity
pragma solidity ^0.5.9;

interface Greetings {
    function hello() external pure returns (string memory);
    function goodbye() external pure returns (string memory);
}

contract FrenchContract is Greetings {
    
    function hello() external pure returns (string memory) {
        return "Bonjour";
   }

}


contract RobotContract is Greetings {
    
    function hello() external pure returns (uint) {
        return 1;
    }
    
}
```
-----


**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Constructor must be payable or non-payable, but is [state-mutability]</pre>||_ContractLevelChecker.cpp_, [Line ...](#)|

This error is diplayed when the keyword `view` or `pure` is attached to the constructor.

**Error (example) :**

```solidity
constructor(bytes32 _name) public view {}
```
-----


**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Constructor must be public or internal</pre>||_ContractLevelChecker.cpp_, [Line ...](#)|


**Error (example) :**

```solidity
constructor(bytes32 _name) private {}
```
-----


**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Library is not allowed to inherit</pre>||_ContractLevelChecker.cpp_, [Line 476](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/ContractLevelChecker.cpp#L476)|


**Error (example) :**

```solidity
library BritishMuseum {
    string constant name = "British Museum"; 
}

library MyLibrary is BritishMuseum {
    
}
```
-----


## _NameAndTypeResolver.cpp_

**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **fatalTypeError**|<pre>Definition of base has to precede definition of derived contract</pre>||_NameAndTypeResolver.cpp_, [Line 388 - 392](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L388-L389)|


**Error (example) :**

```solidity
pragma solidity ^0.5.0;

contract MyContract is MyLibrary {
    
    function encodeString(string memory sentence) public pure returns (bytes memory) {
        return abi.encode(sentence);
    }
    
    function decodeString(bytes memory data) public pure returns (string memory) {
        return abi.decode(data, (string));
    }
    
}

library MyLibrary {
    // interface code here
}
```
-----

## _ReferencesResolver.cpp_

**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **declarationError**|<pre>Undeclared identifier. [identifier-name]. Did you mean ...</pre><pre>[identifier-name] is not (or not yet) visible at this point</pre>||_ReferencesResolver.cpp_, [Line 99 - 111](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L99-L111)|


**Error (example) :**

```solidity
library MyLib {
    function testCall() external {
        test();
    }
    
    function test() public {
        testCall();
    }
}
```
-----


**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>Name has to refer to a struct, enum or contract</pre>||_ReferencesResolver.cpp_, [Line 190 - 194](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L190-L194)|


**Error (example) :**

```solidity
address constant _address1 = msg.sender;
    
function setTwoTx(_address1, address payable _address2) public {
    _address1.transfer(10);
    _address2.transfer(10);
}
    
function sendTx(address _address) public {
    setTwoTx(_address);
}
```
-----


## _ViewPureChecker.cpp_

**Error Type**|**Message**|**Compiler Version**|**Source**|
| --- | --- | --- | --- |
| **typeError**|<pre>"msg.value" or "callvalue()" appear here inside the modifier. [modifier-location]. This modifier uses "msg.value" or "callvalue()" and thus the function has to be payable or 
.</pre>||_ViewPureChecker.cpp_, [Line 283 - 288](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ViewPureChecker.cpp#L283-L288)|


**Error (example) :**

```solidity
modifier TxFee(uint _fee) {
    msg.value + _fee;
    _;
}
    
function sendEther(address payable _recipient, uint _ether) public TxFee(10) {
    _recipient.transfer(_ether);
}
```
-----

