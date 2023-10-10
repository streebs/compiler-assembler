# compiler-assembler

Both the compiler and assembler were designed with object orientation in mind. Object oriented features such as: inhertance and polymorphism are heavily used. This project demonstrates my ability to maintain a large project overtime, create my own datastructures, and implement design patterns. 

This is an overview of a simple compiler, two-pass assembler, and virtual machine written by Sheldon Strebe for the Utah Valley University Capstone course. 

I have been requested by the university to keep the actual repos private to prevent cheating. However, I may grant access to the compiler and/or assembler to those who request it.


# Compiler

The compiler compiles a UVU-designed statically typed language called kxi. It is similar to java in syntax. 

The compiler goes through 4 phases starting with kxi source code and ends with T-Code (a UVU-designed assembly language).

Each phase is as follows:

### Lexical Analysis

The kxi source code undergoes lexical analysis by a python library called PLY (https://www.dabeaz.com/ply/ply.html). Using a list of keywords and regular expressions the lexer verifies that the tokens in the source code are correct.

### Syntax Analysis

After lexical analysis has passed the source code is passed through the PLY parser. Using the kxi grammar the PLY parser ensures there are no syntactical errors in the source code. While the parser is working the source code gets put into an abstract syntax tree of my own design. 

The abstract syntax tree (AST) is built by using a visitor pattern over a tree structure of nodes. Each node is a structure from the source code. 

From this point on the compiler no longer utilizes the source code. Rather, the AST contains the information needed to break down to assembly code. 
**Note**: since the AST is used here on out, so will the visitor pattern. Visitor obects are paramount to the rest of the process.

### Semantic Analysis

Once syntax is verified and the AST is constucted, semantic analysis can begin. Visitor objects are created to walk the tree and check for semantic errors. These errors include things such as: double declarations, undeclared variables, type checking and much more. 

A quick note on Type checking. Type checking is a huge part of semantic analysis and is by far the most difficult to implement.

### Desugaring

Desugaring means remove the 'sugar-coating' from the source code. In this final step the compiler will walk the AST and emit assembly code that is equivalent to the original kxi-source code. Like the previous step this is accomplished by multiple visitors that walk the tree and convert each expression and statement to assembly code.


# Assembler & Virtual Machine


The assmebler takes assembly code and translates it to binary. The virtual machine reads the binary and executes the code based off a UVU-designed instruction set. 

### Assembler

the assembler does 2 things. First, It reads the assembly code and checks for syntax errors using regular expressions. And second, it converts the assembly code to binary. Opcodes and registers are stored as binary to make up the program.

### Virtual Machine

The virtual machine is desiged to emulate a rough Von-Neumann Architecture. by the use of object oriented programming, the virtual machine has a CPU, memory, and Input objects. The CPU is where code execution happens.

The virtual machine creates a symbol table to keep track of addresses in memory. Using the symbol table the CPU executes one command at a time and increments the program counter by 12 (the size of an instruction). 

Output is handled by stdout. 