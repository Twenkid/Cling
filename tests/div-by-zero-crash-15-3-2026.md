## Division by 0 in evaluated expression in a function crashes CLING?
1: 1.2 (below) and
2: 1.4-dev
...

<pre> <font color="#4E9A06"><u style="text-decoration-style:single">sudo</u></font> <font color="#4E9A06">apt</font> list | <font color="#4E9A06">grep</font> <u style="text-decoration-style:single">cling</u>
[sudo] password for idr: 

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

<font color="#CC0000"><b>cling</b></font>-common/jammy,now 1.2-2~jammy amd64 [installed,automatic]
<font color="#CC0000"><b>cling</b></font>/jammy,now 1.2-2~jammy amd64 [installed]
lib<font color="#CC0000"><b>cling</b></font>-dev/jammy 1.2-2~jammy amd64
lib<font color="#CC0000"><b>cling</b></font>/jammy 1.2-2~jammy amd64
<font color="#4E9A06"><b>➜  </b></font><font color="#06989A"><b>~</b></font> <font color="#4E9A06">cling</font> --version                                                  
1.2
<font color="#4E9A06"><b>➜  </b></font><font color="#06989A"><b>~</b></font> <font color="#4E9A06"><u style="text-decoration-style:single">sudo</u></font> <font color="#4E9A06">apt</font> list | <font color="#4E9A06">grep</font> <u style="text-decoration-style:single">cling</u>
<font color="#CC0000"><b>➜  </b></font><font color="#06989A"><b>~</b></font> <font color="#4E9A06">cling</font>          

****************** CLING ******************
* Type C++ code and press enter to run it *
*             Type .q to exit             *
*******************************************
[cling]$ int d;
[cling]$ d
(int) 0
[cling]$ .undo
Stack dump without symbol names (ensure you have llvm-symbolizer in your PATH or set the environment var `LLVM_SYMBOLIZER_PATH` to point to it):
0  libLLVM-18.so.18.1 0x00007e6248b94c96 llvm::sys::PrintStackTrace(llvm::raw_ostream&amp;, int) + 54
1  libLLVM-18.so.18.1 0x00007e6248b92c22 llvm::sys::RunSignalHandlers() + 34
2  libLLVM-18.so.18.1 0x00007e6248b9535b
3  libc.so.6          0x00007e6247642520
4  cling              0x000059696d3a7d93 clang::BaseUsingDecl::removeShadowDecl(clang::UsingShadowDecl*) + 19
5  cling              0x000059696b422c54
6  cling              0x000059696b422eb0 cling::DeclUnloader::VisitDeclContext(clang::DeclContext*) + 288
7  cling              0x000059696b422ff9 cling::DeclUnloader::VisitFunctionDecl(clang::FunctionDecl*, bool) + 89
8  cling              0x000059696b422eb0 cling::DeclUnloader::VisitDeclContext(clang::DeclContext*) + 288
9  cling              0x000059696b4242fc
10 cling              0x000059696b4245e9 cling::DeclUnloader::VisitClassTemplateSpecializationDecl(clang::ClassTemplateSpecializationDecl*) + 41
11 cling              0x000059696b4b18f5 cling::TransactionUnloader::unloadDeclarations(cling::Transaction*, cling::DeclUnloader&amp;) + 517
12 cling              0x000059696b4b1dd6 cling::TransactionUnloader::RevertTransaction(cling::Transaction*) + 518
13 cling              0x000059696b4843dd cling::Interpreter::unload(cling::Transaction&amp;) + 365
14 cling              0x000059696b48459c cling::Interpreter::unload(unsigned int) + 92
15 cling              0x000059696b4d62e0 cling::MetaSema::actOnUndoCommand(unsigned int) + 16
16 cling              0x000059696b4d26fb cling::MetaParser::isundoCommand() + 171
17 cling              0x000059696b4d2f49 cling::MetaParser::isCommand(cling::MetaSema::ActionResult&amp;, cling::Value*) + 633
18 cling              0x000059696b4d3af0 cling::MetaProcessor::process(llvm::StringRef, cling::Interpreter::CompilationResult&amp;, cling::Value*, bool) + 480
19 cling              0x000059696b4eb0c2 cling::UserInterface::runInteractively(bool) + 610
20 cling              0x000059696b38a1ab main + 2747
21 libc.so.6          0x00007e6247629d90
22 libc.so.6          0x00007e6247629e40 __libc_start_main + 128
23 cling              0x000059696b38aee5 _start + 37
PLEASE submit a bug report to https://github.com/llvm/llvm-project/issues/ and include the crash backtrace.
Stack dump:
0.	Program arguments: cling
[1]    1910168 segmentation fault (core dumped)  cling
<font color="#CC0000"><b>➜  </b></font><font color="#06989A"><b>~</b></font>                                     
<font color="#CC0000"><b>➜  </b></font><font color="#06989A"><b>~</b></font> 
<font color="#CC0000"><b>➜  </b></font><font color="#06989A"><b>~</b></font> <font color="#4E9A06">cling</font>

