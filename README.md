# Solidity Compiler Debug List
A curated list of all the Errors and Warning from the Solidity Compiler.

This repository aims to help Smart Contracts Developers on Ethereum to debug their Solidity files. It offers a list of all the errors and warnings you can possibly encounter when developing with the Remix IDE or during compilation with `solc`.

The list is based on the official C++ source code from the Solidity compiler. The errors and warning messages are all mentioned in the files located in the folder [`libsolidity/analysis`](https://github.com/ethereum/solidity/tree/develop/libsolidity/analysis). 

All the errors and warnings listed below are referenced to the original source file + line number for better analysis. Some explanations and solutions are provided to help debugging.


# See the website under development

1) Create a directory and enter into it : 
```
mkdir UI && cd UI
```

2) Install docsify via npm : 

```
npm install docsify --save
```

the `--save` option will save docsify locally in your `UI` folder. 
You can use the `-g` flag to install docsify globally.

3) Clone the repository : 

git clone https://github.com/CJ42/Solidity-Compiler-Debug-List.git


4) Enter in the `/docs` folder within the directory : 
```
cd Solidity-Compiler-Debug-List/docs
```

5) run `docsify serve`

6) Access the website via http://localhost:3000
 

