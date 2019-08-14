# Functions

You will find below all the errors and warnings related to functions in Solidity.

---

## Unused function parameter

|Heading|Description|
|-|-|
|**Title**|Unused function parameter|
|**Type**|`warning`|
|**Message**|```Unused function parameter. Remove or comment out the variable name to silence this warning.```|
|**Solidity version**||
|**Reference**|[StaticAnalyzer.cpp, Line 124](https://github.com/ethereum/solidity/blob/efd8d8fe5eced023476af71491e9eae3dbde4d87/libsolidity/analysis/StaticAnalyzer.cpp#L124)|
|**Contributors**||


**Description**



**Example**

```solidity
pragma solidity ^0.5.0;

contract Example {

    function sayHello(string memory _name, uint8 _age) public pure returns (string memory) {
        string memory myName = _name;
        return myName;
    }
}
```

**Solution**

---

## No visibility specified in function declaration


|Heading|Description|
|-|-|
|**Title**|No visibility specified in function declaration|
|**Type**|`SyntaxError`|
|**Message**|```No visibility specified. Did you intend to add "public"```|
|**Solidity version**||
|**Reference**|[SyntaxChecker.cpp, Line...](#)|
|**Contributors**|CJ42|


**Description**

The function does not specify a visibility.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Test {

    function run() {
        // do something
    }

}
```

**Solution**

Declare your function with one of the following visibility : `public`, `private`, `internal` or `external`.

---

## Function with same name and argument defined twice

|Heading|Description|
|-|-|
|**Title**|Function with same name + argument defined twice|
|**Type**|`declarationError`|
|**Message**|```Function with same name and arguments defined twice.```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, |
|**Contributors**|CJ42|


**Description**

Two functions with the same names and arguments are defined in your smart contract.

**Example**

```solidity
pragma solidity ^0.5.10;

contract Example {
    
    function foo() public pure returns(string memory) {
        return "foo";
    }
    
    function foo() public pure returns(string memory) {
        return "foo";
    }
    
}
```

**Solution**

You can declare several function with the same name (_eg:_ `foo()`) but each function should have different set of arguments.

```solidity
pragma solidity ^0.5.10;

contract Example {
    
    function foo() public pure returns(string memory) {
        return "foo";
    }
    
    function foo(string memory _string) public pure returns(string memory) {
        return _string;
    }
    
}
```

---

## Different return type when overriding function

|Heading|Description|
|-|-|
|**Title**|Different return type when overriding function|
|**Type**|`typeError / overrideError`|
|**Message**|```Overriding function return types differ.```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, |
|**Contributors**|CJ42|


**Description**

The contract overrides a function from an inherited contract but specifies a different type for the return value.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Greetings {

    function hello() external pure returns (string memory);

    function goodbye() external pure returns (string memory);

}

contract French is Greetings {
    
    function hello() external pure returns (string memory) {
        return "Bonjour";
    }

}

contract Robot is Greetings {

    function hello() external pure returns (uint) {
        return 1;
    }
}
```

**Solution**



---

## Different visibility when overriding function

|Heading|Description|
|-|-|
|**Title**|Different visibility when overriding function|
|**Type**|`typeError / overrideError`|
|**Message**|```Overriding function visibility differs.```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, [Lines 190 - 194](https://github.com/ethereum/solidity/blob/4f7fec6911482c9c3f8fbf2e3fa5874597648fc6/libsolidity/analysis/ContractLevelChecker.cpp#L190-L194)|
|**Contributors**|CJ42|


**Description**

The contract overrides a function from an inherited contract but specifies a wrong visibility type. You can only overrides a function in Solidity with the same visibility scope :

- `public`  <---> `external`
- `private` <---> `internal`


**Example**

```solidity
pragma solidity ^0.5.0;

contract Token {
    
   function transfer(address _to, uint256 _value) external returns (bool success) {
       // your code here
   }

}

contract StandardToken is Token {
    
    function transfer(address _to, uint256 _value) internal returns (bool success) {
        // your code here
    }
    
}
```

**Solution**

The following overriding options are available :

- from `external` to `public`
- from `public` to `external`
- from `internal` to `private`
- from `private` to `internal`

---

## Different State Mutability when overriding function

|Heading|Description|
|-|-|
|**Title**|Different State Mutability when overriding function|
|**Type**|`typeError / overrideError`|
|**Message**|```"Overriding function changes state mutability from < State Mutability 1 > to < State Mutability 2 >."```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, [Lines 196 - 205](https://github.com/ethereum/solidity/blob/4f7fec6911482c9c3f8fbf2e3fa5874597648fc6/libsolidity/analysis/ContractLevelChecker.cpp#L196-L205)|
|**Contributors**|CJ42|


**Description**

The contract overrides a function from an inherited contract but implements a different state mutability (`view`, `pure` or nothing).

**Example**

```solidity
pragma solidity ^0.5.0;

contract Token {
    
   function transfer(address _to, uint256 _value) external pure returns (bool success) {
       // your code here
   }

}

contract StandardToken is Token {
    
    function transfer(address _to, uint256 _value) external view returns (bool success) {
        // your code here
    }
    
}
```


**Solution**

Make sure that the overriden function implements the same state mutability than the initial function.

---

## Implemented function redeclared as abstract

|Heading|Description|
|-|-|
|**Title**|Implemented function redeclared as abstract|
|**Type**|`typeError`|
|**Message**|```Redeclaring an already implemented function as abstract```|
|**Solidity version**||
|**Reference**|ContractLevelChecker.cpp, [Lines 235 - 236](https://github.com/ethereum/solidity/blob/4f7fec6911482c9c3f8fbf2e3fa5874597648fc6/libsolidity/analysis/ContractLevelChecker.cpp#L235-L236)|
|**Contributors**|CJ42|


**Description**

A contract re-implements a function inherited from an other contract but declares it as abstract.

**Example**

```solidity
pragma solidity ^0.5.0;

contract Greetings {
    
   function sayHello() public pure returns (string memory) {
        return 'hello';
    }

}

contract Person is Greetings {
    
    function sayHello() public pure returns (string memory);
    
}
```

**Solution**


---

## Function not visible by other function call

|Heading|Description|
|-|-|
|**Title**|Function not visible by other function call|
|**Type**|`declarationError`|
|**Message**|```Undeclared identifier. <function1> is not (or not yet) visible at this point.```|
|**Solidity version**||
|**Reference**|ReferencesResolver.cpp, [Lines 99 - 111](https://github.com/ethereum/solidity/blob/f05805c955f73fd2ea1d14dc9edf14b472631b17/libsolidity/analysis/ReferencesResolver.cpp#L99-L111)|
|**Contributors**|CJ42|


**Description**

**Example**

In the following example, our function `testTwo()` cannot execute because it is calling a function `testOne()` declared as `external`, therefore, not callable inside the contract.

```solidity
pragma solidity ^0.5.0;

contract Test {
    
    function testOne() external {
        // do something
    }
    
    function testTwo() public {
        testOne();
    }
}
```

**Solution**

---