****************** CLING ******************
* Type C++ code and press enter to run it *
*             Type .q to exit             *
*******************************************
[cling]$ namespace n1 { class C { public: C(){int a;} }; }
[cling]$ namespace n2 { class C {public: C(int b){return b*2;}}}
<b>input_line_4:1:42: </b><font color="#CC0000"><b>error: </b></font><b>constructor &apos;C&apos; should not return a value [-Wreturn-type]</b>
namespace n2 { class C {public: C(int b){return b*2;}}}
<font color="#4E9A06"><b>                                         ^      ~~~</b></font>
<b>input_line_4:1:55: </b><font color="#CC0000"><b>error: </b></font><b>expected &apos;;&apos; after class</b>
namespace n2 { class C {public: C(int b){return b*2;}}}
<font color="#4E9A06"><b>                                                      ^</b></font>
<font color="#4E9A06">                                                      ;</font>
[cling]$ namespace n2 { class C {public: C(int b){return b*2;};}
[cling]$ ?   }
<b>input_line_5:1:42: </b><font color="#CC0000"><b>error: </b></font><b>constructor &apos;C&apos; should not return a value [-Wreturn-type]</b>
namespace n2 { class C {public: C(int b){return b*2;};}
<font color="#4E9A06"><b>                                         ^      ~~~</b></font>
<b>input_line_5:1:56: </b><font color="#CC0000"><b>error: </b></font><b>expected &apos;;&apos; after class</b>
namespace n2 { class C {public: C(int b){return b*2;};}
<font color="#4E9A06"><b>                                                       ^</b></font>
<font color="#4E9A06">                                                       ;</font>
[cling]$ namespace n2 { class C {public: C(int b){return b*2;}};}
<b>input_line_6:1:42: </b><font color="#CC0000"><b>error: </b></font><b>constructor &apos;C&apos; should not return a value [-Wreturn-type]</b>
namespace n2 { class C {public: C(int b){return b*2;}};}
<font color="#4E9A06"><b>                                         ^      ~~~</b></font>
[cling]$ namespace n2 { class C {public: C(int b){int d;}};}
[cling]$ 
[cling]$ class C{public: C(float f){int g=2.434;}};
<b>input_line_8:1:34: </b><font color="#75507B"><b>warning: </b></font><b>implicit conversion from &apos;double&apos; to &apos;int&apos; changes value from 2.434 to 2 [-Wliteral-conversion]</b>
class C{public: C(float f){int g=2.434;}};
<font color="#4E9A06"><b>                               ~ ^~~~~</b></font>
[cling]$ class C{public: C(float f){int g=2;}};
<b>input_line_9:1:7: </b><font color="#CC0000"><b>error: </b></font><b>redefinition of &apos;C&apos;</b>
class C{public: C(float f){int g=2;}};
<font color="#4E9A06"><b>      ^</b></font>
<b>input_line_8:1:7: </b><font color="#06989A"><b>note: </b></font>previous definition is here
class C{public: C(float f){int g=2.434;}};
<font color="#4E9A06"><b>      ^</b></font>
[cling]$  
[cling]$  
[cling]$ .undo 
[cling]$ class C{public: C(float f){int g=2;}};
[cling]$ C c1;
<b>input_line_13:2:4: </b><font color="#CC0000"><b>error: </b></font><b>no matching constructor for initialization of &apos;C&apos;</b>
 C c1;
<font color="#4E9A06"><b>   ^</b></font>
<b>input_line_12:1:17: </b><font color="#06989A"><b>note: </b></font>candidate constructor not viable: requires single argument &apos;f&apos;, but no arguments were provided
class C{public: C(float f){int g=2;}};
<font color="#4E9A06"><b>                ^ ~~~~~~~</b></font>
<b>input_line_12:1:7: </b><font color="#06989A"><b>note: </b></font>candidate constructor (the implicit copy constructor) not viable: requires 1 argument, but 0 were provided
class C{public: C(float f){int g=2;}};
<font color="#4E9A06"><b>      ^</b></font>
<b>input_line_12:1:7: </b><font color="#06989A"><b>note: </b></font>candidate constructor (the implicit move constructor) not viable: requires 1 argument, but 0 were provided
class C{public: C(float f){int g=2;}};
<font color="#4E9A06"><b>      ^</b></font>
[cling]$ C c1(4.2);
[cling]$ n1::C c2();
<b>input_line_15:2:10: </b><font color="#75507B"><b>warning: </b></font><b>empty parentheses interpreted as a function declaration [-Wvexing-parse]</b>
 n1::C c2();
<font color="#4E9A06"><b>         ^~</b></font>
<b>input_line_15:2:10: </b><font color="#06989A"><b>note: </b></font>remove parentheses to declare a variable
 n1::C c2();
<font color="#4E9A06"><b>         ^~</b></font>
[cling]$ n1::C c2;
<b>input_line_16:2:8: </b><font color="#CC0000"><b>error: </b></font><b>redefinition of &apos;c2&apos; as different kind of symbol</b>
 n1::C c2;
<font color="#4E9A06"><b>       ^</b></font>
<b>input_line_15:2:8: </b><font color="#06989A"><b>note: </b></font>previous definition is here
 n1::C c2();
<font color="#4E9A06"><b>       ^</b></font>
[cling]$ n1::C c1;
<b>input_line_17:2:8: </b><font color="#CC0000"><b>error: </b></font><b>redefinition of &apos;c1&apos; with a different type: &apos;n1::C&apos; vs &apos;C&apos;</b>
 n1::C c1;
<font color="#4E9A06"><b>       ^</b></font>
<b>input_line_14:2:4: </b><font color="#06989A"><b>note: </b></font>previous definition is here
 C c1(4.2);
<font color="#4E9A06"><b>   ^</b></font>
[cling]$ n1::C c2;
<b>input_line_18:2:8: </b><font color="#CC0000"><b>error: </b></font><b>redefinition of &apos;c2&apos; as different kind of symbol</b>
 n1::C c2;
<font color="#4E9A06"><b>       ^</b></font>
<b>input_line_15:2:8: </b><font color="#06989A"><b>note: </b></font>previous definition is here
 n1::C c2();
<font color="#4E9A06"><b>       ^</b></font>
[cling]$ n1::C c3;
[cling]$ n2::C c4;
<b>input_line_20:2:8: </b><font color="#CC0000"><b>error: </b></font><b>no matching constructor for initialization of &apos;n2::C&apos;</b>
 n2::C c4;
<font color="#4E9A06"><b>       ^</b></font>
<b>input_line_7:1:33: </b><font color="#06989A"><b>note: </b></font>candidate constructor not viable: requires single argument &apos;b&apos;, but no arguments were provided
namespace n2 { class C {public: C(int b){int d;}};}
<font color="#4E9A06"><b>                                ^ ~~~~~</b></font>
<b>input_line_7:1:22: </b><font color="#06989A"><b>note: </b></font>candidate constructor (the implicit copy constructor) not viable: requires 1 argument, but 0 were provided
namespace n2 { class C {public: C(int b){int d;}};}
<font color="#4E9A06"><b>                     ^</b></font>
<b>input_line_7:1:22: </b><font color="#06989A"><b>note: </b></font>candidate constructor (the implicit move constructor) not viable: requires 1 argument, but 0 were provided
namespace n2 { class C {public: C(int b){int d;}};}
<font color="#4E9A06"><b>                     ^</b></font>
[cling]$ n2::C c4(3.44);
<b>input_line_21:2:11: </b><font color="#75507B"><b>warning: </b></font><b>implicit conversion from &apos;double&apos; to &apos;int&apos; changes value from 3.44 to 3 [-Wliteral-conversion]</b>
 n2::C c4(3.44);
<font color="#4E9A06"><b>       ~~ ^~~~</b></font>
[cling]$ n2::C c4(3);
<b>input_line_22:2:8: </b><font color="#CC0000"><b>error: </b></font><b>redefinition of &apos;c4&apos;</b>
 n2::C c4(3);
<font color="#4E9A06"><b>       ^</b></font>
<b>input_line_21:2:8: </b><font color="#06989A"><b>note: </b></font>previous definition is here
 n2::C c4(3.44);
<font color="#4E9A06"><b>       ^</b></font>
[cling]$ n2::C c5(3);
[cling]$ .ls
<b>input_line_24:2:2: </b><font color="#CC0000"><b>error: </b></font><b>expected expression</b>
 .ls
<font color="#4E9A06"><b> ^</b></font>
[cling]$ .printAST
<b>input_line_25:2:2: </b><font color="#CC0000"><b>error: </b></font><b>expected expression</b>
 .printAST
<font color="#4E9A06"><b> ^</b></font>
[cling]$ .ls *
<b>input_line_26:2:2: </b><font color="#CC0000"><b>error: </b></font><b>expected expression</b>
 .ls *
<font color="#4E9A06"><b> ^</b></font>
[cling]$ .ls C
<b>input_line_27:2:2: </b><font color="#CC0000"><b>error: </b></font><b>expected expression</b>
 .ls C
<font color="#4E9A06"><b> ^</b></font>
[cling]$ .ls C;
<b>input_line_28:2:2: </b><font color="#CC0000"><b>error: </b></font><b>expected expression</b>
 .ls C;
<font color="#4E9A06"><b> ^</b></font>
[cling]$ cl
class
cling::
[cling]$ .l
long
[cling]$ #pragma clang diagnostic push
[cling]$ #pragma clang diagnostic ignored &quot;-Winvalid-command-line-argument&quot;
[cling]$ 
[cling]$ #pragma clang __debug dump
<b>input_line_31:1:27: </b><font color="#75507B"><b>warning: </b></font><b>missing argument to debug command &apos;dump&apos; [-Wignored-pragmas]</b>
#pragma clang __debug dump
<font color="#4E9A06"><b>                          ^</b></font>
[cling]$ #pragma clang __debug dump C
lookup results for C:
<font color="#4E9A06"><b>CXXRecordDecl</b></font><font color="#C4A000"> 0x652a2ba31ca8</font> &lt;<font color="#C4A000">input_line_12:1:1</font>, <font color="#C4A000">col:37</font>&gt; <font color="#C4A000">col:7</font> referenced class<font color="#06989A"><b> C</b></font> definition
<font color="#3465A4">|-</font><font color="#4E9A06"><b>DefinitionData</b></font> pass_in_registers empty standard_layout trivially_copyable has_user_declared_ctor can_const_default_init
<font color="#3465A4">| |-</font><font color="#4E9A06"><b>DefaultConstructor</b></font> defaulted_is_constexpr
<font color="#3465A4">| |-</font><font color="#4E9A06"><b>CopyConstructor</b></font> simple trivial has_const_param implicit_has_const_param
<font color="#3465A4">| |-</font><font color="#4E9A06"><b>MoveConstructor</b></font> exists simple trivial
<font color="#3465A4">| |-</font><font color="#4E9A06"><b>CopyAssignment</b></font> simple trivial has_const_param needs_implicit implicit_has_const_param
<font color="#3465A4">| |-</font><font color="#4E9A06"><b>MoveAssignment</b></font> exists simple trivial needs_implicit
<font color="#3465A4">| `-</font><font color="#4E9A06"><b>Destructor</b></font> simple irrelevant trivial needs_implicit
<font color="#3465A4">|-</font><font color="#4E9A06"><b>CXXRecordDecl</b></font><font color="#C4A000"> 0x652a2ba31dd8</font> &lt;<font color="#C4A000">col:1</font>, <font color="#C4A000">col:7</font>&gt; <font color="#C4A000">col:7</font> implicit referenced class<font color="#06989A"><b> C</b></font>
<font color="#3465A4">|-</font><font color="#4E9A06"><b>AccessSpecDecl</b></font><font color="#C4A000"> 0x652a2ba31e80</font> &lt;<font color="#C4A000">col:9</font>, <font color="#C4A000">col:15</font>&gt; <font color="#C4A000">col:9</font> public
<font color="#3465A4">|-</font><font color="#4E9A06"><b>CXXConstructorDecl</b></font><font color="#C4A000"> 0x652a2ba31fb8</font> &lt;<font color="#C4A000">col:17</font>, <font color="#C4A000">col:36</font>&gt; <font color="#C4A000">col:17</font> used<font color="#06989A"><b> C</b></font> <font color="#4E9A06">&apos;void (float)&apos;</font> implicit-inline
<font color="#3465A4">| |-</font><font color="#4E9A06"><b>ParmVarDecl</b></font><font color="#C4A000"> 0x652a2ba31ec0</font> &lt;<font color="#C4A000">col:19</font>, <font color="#C4A000">col:25</font>&gt; <font color="#C4A000">col:25</font><font color="#06989A"><b> f</b></font> <font color="#4E9A06">&apos;float&apos;</font>
<font color="#3465A4">| `-</font><font color="#75507B"><b>CompoundStmt</b></font><font color="#C4A000"> 0x652a2ba32170</font> &lt;<font color="#C4A000">col:27</font>, <font color="#C4A000">col:36</font>&gt;
<font color="#3465A4">|   `-</font><font color="#75507B"><b>DeclStmt</b></font><font color="#C4A000"> 0x652a2ba32158</font> &lt;<font color="#C4A000">col:28</font>, <font color="#C4A000">col:35</font>&gt;
<font color="#3465A4">|     `-</font><font color="#4E9A06"><b>VarDecl</b></font><font color="#C4A000"> 0x652a2ba320b8</font> &lt;<font color="#C4A000">col:28</font>, <font color="#C4A000">col:34</font>&gt; <font color="#C4A000">col:32</font><font color="#06989A"><b> g</b></font> <font color="#4E9A06">&apos;int&apos;</font> cinit
<font color="#3465A4">|       `-</font><font color="#75507B"><b>IntegerLiteral</b></font><font color="#C4A000"> 0x652a2ba32120</font> &lt;<font color="#C4A000">col:34</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"><b> 2</b></font>
<font color="#3465A4">|-</font><font color="#4E9A06"><b>CXXConstructorDecl</b></font><font color="#C4A000"> 0x652a2ba32420</font> &lt;<font color="#C4A000">col:7</font>&gt; <font color="#C4A000">col:7</font> implicit constexpr<font color="#06989A"><b> C</b></font> <font color="#4E9A06">&apos;void (const C &amp;)&apos;</font> inline default trivial noexcept-unevaluated 0x652a2ba32420
<font color="#3465A4">| `-</font><font color="#4E9A06"><b>ParmVarDecl</b></font><font color="#C4A000"> 0x652a2ba32550</font> &lt;<font color="#C4A000">col:7</font>&gt; <font color="#C4A000">col:7</font> <font color="#4E9A06">&apos;const C &amp;&apos;</font>
<font color="#3465A4">`-</font><font color="#4E9A06"><b>CXXConstructorDecl</b></font><font color="#C4A000"> 0x652a2ba32630</font> &lt;<font color="#C4A000">col:7</font>&gt; <font color="#C4A000">col:7</font> implicit constexpr<font color="#06989A"><b> C</b></font> <font color="#4E9A06">&apos;void (C &amp;&amp;)&apos;</font> inline default trivial noexcept-unevaluated 0x652a2ba32630
<font color="#3465A4">  `-</font><font color="#4E9A06"><b>ParmVarDecl</b></font><font color="#C4A000"> 0x652a2ba337a0</font> &lt;<font color="#C4A000">col:7</font>&gt; <font color="#C4A000">col:7</font> <font color="#4E9A06">&apos;C &amp;&amp;&apos;</font>
[cling]$ int d;
[cling]$ dump d
<b>input_line_34:2:2: </b><font color="#CC0000"><b>error: </b></font><b>unknown type name &apos;dump&apos;</b>
 dump d
<font color="#4E9A06"><b> ^</b></font>
[cling]$ #pragma clang __debug dump d
lookup results for d:
<font color="#4E9A06"><b>VarDecl</b></font><font color="#C4A000"> 0x652a2baa3580</font> &lt;<font color="#C4A000">input_line_33:2:2</font>, <font color="#C4A000">col:6</font>&gt; <font color="#C4A000">col:6</font> used<font color="#06989A"><b> d</b></font> <font color="#4E9A06">&apos;int&apos;</font>
[cling]$ int f1(int a, int b){
[cling]$ ?   if (a&gt;b) return b*a;
[cling]$ ?   if (a+b)&lt;34 return b/a;
[cling]$ ?   return 1.35;
[cling]$ ?   }
<b>input_line_36:3:9: </b><font color="#CC0000"><b>error: </b></font><b>expected expression</b>
if (a+b)&lt;34 return b/a;
<font color="#4E9A06"><b>        ^</b></font>
<b>input_line_36:4:8: </b><font color="#75507B"><b>warning: </b></font><b>implicit conversion from &apos;double&apos; to &apos;int&apos; changes value from 1.35 to 1 [-Wliteral-conversion]</b>
return 1.35;
<font color="#4E9A06"><b>~~~~~~ ^~~~</b></font>
[cling]$ f1(34,11);
<b>input_line_37:2:2: </b><font color="#CC0000"><b>error: </b></font><b>use of undeclared identifier &apos;f1&apos;</b>
 f1(34,11);
<font color="#4E9A06"><b> ^</b></font>
[cling]$ int f1(int a, int b){
[cling]$ ?   if (a&gt;b) return b*a;
[cling]$ ?   if ((a+b)&lt;34) return int(b/a);
[cling]$ ?   return 4;
[cling]$ ?   }
[cling]$ f1(3,5)
(int) 1
[cling]$ f1(6,11);
[cling]$ auto x = f1(6,11);
[cling]$ x
(int) 0
[cling]$ auto x = f1(56,11);
<b>input_line_44:2:7: </b><font color="#CC0000"><b>error: </b></font><b>redefinition of &apos;x&apos;</b>
 auto x = f1(56,11);
<font color="#4E9A06"><b>      ^</b></font>
<b>input_line_42:2:7: </b><font color="#06989A"><b>note: </b></font>previous definition is here
 auto x = f1(6,11);
<font color="#4E9A06"><b>      ^</b></font>
[cling]$ auto x1 = f1(56,11);
[cling]$ x1
(int) 0
[cling]$ #pragma clang __debug dump f1
lookup results for f1:
<font color="#4E9A06"><b>FunctionDecl</b></font><font color="#C4A000"> 0x652a2badc1d0</font> &lt;<font color="#C4A000">input_line_38:1:1</font>, <font color="#C4A000">line:5:1</font>&gt; <font color="#C4A000">line:1:5</font> used<font color="#06989A"><b> f1</b></font> <font color="#4E9A06">&apos;int (int, int)&apos;</font>
<font color="#3465A4">|-</font><font color="#4E9A06"><b>ParmVarDecl</b></font><font color="#C4A000"> 0x652a2baa4348</font> &lt;<font color="#C4A000">col:8</font>, <font color="#C4A000">col:12</font>&gt; <font color="#C4A000">col:12</font> used<font color="#06989A"><b> a</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">|-</font><font color="#4E9A06"><b>ParmVarDecl</b></font><font color="#C4A000"> 0x652a2baa43c8</font> &lt;<font color="#C4A000">col:15</font>, <font color="#C4A000">col:19</font>&gt; <font color="#C4A000">col:19</font> used<font color="#06989A"><b> b</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">`-</font><font color="#75507B"><b>CompoundStmt</b></font><font color="#C4A000"> 0x652a2badc610</font> &lt;<font color="#C4A000">col:21</font>, <font color="#C4A000">line:5:1</font>&gt;
<font color="#3465A4">  |-</font><font color="#75507B"><b>IfStmt</b></font><font color="#C4A000"> 0x652a2badc3d0</font> &lt;<font color="#C4A000">line:2:1</font>, <font color="#C4A000">col:19</font>&gt;
<font color="#3465A4">  | |-</font><font color="#75507B"><b>BinaryOperator</b></font><font color="#C4A000"> 0x652a2badc310</font> &lt;<font color="#C4A000">col:5</font>, <font color="#C4A000">col:7</font>&gt; <font color="#4E9A06">&apos;bool&apos;</font> &apos;&gt;&apos;
<font color="#3465A4">  | | |-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc2e0</font> &lt;<font color="#C4A000">col:5</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  | | | `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc2a0</font> &lt;<font color="#C4A000">col:5</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa4348</font><font color="#06989A"><b> &apos;a&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  | | `-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc2f8</font> &lt;<font color="#C4A000">col:7</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  | |   `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc2c0</font> &lt;<font color="#C4A000">col:7</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa43c8</font><font color="#06989A"><b> &apos;b&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  | `-</font><font color="#75507B"><b>ReturnStmt</b></font><font color="#C4A000"> 0x652a2badc3c0</font> &lt;<font color="#C4A000">col:10</font>, <font color="#C4A000">col:19</font>&gt;
<font color="#3465A4">  |   `-</font><font color="#75507B"><b>BinaryOperator</b></font><font color="#C4A000"> 0x652a2badc3a0</font> &lt;<font color="#C4A000">col:17</font>, <font color="#C4A000">col:19</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &apos;*&apos;
<font color="#3465A4">  |     |-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc370</font> &lt;<font color="#C4A000">col:17</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  |     | `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc330</font> &lt;<font color="#C4A000">col:17</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa43c8</font><font color="#06989A"><b> &apos;b&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  |     `-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc388</font> &lt;<font color="#C4A000">col:19</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  |       `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc350</font> &lt;<font color="#C4A000">col:19</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa4348</font><font color="#06989A"><b> &apos;a&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  |-</font><font color="#75507B"><b>IfStmt</b></font><font color="#C4A000"> 0x652a2badc5c0</font> &lt;<font color="#C4A000">line:3:1</font>, <font color="#C4A000">col:29</font>&gt;
<font color="#3465A4">  | |-</font><font color="#75507B"><b>BinaryOperator</b></font><font color="#C4A000"> 0x652a2badc4c0</font> &lt;<font color="#C4A000">col:5</font>, <font color="#C4A000">col:11</font>&gt; <font color="#4E9A06">&apos;bool&apos;</font> &apos;&lt;&apos;
<font color="#3465A4">  | | |-</font><font color="#75507B"><b>ParenExpr</b></font><font color="#C4A000"> 0x652a2badc480</font> &lt;<font color="#C4A000">col:5</font>, <font color="#C4A000">col:9</font>&gt; <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  | | | `-</font><font color="#75507B"><b>BinaryOperator</b></font><font color="#C4A000"> 0x652a2badc460</font> &lt;<font color="#C4A000">col:6</font>, <font color="#C4A000">col:8</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &apos;+&apos;
<font color="#3465A4">  | | |   |-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc430</font> &lt;<font color="#C4A000">col:6</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  | | |   | `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc3f0</font> &lt;<font color="#C4A000">col:6</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa4348</font><font color="#06989A"><b> &apos;a&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  | | |   `-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc448</font> &lt;<font color="#C4A000">col:8</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  | | |     `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc410</font> &lt;<font color="#C4A000">col:8</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa43c8</font><font color="#06989A"><b> &apos;b&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  | | `-</font><font color="#75507B"><b>IntegerLiteral</b></font><font color="#C4A000"> 0x652a2badc4a0</font> &lt;<font color="#C4A000">col:11</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"><b> 34</b></font>
<font color="#3465A4">  | `-</font><font color="#75507B"><b>ReturnStmt</b></font><font color="#C4A000"> 0x652a2badc5b0</font> &lt;<font color="#C4A000">col:15</font>, <font color="#C4A000">col:29</font>&gt;
<font color="#3465A4">  |   `-</font><font color="#75507B"><b>CXXFunctionalCastExpr</b></font><font color="#C4A000"> 0x652a2badc588</font> &lt;<font color="#C4A000">col:22</font>, <font color="#C4A000">col:29</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> functional cast to int &lt;NoOp&gt;
<font color="#3465A4">  |     `-</font><font color="#75507B"><b>BinaryOperator</b></font><font color="#C4A000"> 0x652a2badc568</font> &lt;<font color="#C4A000">col:26</font>, <font color="#C4A000">col:28</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &apos;/&apos;
<font color="#3465A4">  |       |-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc538</font> &lt;<font color="#C4A000">col:26</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  |       | `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc4f8</font> &lt;<font color="#C4A000">col:26</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa43c8</font><font color="#06989A"><b> &apos;b&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  |       `-</font><font color="#75507B"><b>ImplicitCastExpr</b></font><font color="#C4A000"> 0x652a2badc550</font> &lt;<font color="#C4A000">col:28</font>&gt; <font color="#4E9A06">&apos;int&apos;</font> &lt;<font color="#CC0000">LValueToRValue</font>&gt;
<font color="#3465A4">  |         `-</font><font color="#75507B"><b>DeclRefExpr</b></font><font color="#C4A000"> 0x652a2badc518</font> &lt;<font color="#C4A000">col:28</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"> lvalue</font> <font color="#4E9A06"><b>ParmVar</b></font><font color="#C4A000"> 0x652a2baa4348</font><font color="#06989A"><b> &apos;a&apos;</b></font> <font color="#4E9A06">&apos;int&apos;</font>
<font color="#3465A4">  `-</font><font color="#75507B"><b>ReturnStmt</b></font><font color="#C4A000"> 0x652a2badc600</font> &lt;<font color="#C4A000">line:4:1</font>, <font color="#C4A000">col:8</font>&gt;
<font color="#3465A4">    `-</font><font color="#75507B"><b>IntegerLiteral</b></font><font color="#C4A000"> 0x652a2badc5e0</font> &lt;<font color="#C4A000">col:8</font>&gt; <font color="#4E9A06">&apos;int&apos;</font><font color="#06989A"><b> 4</b></font>
[cling]$ f1(0,0)
Stack dump without symbol names (ensure you have llvm-symbolizer in your PATH or set the environment var `LLVM_SYMBOLIZER_PATH` to point to it):
0  libLLVM-18.so.18.1 0x0000717ae1794c96 llvm::sys::PrintStackTrace(llvm::raw_ostream&amp;, int) + 54
1  libLLVM-18.so.18.1 0x0000717ae1792c22 llvm::sys::RunSignalHandlers() + 34
2  libLLVM-18.so.18.1 0x0000717ae179535b
3  libc.so.6          0x0000717ae0242520
4  libc.so.6          0x0000717ae087302d
5  libc.so.6          0x0000717ade1b6037
6  cling              0x0000652a281dd9bc cling::IncrementalExecutor::executeWrapper(llvm::StringRef, cling::Value*) const + 204
7  cling              0x0000652a281f8833 cling::Interpreter::RunFunction(clang::FunctionDecl const*, cling::Value*) + 195
8  cling              0x0000652a281f8f87 cling::Interpreter::EvaluateInternal(std::__cxx11::basic_string&lt;char, std::char_traits&lt;char&gt;, std::allocator&lt;char&gt;&gt; const&amp;, cling::CompilationOptions, cling::Value*, cling::Transaction**, unsigned long) + 535
9  cling              0x0000652a281f932f cling::Interpreter::process(std::__cxx11::basic_string&lt;char, std::char_traits&lt;char&gt;, std::allocator&lt;char&gt;&gt; const&amp;, cling::Value*, cling::Transaction**, bool) + 399
10 cling              0x0000652a28249b6a cling::MetaProcessor::process(llvm::StringRef, cling::Interpreter::CompilationResult&amp;, cling::Value*, bool) + 602
11 cling              0x0000652a282610c2 cling::UserInterface::runInteractively(bool) + 610
12 cling              0x0000652a281001ab main + 2747
13 libc.so.6          0x0000717ae0229d90
14 libc.so.6          0x0000717ae0229e40 __libc_start_main + 128
15 cling              0x0000652a28100ee5 _start + 37
PLEASE submit a bug report to https://github.com/llvm/llvm-project/issues/ and include the crash backtrace.
Stack dump:
0.	Program arguments: cling
[1]    1913261 floating point exception (core dumped)  cling
<font color="#CC0000"><b>➜  </b></font><font color="#06989A"><b>~</b></font> 
</pre>


## 1.4-dev

```
****************** CLING ******************
* Type C++ code and press enter to run it *
*     Type .? for help and .q to exit     *
*******************************************
[cling]$ int f(){ int a = 0; int b = 34; return b/a;}
[cling]$ f();
Stack dump without symbol names (ensure you have llvm-symbolizer in your PATH or set the environment var `LLVM_SYMBOLIZER_PATH` to point to it):
0  cling     0x00005a1b1abf6e92 llvm::sys::PrintStackTrace(llvm::raw_ostream&, int) + 66
1  cling     0x00005a1b1abf4356
2  libc.so.6 0x000077e69a245330
3  libc.so.6 0x000077e69aaa301a
4  libc.so.6 0x000077e69aaa5034
5  cling     0x00005a1b1aad17e8 cling::IncrementalExecutor::executeWrapper(llvm::StringRef, cling::Value*) const + 200
6  cling     0x00005a1b1aaeab58 cling::Interpreter::RunFunction(clang::FunctionDecl const*, cling::Value*) + 200
7  cling     0x00005a1b1aaeb318 cling::Interpreter::EvaluateInternal(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char>> const&, cling::CompilationOptions, cling::Value*, cling::Transaction**, unsigned long) + 520
8  cling     0x00005a1b1aaeb632 cling::Interpreter::process(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char>> const&, cling::Value*, cling::Transaction**, bool) + 338
9  cling     0x00005a1b1ab3d407 cling::MetaProcessor::process(llvm::StringRef, cling::Interpreter::CompilationResult&, cling::Value*, bool) + 599
10 cling     0x00005a1b1ac1abbf cling::UserInterface::runInteractively(bool) + 623
11 cling     0x00005a1b1a8f2e91 main + 3073
12 libc.so.6 0x000077e69a22a1ca
13 libc.so.6 0x000077e69a22a28b __libc_start_main + 139
14 cling     0x00005a1b1a9e12e5 _start + 37
PLEASE submit a bug report to https://github.com/llvm/llvm-project/issues/ and include the crash backtrace.
Stack dump:
0.      Program arguments: ./bin/cling
Floating point exception (core dumped)
```


