# Solidity Compiler Debug List
A curated list of all the Errors and Warning from the Solidity Compiler.

This repository aims to help Smart Contracts Developers on Ethereum to debug their Solidity files. It offers a list of all the errors and warnings you can possibly encounter when developing with the Remix IDE or during compilation with `solc`.

The list is based on the official C++ source code from the Solidity compiler. The errors and warning messages are all mentioned in the files located in the folder [`libsolidity/analysis`](https://github.com/ethereum/solidity/tree/develop/libsolidity/analysis). 

All the errors and warnings listed below are referenced to the original source file + line number for better analysis. Some explanations and solutions are provided to help debugging.

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


# Everything below need to be cleaned up

**Warnings**

|   | **Warning** | **Source code file + line** | **Other references** | **Code example** |
| --- | --- | --- | --- | --- |
| warning  | Unused function parameter. Remove or comment out the variable name to silence this warning.  | StaticAnalyzer.cpp[Line 124](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/StaticAnalyzer.cpp#L124) |   | function sayHello(string memory \_name, uint8 \_age) publicpurereturns (string memory) {        string memory myName = \_name;        return myName;    }  |
| warning  | Unused local variable.  | StaticAnalyzer.cpp[Line 127](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/StaticAnalyzer.cpp#L127) |   | function birthday(uint8 \_age) publicpurereturns (uint8) {        uint8 new\_age = \_age++;        return \_age++;    }  |
| warning  | Variable covers a large part of storage and thus makes collision likely. Either use mappings or dynamic arrays and allow their size to be increased only in small quantities per transaction  | StaticAnalyzer.cpp[Line 158 - 164](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/StaticAnalyzer.cpp#L158-L164) | [_https://ethereum.stackexchange.com/questions/49278/initializing-full-array-warning_](https://ethereum.stackexchange.com/questions/49278/initializing-full-array-warning) |   |
| warning  | Statement has no effect | StaticAnalyzer.cpp[Line 185](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/StaticAnalyzer.cpp#L185) |   |   |
| warning  | The constructor of the contract (or its base) uses inline assembly. Because of that, it might be that the deployed bytecode is different from type(...).runtimeCode.  | StaticAnalyzer.cpp[Line 213-214](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/StaticAnalyzer.cpp#L213-L214) |   |   |
| warning  | \&quot;this\&quot; used in constructor. Note that external functions of a contract. cannot be called while it is being constructed.&quot;  | StaticAnalyzer.cpp[Line 234-240](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/StaticAnalyzer.cpp#L234-240) |   | constructor(uint \_ether) public {        this.sendEther(\_ether);    }        function sendEther(uint \_ether) external {        address(this).balance + \_ether;        msg.sender.balance - \_ether;    }  |





_See here :_

**Errors**

| **Type** | **Message** | **Source code file + line** | **Other references** | **Code example** |
| --- | --- | --- | --- | --- |
| typeError  | \&quot;msg.gas\&quot; has been deprecated in favor of \&quot;gasleft()\&quot; | StaticAnalyzer.cpp[line 195-199](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L195-L199) |   |   |
| typeError  | \&quot;block.blockhash()\&quot; has been deprecated in favor of \&quot;blockhash()\&quot; | StaticAnalyzer.cpp[Lines 200-204](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L200-L204) |   |   |
| typeError  | \&quot;callcode\&quot; has been deprecated in favour of \&quot;delegatecall\&quot;. | StaticAnalyzer.cpp[Line 219 - 225](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L219-L225) |   |   |
| **typeError** | Division by zero.&quot; : &quot;Modulo zero. | StaticAnalyzer.cpp[Line 284-288](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L284-L288) |   | function divide(uint \_number) publicpure {        uint result = \_number / 0;    }  |
| **typeError** | Arithmetic modulo zero. | StaticAnalyzer.cpp[Line 306-310](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L306-L310) |   |   |
| **typeError** | The function declaration is here: \&lt;functionType-\&gt;declaration().scope()-\&gt;location()\&gt; Libraries cannot call their own functions externally. | StaticAnalyzer.cpp[Line 312-324](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/StaticAnalyzer.cpp#L312-L324) | Since Compiler version 0.5.9 [https://github.com/ethereum/solidity/issues/6451](https://github.com/ethereum/solidity/issues/6451) **https://github.com/ethereum/solidity/pull/6604/files** | pragmasolidity 0.5.9; library L {    using L for \*;        function f() publicpurereturns (uint r) {         return r.g();    }        function g(uint) publicpurereturns (uint) {         return2;     }}  |













**SyntaxChecker.cpp**

The files related to the syntax checker are the SyntaxChecker.h, but most importantly /liblangutil/Token.h

|   |   |   |   |
| --- | --- | --- | --- |
| SyntaxError | Source file does not specify required compiler version! Consider adding &quot;pragma solidity&quot;  | [Line 57 - 72](https://github.com/ethereum/solidity/blob/2ee272acf32fbad4efd1da7919c59792597ce9e6/libsolidity/analysis/SyntaxChecker.cpp#L57-L72) | \&gt; error occurs herecontract NewHello{    // contract code here}  |
| SyntaxError | Invalid pragma | SyntaxChecker.cpp |   |
| SyntaxError | Experimental feature name is missing  | [Line 86 - 90](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L86-L90) | pragma experimental; contract MyContract {    // Write your code here}  |
| SyntaxError | Stray arguments  | [Line 91 - 95](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L91-L95) | pragmaexperimental ABIEncoderV2 nextGen; contract MyContract {    // Write your code here}  |
| SyntaxError | Empty experimental feature name is invalid.  | SyntaxChecker.cpp |   |
| SyntaxError | Unsupported experimental feature name. | SyntaxChecker.cpp |   |
| SyntaxError | Duplicate experimental feature name. | [Line 103 - 104](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L103-L104) | pragmaexperimental SMTChecker;pragmaexperimental SMTChecker; contract MyContract {    // Write your code here}  |
| Warning | Duplicate experimental feature name. | SyntaxChecker.cpp |   |
| SyntaxError | &quot;Source file requires different compiler version (current compiler is &quot; + string(VersionString) + &quot; - note that nightly builds are considered to be strictly less than the released version&quot; | SyntaxChecker.cpp |   |
| SyntaxError | Unknown pragma | SyntaxChecker.cpp |   |
| SyntaxError | Modifier body does not contain &#39;\_&#39;  | [Line 143 - 144](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L143-L144) | pragmasolidity ^0.5.9; contract MyContract {        modifier Fee {        require (msg.value != 0);    }}  |
| SyntaxError | Variable declarations can only be used inside blocks.  | [Line 151 - 152](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L151-L152) | pragmasolidity ^0.5.9; contract MyContract {        uint a = 3;    uint b = 2;        function test() public {                for (uint x = 1; x \&lt;= 10; x++) uint c = a + b + x;            }    }  |
| SyntaxError | \&quot;continue\&quot; has to be in a \&quot;for\&quot; or \&quot;while\&quot; loop. | [Line 189 - 191](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L189-L191) | pragmasolidity ^0.5.9; contract MyContract {        uint a = 3;    uint b = 2;        function test(uint x) public {        if ( x \&lt; a &amp;&amp; x \&gt; b) {            bool result = true;            continue;        }    }    }  |
| SyntaxError | \&quot;break\&quot; has to be in a \&quot;for\&quot; or \&quot;while\&quot; loop. | Line 197 - 199 | pragmasolidity ^0.5.9; contract MyContract {        uint a = 3;    uint b = 2;        function test(uint x) public {        if ( x \&lt; a &amp;&amp; x \&gt; b) {            bool result = true;            break;        }    }    }  |
| SyntaxError | \&quot;throw\&quot; is deprecated in favour of \&quot;revert()\&quot;, \&quot;require()\&quot; and \&quot;assert()\&quot; | SyntaxChecker.cpp |   |
| SyntaxError | Invalid use of underscores in number literal. No trailing underscores allowed. | SyntaxChecker.cpp |        uint constant bitcoin\_supply = 21\_000\_000\_;    |
| SyntaxError | Invalid use of underscores in number literal. Only one consecutive underscores between digits allowed. | SyntaxChecker.cpp |         uint constant bitcoin\_supply = 21\_000\_\_000;      |
| SyntaxError | Invalid use of underscores in number literal. No underscores in front of the fraction part allowed. | [Lines 236 - 237](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L236-L237) |     fixed constant test = 10\_000\_.5;  |
| SyntaxError | Invalid use of underscores in number literal. No underscores in front of the fraction part allowed. | [Lines 239 - 240](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/SyntaxChecker.cpp#L239-L240) |     fixed constant test = 10\_000.\_5;  |
| SyntaxError | Invalid use of underscores in number literal. No underscore at the end of the mantissa allowed. | SyntaxChecker.cpp |   |
| SyntaxError | Invalid use of underscores in number literal. No underscore in front of exponent allowed. | SyntaxChecker.cpp |   |
| SyntaxError | Use of unary + is disallowed. | SyntaxChecker.cpp |   |
| SyntaxError | The msize instruction cannot be used when the Yul optimizer is activated because it can change its semantics. Either disable the Yul optimizer or do not use the instruction. | SyntaxChecker.cpp |   |
| SyntaxError | Functions are not allowed to have the same name as the contract. If you intend this to be a constructor, use \&quot;constructor(...) { ... }\&quot; to define it. | SyntaxChecker.cpp | pragmasolidity ^0.5.9; contract MyContract {        function MyContract() public {        // do something    }    }  |
| SyntaxError | No visibility specified. Did you intend to add \&quot;&quot; + suggestedVisibility + &quot;\&quot;? | SyntaxChecker.cpp |    function test() {        // do something    }  |
| SyntaxError | Functions without implementation cannot have modifiers. | SyntaxChecker.cpp |     modifier TxFee(uint \_fee) {        msg.value + \_fee;        \_;    }        function sendEther() public TxFee;  |
| Warning | Naming function type parameters is deprecated. | SyntaxChecker.cpp |   |
| SyntaxError | Return parameters in function types may not be named. | SyntaxChecker.cpp |   |
| SyntaxError | The use of the \&quot;var\&quot; keyword is disallowed. The declaration part of the statement can be removed, since it is empty | SyntaxChecker.cpp |   |
| SyntaxError | Defining empty structs is disallowed. | SyntaxChecker.cpp |     struct Unknown {       // Struct definition here     }  |



**TypeChecker.cpp (really long, 2500 lines of code)**

| **Error Type** | **Message** | **Source File** |   |   |
| --- | --- | --- | --- | --- |
|   | Type requested but not present. |   |   |   |
| warning | &quot;This assignment performs two copies to storage. Since storage copies do not first copy to a temporary location, one of them might be overwritten before the second is executed and thus may have unexpected effects. It is safer to perform the copies separately or assign to storage pointers first.&quot; |   |   |   |
| typeError  | &quot;This function takes two arguments, but &quot; + toString(arguments.size()) + &quot; were provided.&quot; |   |   |   |
| typeErrorConcatenateDescriptions | &quot;Invalid type for argument in function call. Invalid implicit conversion from &quot; + type(\*arguments.front())-\&gt;toString() + &quot; to bytes memory requested.&quot;, result.message() |   |   |   |
| typeError  | &quot;The second argument to \&quot;abi.decode\&quot; has to be a tuple of types.&quot; |   |   |     function decodeNb(bytes memory data) publicpurereturns (uint8) {        uint8 nb;        nb = abi.decode(data, uint8);        return nb;    }  |
| typeError  | &quot;Decoding type &quot; + actualType-\&gt;toString(false) + &quot; not supported.&quot; |   |   |   |
| typeError  | &quot;Argument has to be a type name.&quot; |   |   |   |
| typeError  | &quot;This function takes one argument, but &quot; + toString(arguments.size()) + &quot; were provided.&quot; | [Line 209](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/TypeChecker.cpp#L209) |   |   |
| typeError | &quot;Invalid type for argument in function call. Contract type required, but &quot; +type(\*arguments.front())-\&gt;toString(true) +&quot; provided.&quot; |   |   |   |
| solAssert | Base contract not available. |   |   |   |
| typeError | Interfaces cannot inherit. |   |   | pragmasolidity ^0.5.0; contract MyContract {        function encodeString(string memory sentence) publicpurereturns (bytes memory) {        return abi.encode(sentence);    }        function decodeString(bytes memory data) publicpurereturns (string memory) {        return abi.decode(data, (string));    }    } interface MyInterface is MyContract {    // interface code here}  |
| typeError | Libraries cannot be inherited from. |   |   | pragmasolidity ^0.5.0; library MyLibrary {    // interface code here} contract MyContract is MyLibrary {        function encodeString(string memory sentence) publicpurereturns (bytes memory) {        return abi.encode(sentence);    }        function decodeString(bytes memory data) publicpurereturns (string memory) {        return abi.decode(data, (string));    }    }  |
| typeError | &quot;Wrong argument count for constructor call: &quot; + toString(arguments-\&gt;size()) + &quot; arguments given but expected &quot; + toString(parameterTypes.size()) + &quot;. Remove parentheses if you do not want to provide arguments here.&quot; |   |   |   |
| typeErrorConcatenateDescriptions | &quot;Invalid type for argument in constructor call. Invalid implicit conversion from &quot; + type(\*(\*arguments)[i])-\&gt;toString() + &quot; to &quot; + parameterTypes[i]-\&gt;toString() + &quot; requested.&quot;, |   |   |   |
| fatalTypeError | Library name expected. |   |   |   |
| solAssert | Type cannot be used in struct. |   |   |   |
| fatalDeclarationError | Struct definition exhausting cyclic dependency validator. |   |   |   |
| fatalTypeError | Recursive struct definition. |   |   |     struct FootballPlayer {        string name;        uint8 age;        FootballPlayer mentor;    }  |
| typeError | Library functions cannot be payable. |   |   |   |
| typeError | Internal functions cannot be payable. |   |   |   |
| typeError | Mapping types can only have a data location of \&quot;storage\&quot; and thus only be parameters or return variables for internal or library functions. |   |   |   |
| typeError | Mapping types can only have a data location of \&quot;storage\&quot;. |   |   |   |
| TypeError | Mapping types for parameters or return variables can only be used in internal or library functions. |   |   |   |
| typeError | Type is required to live outside storage. |   |   |   |
| solAssert | Expected detailed error message! |   |   |   |
| typeError | This type is only supported in the new experimental ABI encoder.Use \&quot;pragma experimental ABIEncoderV2;\&quot; to enable the feature. |   |   |   |
| declarationError | Base constructor already provided. |   |   |   |
| typeError | Functions in interfaces cannot have an implementation. |   |   |   |
| typeError | Functions in interfaces must be declared external. |   |   |   |
| typeError | Constructor cannot be defined in interfaces. |   |   |   |
| typeError | Constructor cannot be defined in libraries. |   |   |   |
| typeError | Constructor must be implemented if declared. |   |   |   |
| typeError | Internal library function must be implemented if declared. |   |   |   **Wrong :**  library Messenger {        function testConnection() internal pure returns(bool);    }  **Right :**  library Messenger {        function testConnection() public pure returns(bool);    } |
| typeError | Variables cannot be declared in interfaces. |   |   |   |
| solAssert | Variable type not provided. |   |   |   |
| typeError | Constants of non-value type not yet implemented. |   |   |   |
| typeError | Uninitialized \&quot;constant\&quot; variable. |   |   |     uint constant bitcoin\_supply;  |
| typeError | Initial value for constant variable has to be compile-time constant. |   |   |   |
| typeError | &quot;Type &quot; + varType-\&gt;toString() + &quot; is only valid in storage.&quot; |   |   |   |
| typeError | &quot;The following types are only supported for getters in the new experimental ABI encoder: &quot; +oinHumanReadable(unsupportedTypes) +&quot;. Either remove \&quot;public\&quot; or use \&quot;pragma experimental ABIEncoderV2;\&quot; to enable the feature.&quot; |   |   |   |
| typeError | Internal or recursive type is not allowed for public state variables. |   |   |   |
| typeError | Array is too large to be encoded. |   |   |   |
| typeError | Referenced declaration is neither modifier nor base class. |   |   |   |
| typeError | &quot;Wrong argument count for modifier invocation: &quot; + toString(arguments.size()) + &quot; arguments given but expected &quot; + toString(parameters-\&gt;size()) + &quot;.&quot; |   |   |   |
| typeErrorConcatenateDescriptions | &quot;Invalid type for argument in modifier invocation. &quot; &quot;Invalid implicit conversion from &quot; + type(\*arguments[i])-\&gt;toString() + &quot; to &quot; + type(\*(\*parameters)[i])-\&gt;toString() + &quot; requested.&quot; |   |   |   |
| typeError | Type is required to live outside storage. |   |   |   |
| typeError | Internal or recursive type is not allowed as event parameter type. |   |   |   |
| typeError | &quot;This type is only supported in the new experimental ABI encoder. Use \&quot;pragma experimental ABIEncoderV2;\&quot; to enable the feature.&quot; |   |   |   |
| typeError | More than 4 indexed arguments for anonymous event. |   |   |   |
| typeError | More than 3 indexed arguments for event. |   |   |   |
| solAssert | External function type uses internal types. |   |   |   |
| solAssert | Expected variable type! |   |   |   |
| typeError | Constant variables not supported by inline assembly. |   |   |   |
| typeError | The suffixes \_offset and \_slot can only be used on storage variables. |   |   |   |
| typeError | Storage variables cannot be assigned to. |   |   |   |
| typeError | Only local variables are supported. To access storage variables, use the \_slot and \_offset suffixes. |   |   |   |
| typeError | You have to use the \_slot or \_offset suffix to access storage reference variables. |   |   |   |
| typeError | Call data elements cannot be accessed directly. Copy to a local variable first or use \&quot;calldataload\&quot; or \&quot;calldatacopy\&quot; with manually determined offsets and sizes. |   |   |   |
| typeError | Only types that use one stack slot are supported. |   |   |   |
| typeError | The suffixes \_offset and \_slot can only be used on storage variables. |   |   |   |
| typeError | Only local variables can be assigned to in inline assembly. | [**Line 676**](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/TypeChecker.cpp#L676) |   |   |
| solAssert  | Type of declaration required but not yet determined. |   |   |   |
| typeError | Expected a library. |   |   |   |
| typeError | Return arguments required |   |   |   |
| typeError  | Return arguments not allowed. |   |   |   |
| typeError | Different number of arguments in return statement than in returns declaration. |   | **Here, the case is if we have multiple return value declared in the function head.** Here we look at how many components are mentioned in the tuple in the final return statement.We compare if this number **is different than the number of arguments mentioned in the return**  **declaration** (function head) |   |
| typeErrorConcatenateDescriptions | &quot;Return argument type &quot; + type(\*\_return.expression())-\&gt;toString() + &quot; is not implicitly convertible to expected type &quot; + TupleType(returnTypes).toString(false) + &quot;.&quot;, result.message() |   |   |   |
| typeError | Different number of arguments in return statement than in returns declaration.&quot; |   | **Here, the case is if we have only one return value declared in the function head.** Here we look at how many components are mentioned in the tuple in the final return statement.
We check **if this number is not equal to one.** |   |
| typeErrorConcatenateDescriptions  | &quot;Return argument type &quot; + type(\*\_return.expression())-\&gt;toString() + &quot; is not implicitly convertible to expected type (type of first return variable) &quot; + expected-\&gt;toString() + &quot;.&quot;, result.message() |   |   |   |
| typeError | Expression has to be an event invocation. |   |   |   |
| fatalTypeError | Use of the \&quot;var\&quot; keyword is disallowed. | [Line 874](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/TypeChecker.cpp#L874)[Line 879](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/TypeChecker.cpp#L879) |   |   |
| solAssert | Uninitialized storage pointer.Expected a specified location at this point |   |   |   |
| typeError | Uninitialized mapping. Mappings cannot be created dynamically, you have to assign them from a state variable. |   |   |   |
| typeError | &quot;Different number of components on the left hand side (&quot; + toString(variables.size()) + &quot;) than on the right hand side (&quot; + toString(valueTypes.size()) + &quot;).&quot; |   |   |   |
| solAssert | Value has to be tied to statement |   |   |   |
| fatalTypeError | &quot;Invalid rational &quot; +valueComponentType-\&gt;toString() +&quot; (absolute value too large or division by zero).&quot; |   |   |   |
| solAssert | Cannot declare variable with void (empty tuple) type. |   |   |   |
| solAssert | &quot;, which can hold values between &quot; + minValue + &quot; and &quot; + maxValue; | [**Line 974**](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/TypeChecker.cpp#L974) | **Not clear, need to deep dive and understand what this does** |   |
| solAssert | Unknown type. |   |   |   |
| typeError | &quot;Type &quot; + valueComponentType-\&gt;toString() + &quot; is not implicitly convertible to expected type &quot; + var.annotation().type-\&gt;toString(); + &quot;, but it can be explicitly converted.&quot; |   |   |   |
| typeError | &quot;Type &quot; + valueComponentType-\&gt;toString() + &quot; is not implicitly convertible to expected type &quot; + var.annotation().type-\&gt;toString(); + &quot;. Try converting to type &quot; + valueComponentType-\&gt;mobileType()-\&gt;toString() + &quot; or use an explicit conversion.&quot; |   |   |   |
| solAssert | Should have errors! |   |   |   |
| syntaxError | &quot;Use of the \&quot;var\&quot; keyword is disallowed. Type cannot be expressed in syntax.&quot; |   |   |   |
| syntaxError | &quot;Use of the \&quot;var\&quot; keyword is disallowed. Use explicit declaration `&quot; + createTupleDecl(variables) + &quot; = ...´ instead.&quot; |   |   |   |
| typeError | Invalid rational number. |   |   |   |
| warning | Return value of low-level calls not used. |   |   |   |
| warning | Failure condition of &#39;send&#39; ignored. Consider using &#39;transfer&#39; instead. |   |   |   |
| typeError | Invalid mobile type in true expression. |   |   |   |
| typeError | Invalid mobile type in false expression. |   |   |   |
| typeError | &quot;True expression&#39;s type &quot; + trueType-\&gt;toString() + &quot; doesn&#39;t match false expression&#39;s type &quot; + falseType-\&gt;toString() + &quot;.&quot; |   |   |   |
| typeError | Conditional expression as left value is not supported yet. |   | **Related to LValue** |   |
| solAssert | Array sizes don&#39;t match or no errors generated. |   |   |   |
| typeError | Mappings cannot be assigned to. |   |   |   |
| typeError | Compound assignment is not allowed for tuple types. |   |   |   |
| typeError | &quot;Operator &quot; + string(TokenTraits::toString(\_assignment.assignmentOperator())) + &quot; not compatible with types &quot; + t-\&gt;toString() + &quot; and &quot; + type(\_assignment.rightHandSide())-\&gt;toString() |   |   |   |
| fatalTypeError | Inline array type cannot be declared as LValue. |   |   |   |
| fatalTypeError | Tuple component cannot be empty. |   |   |   |
| fatalTypeError | Array component cannot be empty. |   |   |   |
| typeError | Tuple component cannot be empty. |   |   |   |
| fatalTypeError | Invalid rational number. |   |   |   |
| solAssert | Inline array cannot have empty components |   |   |   |
| fatalTypeError | Invalid mobile type. |   |   |   |
| fatalTypeError | Unable to deduce common type for array elements. |   |   |   |
| fatalTypeError | &quot;Type &quot; + inlineArrayType-\&gt;toString() + &quot; is only valid in storage.&quot; |   |   |   |
| typeError | &quot;Unary operator &quot; + string(TokenTraits::toString(op)) + &quot; cannot be applied to type &quot; + subExprType-\&gt;toString() |   |   |   |
| typeError | &quot;Operator &quot; + string(TokenTraits::toString(\_operation.getOperator())) + &quot; not compatible with types &quot; + leftType-\&gt;toString() + &quot; and &quot; + rightType-\&gt;toString() + (!result.message().empty() ? &quot;. &quot; + result.message() : &quot;&quot;) |   |   |   |
| warning  | &quot;Result of exponentiation / shift has type &quot; + commonType-\&gt;toString() + &quot; and thus might overflow. Silence this warning by converting the literal to the expected type.&quot; |   |   |   |
| typeError | Exactly one argument expected for explicit type conversion. |   |   |   |
| typeError | Type conversion cannot allow named arguments. |   |   |   |
| solAssert | Invalid explicit conversion to storage type. |   |   |   |
| ??? | Did you mean to declare this variable as &quot;address payable&quot;? |   |   |   |
| typeError | &quot;Explicit type conversion not allowed from non-payable \&quot;address\&quot; to \&quot;&quot; + resultType-\&gt;toString() + &quot;\&quot;, which has a payable fallback function.&quot; |   |   |   |
| typeError | &quot;Explicit type conversion not allowed from \&quot;&quot; + argType-\&gt;toString() + &quot;\&quot; to \&quot;&quot; + resultType-\&gt;toString() + &quot;\&quot;.&quot; |   |   |   |
| typeError | &quot;staticcall&quot; is not supported by the VM version. |   |   |   |
| typeError | Event invocations have to be prefixed by \&quot;emit\&quot;. |   |   |   |
| solAssert | ABI function has unexpected FunctionType::Kind. |   | From here, ABI related |   |
| solAssert | ABI functions should be variadic. |   |   |   |
| solAssert | ABI function with unexpected padding |   |   |   |
| typeError | Named arguments cannot be used for functions that take arbitrary parameters. |   |   |   |
| typeError | Fractional numbers cannot yet be encoded. |   |   |   |
| typeError | Invalid rational number (too large or division by zero). |   |   |   |
| typeError | Cannot perform packed encoding for a literal. Please convert it to an explicit type first. |   |   |   |
| typeError | Type not supported in packed mode. |   |   |   |
| typeError | This type cannot be encoded. |   |   |   |
| solAssert | Struct constructor calls cannot be variadic. |   | From here, general checks |   |
| typeError | &quot;Need at least &quot; + toString(parameterTypes.size()) + &quot; arguments for &quot; + string(isStructConstructorCall ? &quot;struct constructor&quot; : &quot;function call&quot;) + &quot;, but provided only &quot; + toString(arguments.size()) + &quot;.&quot; |   |   |   |
| typeError | &quot;Wrong argument count for &quot; + string(isStructConstructorCall ? &quot;struct constructor&quot; : &quot;function call&quot;) + &quot;: &quot; + toString(arguments.size()) + &quot; arguments given but &quot; + string(isVariadic ? &quot;need at least &quot; : &quot;expected &quot;) + toString(parameterTypes.size()) + &quot;.&quot;; |   |   |   |
| typeError | Members that have to be skipped in memory: \&lt;member\&gt; |   |   |   |
| typeError | &quot; This function requires a single bytes argument. Use \&quot;\&quot; as argument to provide empty calldata.&quot; |   |   |   |
| typeError | &quot; This function requires a single bytes argument. If all your arguments are value types, you can use abi.encode(...) to properly generate it.&quot; |   |   |   |
| typeError | &quot; This function requires a single bytes argument. Use abi.encodePacked(...) to obtain the pre-0.5.0 behaviour or abi.encode(...) to use ABI encoding.&quot; |   |   |   |
| solAssert | Unexpected parameter length mismatch! |   |   |   |
| typeError | &quot;Duplicate named argument \&quot;&quot; + \*argumentNames[i] + &quot;\&quot;.&quot; |   |   |   |
| typeError | &quot;Named argument \*argumentNames[i] does not match function declaration.&quot; |   |   |   |
| solAssert | unmapped parameter |   |   |   |
| typeError  | &quot;Invalid type for argument in function call. Invalid implicit conversion from \&lt;type(\*paramArgMap[i])-\&gt;toString()\&gt; to \&lt;parameterTypes[i]-\&gt;toString()\&gt; requested.&quot; |   |   |   |
| typeError  | &quot; This function requires a single bytes argument. If all your arguments are value types, you can use abi.encode(...) to properly generate it.&quot; |   |   |   |
| typeError  | &quot; This function requires a single bytes argument. Use abi.encodePacked(...) to obtain the pre-0.5.0 behaviour or abi.encode(...) to use ABI encoding.&quot; |   |   |   |
| typeError | Type is not callable |   | **?????** |   |
| solAssert | Type name not resolved. |   |   |   |
| fatalTypeError  | Identifier is not a contract. |   |   |   |
| fatalTypeError  | Cannot instantiate an interface. |   |   |   |
| typeError  | Missing implementation: function-\&gt;location(). Trying to create an instance of an abstract contract.  |   |   |   |
| typeError | Contract with internal constructor cannot be created directly. |   |   |   |
| solAssert | Linearized base contracts not yet available. |   |   |   |
| typeError | Circular reference for contract creation (cannot create instance of derived or same contract). |   |   |   |
| fatalTypeError | Type cannot live outside storage. |   |   |   |
| typeError | Length has to be placed in parentheses after the array type for new expression. |   |   |   |
| fatalTypeError | Contract or array type expected. |   |   |   |
| fatalTypeError | Member \&lt;memberName\&gt; is not available in \&lt;exprType-\&gt;toString()\&gt; outside of storage. |   |   |   |
| ??? | Member \&lt;memberName\&gt; not found or not visible after argument-dependent lookup in \&lt;exprType-\&gt;toString()\&gt;. Did you intend to call the function? |   |   |   |
|   | Constructor for &quot; + t.front()-\&gt;toString() + &quot; must be payable for member \&quot;value\&quot; to be available. |   |   |   |
|   | &quot;Member \&quot;value\&quot; is not allowed in delegated calls due to \&quot;msg.value\&quot; persisting.&quot; |   |   |   |
|   | &quot;Member \&quot;value\&quot; is only available for payable functions.&quot; |   |   |   |
|   | Member \&lt;memberName\&gt; not found or not visible after argument-dependent lookup in \&lt;exprType-\&gt;toString()\&gt;. Use \&quot;address(&quot; + varName + &quot;).&quot; + memberName + &quot;\&quot; to access this address member. |   |   |   |
| solAssert | Expected address not-payable as members were not found |   |   |   |
|   | &quot;send&quot; and &quot;transfer&quot; are only available for objects of type &quot;address payable&quot;, not \&lt;exprType-\&gt;toString()\&gt;&quot;.  |   |   |   |
| fatalTypeError  | Member \&lt;memberName\&gt; not unique after argument-dependent lookup in \&lt;exprType-\&gt;toString()\&gt; - did you forget the &quot;payable&quot; modifier? |   | [**Line 2116-2118**](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/TypeChecker.cpp#L2116) |   |
| fatalTypeError  | Member \&lt;memberName\&gt; not unique after argument-dependent lookup in \&lt;exprType-\&gt;toString |   | [**Line 2116-2118**](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/TypeChecker.cpp#L2116) |   |
| solAssert | Function \&lt;memberName\&gt; cannot be called on an object of type \&lt;exprType-\&gt;toString()\&gt; (expected \&lt;funType-\&gt;selfType()-\&gt;toString()\&gt;). |   |   |   |
| typeError | Circular reference for contract code access. |   |   |   |
| typeError | Index expression cannot be omitted. |   |   |   |
| typeError | Index access for string is not possible. |   |   |   |
| typeError | Out of bounds array access. |   |   |   |
| typeError | Index expression cannot be omitted. |   |   |   |
| typeError | Index access for contracts or libraries is not possible. |   |   |   |
| fatalTypeError | Integer constant expected. |   |   |   |
| solAssert | Expected errors as expectType returned false |   |   |   |
| typeError | Index expression cannot be omitted. |   |   |   |
| fatalTypeError | Index expression cannot be represented as an unsigned integer. |   |   |   |
| typeError | Out of bounds array access. |   |   |   |
| fatalTypeError | Indexed expression has to be a type, mapping or array (is \&lt;baseType-\&gt;toString()\&gt;) |   |   |   |
| fatalTypeError | No matching declaration found after variable lookup. |   |   |   |
| fatalTypeError | No unique declaration found after variable lookup. |   |   |   |
| fatalTypeError | No candidates for overload resolution found. |   |   |   |
| solAssert | Requested type not present. |   |   |   |
| fatalTypeError | No matching declaration found after argument-dependent lookup. |   |   |   |
| fatalTypeError | No unique declaration found after argument-dependent lookup. |   |   |   |
| solAssert | Referenced declaration is null after overload resolution. |   |   |   |
| fatalTypeError | Declaration referenced before type could be determined. |   |   |   |
| typeError | &quot;sha3&quot; has been deprecated in favour of &quot;keccak256&quot;  |   |   |   |
| typeError | &quot;suicide&quot; has been deprecated in favour of &quot;selfdestruct&quot; |   |   |   |
| syntaxError | &quot;This looks like an address but is not exactly 40 hex digits. It is \&lt;to\_string(\_literal.valueWithoutUnderscores().length() - 2)\&gt; hex digits.&quot; |   |   |   |
| syntaxError | This looks like an address but has an invalid checksum.Correct checksummed address: \&lt;\_literal.getChecksummedAddress\&gt;.  |   |   |   |
| syntaxError | If this is not used as an address, please prepend &#39;00&#39;For more information please see https://solidity.readthedocs.io/en/develop/types.html#address-literals | [**Line 2400**](https://github.com/ethereum/solidity/blob/1cc8475309dd1ae36436b0a5cb2285de0e679a35/libsolidity/analysis/TypeChecker.cpp#L2400) |   |   |
| fatalTypeError | Hexadecimal numbers cannot be used with unit denominations.You can use an expression of the form &quot;0x1234 \* 1 day&quot; instead.  |   |   |   |
| typeError | Using \&quot;years\&quot; as a unit denomination is deprecated. |   |   |   |
| fatalTypeError | Invalid literal value. |   |   |   |
| solAssert | Declaration not stored. |   |   |   |
| solAssert | Declaration not stored. |   |   |   |
| typeError | Type \&lt;type(\_expression)-\&gt;toString()\&gt; is not implicitly convertible to expected type \&lt;\_expectedType.toString()\&gt;, but it can be explicitly converted. |   |   |   |
| typeError | Type \&lt;type(\_expression)-\&gt;toString()\&gt; is not implicitly convertible to expected type \&lt;\_expectedType.toString()\&gt;. Try converting to type \&lt;type(\_expression)-\&gt;mobileType()-\&gt;toString()\&gt; or use an explicit conversion. |   |   |   |
| typeError | Cannot assign to a constant variable. |   |   |   |
| typeError | Expression has to be an lvalue. |   |   |   |



**ConstantEvaluator.cpp**

| **Error Type** | **Message** | **Source File** | **Description** |   |
| --- | --- | --- | --- | --- |
| fatalTypeError  | &quot;Operator &quot; + string(TokenTraits::toString(\_operation.getOperator())) + &quot; not compatible with types &quot; + left-\&gt;toString() + &quot; and &quot; + right-\&gt;toString() | [Line 48-56](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ConstantEvaluator.cpp#L48-L56) |   |   |
| fatalTypeError  | Cyclic constant definition (or maximum recursion depth exhausted). | [Line 84-85](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ConstantEvaluator.cpp#L84-L85) | Happen if we go over 32 bytes. (for m\_depth, I am not sure what it is tho…) |   |



**ContractLevelChecker.cpp**

| **Error Type** | **Message** | **Source File** | **Example** |
| --- | --- | --- | --- |
| declarationError  | &quot;Another declaration is here:&quot;, constructor-\&gt;location()), &quot;More than one constructor defined.&quot; |   | constructor() public {        owner = msg.sender;    }        constructor() public {        map1[msg.sender] = 40;    } |
| declarationError  | &quot;Another declaration is here:&quot;, fallback-\&gt;location()), &quot;Only one fallback function is allowed.&quot; |   | constructor() public {        owner = msg.sender;    }        constructor() public {        map1[msg.sender] = 40;    } |
| findDuplicateDefinitions | Function with same name and arguments defined twice. |   | function foo() public pure returns (string memory) {        return &quot;foo&quot;;    }        function foo() public pure returns (string memory) {        return &quot;bar&quot;;    } |
| findDuplicateDefinitions | Event with same name and arguments defined twice. |   | event Deposit(address \_address, uint \_deposit);    event Deposit(address \_address, uint \_deposit); |
|   | Other declaration is here:&quot;, overloads[j]-\&gt;location() **This applies to anything in general (functions, events…)** | [Line 118](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/ContractLevelChecker.cpp#L118) | function foo() public pure returns (string memory) {        return &quot;foo&quot;;    }        function foo() public pure returns (string memory) {        return &quot;bar&quot;;    } |
| typeError | Override changes modifier signature. |   |   |
| typeError | Override changes modifier to function. |   |   |
| typeError / overrideError | Overriding function return types differ. |   | interface Greetings {    function hello() external pure returns (string memory);    function goodbye() external pure returns (string memory);} contract FrenchContract is Greetings {        function hello() external pure returns (string memory) {        return &quot;Bonjour&quot;;   } }  contract RobotContract is Greetings {        function hello() external pure returns (uint) {        return 1;    }    } |
| typeError / overrideError | Overriding function visibility differs. |   |   |
| typeError / overrideError  | &quot;Overriding function changes state mutability from \&quot;&quot; + stateMutabilityToString(\_super.stateMutability()) + &quot;\&quot; to \&quot;&quot; + stateMutabilityToString(\_function.stateMutability()) + &quot;\&quot;.&quot; |   |   |
| typeError | Overridden function is here:&quot;, super.location() |   |   |
| typeError | Redeclaring an already implemented function as abstract |   |   |
| declarationError | Modifier-style base constructor call without arguments. |   |   |
|   | Second constructor call is here:&quot;, \_argumentNode-\&gt;location() |   |   |
|   | First constructor call is here: &quot;, \_argumentNode-\&gt;location() |   |   |
|   | Second constructor call is here: &quot;, previousNode-\&gt;location() |   |   |
|   | Base constructor arguments given twice. |   |   |
| typeError | Non-empty \&quot;returns\&quot; directive for constructor. |   |   |
| typeError | &quot;Constructor must be payable or non-payable, but is \&quot;&quot; + stateMutabilityToString(constructor-\&gt;stateMutability()) + &quot;\&quot;.&quot; |   | constructor(bytes32 \_name) public **view** {} |
| typeError | Constructor must be public or internal. |   | constructor(bytes32 \_name) **private** {} |
| typeError | Libraries cannot have fallback functions. |   |   |
| typeError | &quot;Fallback function must be payable or non-payable, but is \&quot;&quot; + stateMutabilityToString(fallback-\&gt;stateMutability()) + &quot;\&quot;.&quot; |   |   |
| typeError | Fallback function cannot take parameters. |   |   |
| typeError | Fallback function cannot return values. |   |   |
| typeError | Fallback function must be defined as \&quot;external\&quot;. |   |   |
| typeError | Function overload clash during conversion to external types for arguments. |   |   |
| typeError | Function signature hash collision for &quot;) + it.second-\&gt;externalSignature() |   |   |
| typeError | Library is not allowed to inherit. |   |   |
| typeError | Library cannot have non-constant state variables |   |   |
| typeError | Library is not allowed to inherit | [Line 476](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/ContractLevelChecker.cpp#L476) | library BritishMuseum {    string constant name = &quot;British Museum&quot;; } library MyLibrary is BritishMuseum {    } |
|   | Function has no declaration?! |   |   |
|   | Type only supported by the new experimental ABI encoder&quot;, currentLoc |   |   |
| fatalTypeError | &quot;Contract \&quot;&quot;) + \_contract.name() + &quot;\&quot; does not use the new experimental ABI encoder but wants to inherit from a contract &quot; + &quot;which uses types that require it. &quot; + &quot;Use \&quot;pragma experimental ABIEncoderV2;\&quot; for the inheriting contract as well to enable the feature.&quot; |   |   |



**ContractFlowAnalyzer.cpp**

| **Error Type** | **Message** | **Source File** |   |
| --- | --- | --- | --- |
| typeError | The variable was declared here.&quot;, variableOccurrence-\&gt;declaration().location() This variable is of storage pointer type and can be &quot;) + (variableOccurrence-\&gt;kind() == VariableOccurrence::Kind::Return ? &quot;returned&quot; : &quot;accessed&quot;) + &quot; without prior assignment.&quot; | [Lines 135 - 146](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ControlFlowAnalyzer.cpp#L135-L146)  **I really do not understand this error** | **    function retrieve(address \_sender) internal view returns (mapping(address =\&gt; uint) storage) {****        ****     }** |
| warning | Unreachable code. | [Line 179](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ControlFlowAnalyzer.cpp#L179) |   |



**ContractFlowBuilder.cpp**

_Nothing here, but might need to deep dive to understand what does this file do_

**ContractFlowGraph.cpp**

_Same_

**DeclarationContainer.cpp**

| **Error Type** | **Message** | **Source File** |   |
| --- | --- | --- | --- |
| solAssert | Tried to activate a non-inactive variable or multiple inactive variables with the same name. | [Line 85-88](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/DeclarationContainer.cpp#L85-L88) |   |
| solAssert | Attempt to update function definition. | [Line 113](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/DeclarationContainer.cpp#L113) |   |
| solAssert | Attempt to resolve empty name. | [Line 128](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/DeclarationContainer.cpp#L128) |   |



**DocStringAnalyser.cpp**

| **Error Type** | **Message** | **Source File** |   |
| --- | --- | --- | --- |
| DocstringParsingError  | &quot;Documented parameter \&quot;&quot; + i-\&gt;second.paramName +&quot;\&quot; not found in the parameter list of the function.&quot; | [Line 87-92](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/DocStringAnalyser.cpp#L87-L92) |   |
| DocstringParsingError  | Doc tag @ \&lt;tagName\&gt; not valid for &quot; + \_nodeName + &quot;. | [Line 134](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/DocStringAnalyser.cpp#L134) |
 ![](data:image/*;base64,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)

 ![](data:image/*;base64,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)


    /// @title sqrt()    /// @notice Math function to calculate square root    /// @dev Not working with decimal numbers    /// @param x The number to calculate the square root of    /// @return y The square root of x    function sqrt(uint x) internal pure returns (uint y) {        uint z = (x + 1) / 2;        y = x;        while (z \&lt; y) {            y = z;            z = (x / z + z) / 2;        }    } |
| DocstringParsingError | End of tag \&lt;tagName\&gt; not found | Can&#39;t find it anywere |
 ![](data:image/*;base64,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)


 |

**GlobalContext.cpp**

_Nothing_



**NameAndTypeResolver.cpp**

| **Error Type** | **Message** | **Source File** |   |
| --- | --- | --- | --- |
| solAssert | Unable to register global declaration. | [Lines 50 - 53](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L50-L53) |   |
| declarationError | Import \&quot;&quot; + path + &quot;\&quot; (referenced as \&quot;&quot; + imp-\&gt;path() + &quot;\&quot;) not found. | [Lines 80 - 87](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L80-L88) |   |
| declarationError | &quot;Declaration \&quot;&quot; + alias.first-\&gt;name() + &quot;\&quot; not found in \&quot;&quot; + path + &quot;\&quot; (referenced as \&quot;&quot; + imp-\&gt;path() + &quot;\&quot;).&quot; | [Lines 95 - 108](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L95-L108) |   |
| solAssert | Updated declaration outside global scope.&quot; | [Lines 143 - 147](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L143-L147) |   |
| solAssert | Found overloading involving something not a function, event or a (magic) variable. | [Lines 210 - 216](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L210-L216) |   |
| solAssert | Failed to determine the function type of the overloaded. | [Line 221](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L221) |   |
| fatalDeclarationError | Function type can not be used in this context. | [Lines 224 - 225](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L224-L225) |   |
| warning | Variable is shadowed in inline assembly by an instruction of the same name | [Lines 255 - 258](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L255-L258) |   |
| declarationError | &quot;The previous declaration is here:&quot;, firstDeclarationLocation), &quot;Identifier already declared.&quot; | [Lines 365 - 369](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L365-L369) |   |
| fatalTypeError | Contract expected. | [Lines 382 - 383](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L382-L383) |   |
| fatalTypeError | Definition of base has to precede definition of derived contract | [Lines 388 - 389](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L388-L389) | pragmasolidity ^0.5.0; contract MyContract is MyLibrary {        function encodeString(string memory sentence) publicpurereturns (bytes memory) {        return abi.encode(sentence);    }        function decodeString(bytes memory data) publicpurereturns (string memory) {        return abi.decode(data, (string));    }    } library MyLibrary {    // interface code here}  |
| fatalTypeError | Linearization of inheritance graph impossible | [Lines 394 - 395](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L394-L395) |   |
| solAssert | Scopes not correctly closed. | [Line 469](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L469) |   |
| declarationError  | &quot;The previous declaration is here:&quot;, firstDeclarationLocation),&quot;Identifier already declared.&quot; | [Lines 515 - 519](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L515-L519) |   |
| warning | This declaration shadows a builtin symbol. | [Lines 524 - 528](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L524-L528) |   |
| warning  | This declaration shadows an existing declaration.The shadowed declaration is here:&quot;, shadowedLocation | [Lines 529 - 537](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L529-L537) |   |
| solAssert | Variable declaration without function. | [Line 679](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L679) |   |
| solAssert | Unable to add new scope. | [Line 708](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L708) |   |
| solAssert | Closed non-existing scope. | [Line 714](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L714) |   |
| solAssert | No current scope. | [Line 720](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/NameAndTypeResolver.cpp#L720) |   |

**PostTypeChecker.cpp**

| **Error Type** | **Message** | **Source File** |   |
| --- | --- | --- | --- |
| typeError | &quot;The value of the constant &quot; + declaration-\&gt;name() +&quot; has a cyclic dependency via &quot; + identifier-\&gt;name() + &quot;.&quot; | [Line 52 - 57](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/PostTypeChecker.cpp#L52-L57) |   |
| fatalDeclarationError | Variable definition exhausting cyclic dependency validator. | [Line 96 - 97](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/PostTypeChecker.cpp#L96-L97) |   |



**ReferencesResolver.cpp**

| **Error Type** | **Message** | **Source File** |   |
| --- | --- | --- | --- |
| declarationError | Undeclared identifier. […]  - is not (or not yet) visible at this point.- Did you mean … | [Line 99 - 111](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L99-L111) | library MyLib {    function testCall() external {        test();    }        function test() public {        testCall();    }}  |
| typeError | Address types can only be payable or non-payable. | [Line 138 - 141](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L138-L141) |   |
| fatalDeclarationError | Identifier not found or not unique. | [Line 176 - 180](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L176-L180) |   |
| typeError | Name has to refer to a struct, enum or contract. | [Line 190 - 194](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L190-L194) |   |
| fatalTypeError | Invalid visibility, can only be \&quot;external\&quot; or \&quot;internal\&quot;. | [Line 199 - 207](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L199-L207) |   |
| fatalTypeError | Only external function types can be payable. | [Line 209 - 213](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L209-L213) |   |
| solAssert | Type not set for parameter | [Line 218](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L218) |   |
| fatalTypeError | Internal type cannot be used for external function type. | [Line 219 - 223](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L219-L223) |   |
| fatalTypeError | Illegal base type of storage size zero for array. | [Line 248 - 249](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L248-L249) |   |
| fatalTypeError | Invalid array length, expected integer literal or constant expression. | [Line 256 - 257](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L256-L257) |   |
| fatalTypeError | Array with zero length specified. | [Line 258 - 259](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L258-L259) |   |
| fatalTypeError | Array with fractional length specified. | [Line 260 - 261](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L260-L261) |   |
| fatalTypeError | Array with negative length specified. | [Line 262 - 263](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L262-L263) |   |
| declarationError | In variable names \_slot and \_offset can only be used as a suffix. | [Lines 297 - 301](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L297-L301) |   |
| declarationError | Multiple matching identifiers. Resolving overloaded identifiers is not supported. | [Lines 304 - 308](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L304-L308) |   |
| declarationError | Cannot access local Solidity variables from inside an inline assembly function. | [Lines 312 - 316](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L312-L316) |   |
| declarationError | The \&quot;constant\&quot; keyword can only be used for state variables. | [Lines 339 – 350](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L349-L350) |   |
| typeError | Data location can only be specified for array, struct or mapping types  | [Lines 381 - 382](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L381-L382) |   |
| typeError | Data location must be /memory/storage/calldata/none for return / external for variable | [Lines 383 - 402](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L383-L402) |   |
| solAssert | Data location not properly set. | [Lines 437 - 438](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L437-L438) |   |

**ViewPureChecker.cpp**

| **Error Type** | **Message** | **Source File** |   |
| --- | --- | --- | --- |
| warning | &quot;Function state mutability can be restricted to &quot; + stateMutabilityToString(m\_bestMutabilityAndLocation.mutability) | [Line 169 - 181](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ViewPureChecker.cpp#L169-L181) |   |
| typeError | &quot;Function declared as pure, but this expression (potentially) reads from the environment or state and thus requires \&quot;view\&quot;.&quot; | [Line 252 - 265](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ViewPureChecker.cpp#L252-L265) |   |
| typeError | &quot;Function declared as &quot; + tateMutabilityToString(m\_currentFunction-\&gt;stateMutability()) + &quot;, but this expression (potentially) modifies the state and thus &quot;&quot;requires non-payable (the default) or payable.&quot; | [Line 266 - 276](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ViewPureChecker.cpp#L266-L276) |   |
| typeError | &quot;\&quot;msg.value\&quot; or \&quot;callvalue()\&quot; appear here inside the modifier.&quot;, \*\_nestedLocation &quot;This modifier uses \&quot;msg.value\&quot; or \&quot;callvalue()\&quot; and thus the function has to be payable or internal.&quot; | [Line 283 - 288](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ViewPureChecker.cpp#L283-L288) |     modifier TxFee(uint \_fee) {        msg.value + \_fee;        \_;    }        function sendEther(address payable \_recipient, uint \_ether) public TxFee(10) {        \_recipient.transfer(\_ether);    }  |
| typeError | &quot;\&quot;msg.value\&quot; and \&quot;callvalue()\&quot; can only be used in payable public functions. Make the function &quot;&quot;\&quot;payable\&quot; or use an internal function to avoid this error.&quot; | [Line 289 - 294](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ViewPureChecker.cpp#L289-L294) |   |
