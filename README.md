# System-RDL-preprocess
This project is used to develop a script to support the preprocess code that defined in System RDL spec 1.0 Chapter 13.
Only part of the following verilog directives are supported (see the comments below.) but I think it's enough.

```
`define SystemVerilog Text macro definition
`if Verilog Conditional compilation        //UNSUPPORTED
`else Verilog Conditional compilation              
`elsif Verilog Conditional compilation
`ifdef Verilog Conditional compilation
`ifndef Verilog Conditional compilation
`include Verilog File inclusion
`line Verilog Source filename and number    //UNSUPORTED
`undef Verilog Undefine text macro
```
=====Embedded Perl preprocessing SPEC from System RDL spec 1.0 Chapter 13 =======

## 13. Preprocessor directives

SystemRDL provides for file inclusion and text substitution through the use of preprocessor directives.
There are two phases of preprocessing in SystemRDL: embedded Perl preprocessing and a more traditional
Verilog-style preprocessor. The embedded Perl preprocessing is handled first and the resulting substituted
code is passed through a traditional Verilog-style preprocessor.

### 13.1 Embedded Perl preprocessing

The SystemRDL preprocessor provides more power than traditional macro-based preprocessing without the
dangers of unexpected text substitution. Instead of macros, SystemRDL allows designers to embed snippets
of Perl code into the source.

#### 13.1.1 Semantics

a) Perl snippets shall begin with <% and be terminated by %>; between these markers any valid Perl
syntax may be used.
b) Any SystemRDL code outside of the Perl snippet markers is equivalent to the Perl print 'RDL
code' and the resulting code is printed directly to the post-processed output.
c) <%=$VARIABLE%> (no whitespace is allowed) is equivalent to the Perl print $VARIABLE.
d) The resulting Perl code is interpreted and the result is sent to the traditional Verilog-style preprocessor.

#### 13.1.2 Example

This example shows the use of the SystemRDL preprocessor.
```
// An example of Apache’s ASP standard for embedding Perl
reg myReg {
<% for( $i = 0; $i < 6; $i += 2 ) { %>
myField data<%=$i%> [<%=$i+1%>:<%=$i%>];
<% } %>
};
```
When processed, this is replaced by the following.
```
// Code resulting from embedded Perl script
reg myReg {
myField data0 [1:0];
myField data2 [3:2];
myField data4 [5:4];
};
```

