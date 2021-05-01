# codeshaunted Style Guide For C++
## Table Of Contents
Coming soon!
##  Introduction
The following style guide is what's used on all codeshaunted projects written in C++. It is very opinionated and designed for high efficiency reading and writing of code. If you don't like it, fork it! Pull requests will most likely be denied. Issues are allowed if questions are raised or discussion is warranted.
## Project Structure
All projects' directory trees structured according to the following rules. An example directory tree is included below them. 
#### The Rules
- All projects should an `include` folder and a `source` directory (unless it is a header only project). Within these directories a sub directory named after the project should be included. This is especially helpful for library projects, so users can organize includes.
- If a project makes use of any third party libraries, they should be placed in a directory named `third_party`. They should be included using git's sub module feature if at all possible.
```
example
│   .gitignore
|   .gitmodules
|   CONTRIBUTING.md
|   CMakeLists.txt
│   README.md
│
└───include
|   |
│   └───example
│       │   example.hh
│       │   example_manager.hh
|
└───source
│   │   CMakeLists.txt
│   │
│   └───example
|       |   CMakeLists.txt
|       |   example.cc
│       │   example_manager.cc
│       │   main.cc
|
└───third_party
│   │   CMakeLists.txt
│   │
│   └───lib_example
│       │   ...
│   
```
## Naming Conventions
Following this, rules for naming conventions will be laid out. It is important to keep in mind that many of these conventions are optimized for readability and for ease of use with auto completion plugins (Intellisense, etc). For example, as parameters are named beginning with two underscores, when working within a function one can easily identify and find parameters using auto completion by simply typing two underscores.
### Abbreviations
It is generally wise to avoid excessive abbreviation in naming, only use common abbreviations and try not to use them too often. Never name something using a single character unless it is an iterator (`i`) or a template type (`T`). Type and enum names are exempt from these rules.
### Projects
All projects should be named simply with a single word if possible. The project name should be stylized as lowercase. If there is more than one word, use underscores to separate them. If a project is a library, prefix the name with `lib_`.
### Files
As a general rule, files should be named lowercase, with underscores separating multiple words unless it is not common practice to do so (`CMakeLists.txt`, `README.md`, etc).
#### Header Files
Header files should use the extension `.hh`.
#### Source Files
Source files should use the extension `.cc`.
### Variables
Regular local variables should be named lowercase with every word separated by an underscore. 
```cpp
int example_integer = 1;
```
#### Class Member Variables
Private class member variables should be named like local variables, except that they should be prefixed with an underscore. This doesn't apply to public class member variables.
```cpp
class Example {
  public:
    int example_integer;
  private:
    int _example_integer;
};
```
#### Struct Member Variables
Private struct member variables should be named like local variables, except that they should be prefixed with an underscore. This doesn't apply to public struct member variables.
#### Parameters
Parameters should be named like local variables, except they should be prefixed with two underscores.
```cpp
void exampleFunction(int __example_parameter) {}
```
### Functions
Local functions should be named with the first word lowercase and every consecutive word's first letter capitalized.
```cpp
void exampleFunction() {}
```
#### Class Member Functions
Private class member functions should be named like local functions, except that they should be prefixed with an underscore. This doesn't apply to public class member functions.
```cpp
class Example {
  public:
    Example();
    ~Example();
  private:
    void _doExampleStuff();
};
```
#### Struct Member Functions
Private struct member functions should be named like local functions, except that they should be prefixed with an underscore. This doesn't apply to public struct member functions. Also, please stop putting functions into structs. ;)
### Namespaces
Namespaces should be named like projects.
### Primitive Types
Types should be named short, lowercase and be prefixed with `t_`. Heavy abbreviation of words is permissible as long as the abbreviations are somewhat common.
```cpp
typedef unsigned long long t_ui32; // unsigned integer, 32 bit
```
### Classes
Classes should be named lowercase, but with the first letter in every word capitalized. An exception to this is classes designed to be used akin to primitive types, which may be named as if they are primitive types.
```cpp
class Example;
class t_vec3; // designed be used like a primitive type
```
### Structs
Structs should be named lowercase
### Enums
Enums should be named short, lowercase and be prefixed with `e_`. Heavy abbreviation of words is permissible as long as the abbreviations are somewhat common.
```cpp
enum class e_example;
```
#### Enum Members
Enum members should be named fully capitalized with underscores separating words.
```cpp
enum class e_example {
  MEMBER_ZERO,
  MEMBER_ONE,
  MEMBER_TWO
};
```
### Macros
Macros should be named fully capitalized with underscores separating words, as well as beginning with the project name.
```cpp
EXAMPLE_EXAMPLE_MACRO 1
```
#### Macro Parameters
Macro parameters should be named with the first letter in each word capitalized.
```cpp
#define EXAMPLE_EXAMPLE_MACRO(Parameter) Parameter
```
## Building
All projects should be built using CMake. There should be a `CMakeLists.txt` file in the main project directory, as well as `source`, `include` and `third_party` plus all their sub directories. Don't dynamically include source files for build. Explicitly list sources within your lists.
## Header Files
In general, every source file should have a counterpart header file (`main.cc` being a notable exception). The following are some rules for header files.
### Include Guards
All header files should contain an include guard to prevent against multiple inclusion. I am fully aware that `#pragma once` directive exists, but it's non-standard and means different things on different compilers. In the interest of keeping everything working smoothly, classic `#define` include guards should be used. It should consist of an `#ifndef`, a `#define` and an `#endif`. These preprocessor directives should use a macro named according to the path of the header file beginning from the `include` directory. They should be fully capitalized, each directory or file name separated by an underscore, and the `.` between the file extension and the file name replaced with an underscore. All includes and code should go after the `#ifndef` and `#define`, but before the `#endif`.
```cpp
// codeshaunted - example
// example.hh

#ifndef EXAMPLE_EXAMPLE_HH
#define EXAMPLE_EXAMPLE_HH

// code and includes goes here

#endif // EXAMPLE_EXAMPLE_HH
```
### Forward Declarations
Use forward declarations if convenient. They can drastically reduce compile times.
```cpp
// #include "example.hh"
class Example;
```
## Includes
You should only include what you use. If you weren't aware, the preprocessor basically does a glorified copy paste of every included file into the file you included it in. Including things you don't need can lead to longer compile times.
### Include Order
When you include files, stick to the order specified below, with a blank line in between each section.
- If a source file, the relevant header to it (`example.hh` for `example.cc`)
- C system headers (`stdlib.h`, `math.h`, etc)
- C++ standard headers (`iostream`, `algorithm`, etc)
- Third party library headers
- Internal project headers
```cpp
// codeshaunted - example
// example.cc

#include "example.hh"

#include <math.h>

#include <iostream>

#include "lib_example/example.hh"

#include "example_manager.hh"
```
### `<` versus `"`
When including files, you should only include C system headers and C++ standard headers using `<`, all other headers should be included using `"`.
```cpp
#include "example.hh" // internal project header

#include <math.h> // C system header

#include <iostream> // C++ standard header
```
## Namespacing
All code should be within at least one namespace besides the global one. The first namespace all code will be in besides the global namespace is one named after the project. The latter bracket of the namespace expression should be denoted with a comment that states `namespace <namespace name`, `<namespace name>` being a stand-in for the name of the namespace. Code within a namespace shouldn't be indented. Additionally, never make a `using namespace` statement outside of `main.cc` for your project's namespace.
```cpp
// example
// example.cc

#include "example.hh"

namespace example {
namespace stuff {

// code goes here

} // namespace stuff
} // namespace example
```
## Classes
Classes should be used extensively in C++ programming, as they are a fundamental part of C++'s implementation of object oriented programming. The following are some rules for their usage.
### Member Access
All data members of classes (not functions) should be private. If they need to be accessed from outside of the class, implement get and set functions for them. 
```cpp
class Example {
  public:
    getExampleInteger() { return _example_integer; }
  private:
    int _example_integer = 1;
};
```
### Declaration
Classes can have `public:`, `protected:` and `private:` sections, only add the ones that you use. Indent each of these sections and the declarations within them. Within these sections, members should be declared in the following order:
- Constructors and destructors
- static functions
- virtual functions
- all other functions
- static data members
- constant data members
- all other data members

```cpp
class Example {
  public:
    Example();
    ~Example();
    static int getStaticExampleInteger() { return _static_example_integer; }
    virtual int getProtectedExampleInteger() { return _protected_example_integer; }
    int getExampleInteger() { return _example_integer; }
  protected:
    int _protected_example_integer;
  private:
    static int _static_example_integer;
    const int _constant_example_integer = 1;
    int _example_integer = 1;
}
```
## Structs
Structs are very good for passing around grouped chunks of data that don't need any logic implementation. All rules for classes apply to structs, besides the rules regarding member access (if at all possible, struct members should always be public) and the rules for access section specifiers (`public:`, `protected:`, `private:`).
## Enums
Enums are useful for error codes, response codes, etc. There is only one rule to enums, you must use `enum class`, rather than just `enum` when declaring them. Using `enum class` prevents all of an enum's members from being dumped into the global namespace.
```cpp
enum class e_example {
  MEMBER_ZERO,
  MEMBER_ONE,
  MEMBER_TWO
};
```
## Macros
Macros are a very powerful tool for abbreviating code, but the use of macros should usually be avoided, as they can cause bugs that are hard to trace or fix. They also may not behave the same on every preprocessor. Use your best judgement for if you should use them or not. Generally macros should exist only in the file that they are used in and should be deleted using `#undef` after they are done being used.
## Preprocessor Directives
Preprocessor directives can be for a variety of things, selectively enabling features, changing code between platforms, etc. The only rule for preprocessor directives is that every `#endif` tied to an `#ifdef` (not `#if`) must have a comment containing the macro it was checking for a definition for.
```cpp
#ifdef _WIN32

doWindowsStuff();

#endif // _WIN32
```
## Pointers and References
Pointers and references are extremely powerful tool for accessing objects without copying them. References should be used before pointers, but if there is a case where a reference cannot be used (struct or class data members), a pointer should be used instead.
```cpp
// reference
void doExampleStuff(Example& __example) {
  __example.doExampleStuff();
}

// pointer
struct ExampleStuff {
  Example* example; 
}
```
### Smart pointers
Do not use smart pointers, their implementation varies, as well as their performance footprint. If you are going to ignore me anyways, use `std::unique_ptr`, it has the smallest performance footprint overall.
### Pointer Lifetimes
Every `new` call should have at least one or more matching `delete` calls to prevent memory leaks. After deleting a pointer, it should be assigned a value of `nullptr` to prevent undefined behavior if `delete` is called on it again.
```cpp
Example* example = new Example();

example.doExampleStuff();

delete example;
example = nullptr;
```
## Comments
Comments help to explain code to others who may read it (or maybe even yourself a few months later). Use them liberally.
### Formatting
Comments should be lowercase, as well as short and to the point. Don't ramble on. Punctuation is not required. There should always be a space between the `//` or `/*` of the comment and the content of the comment.
```cpp
doExampleStuff(); // does example stuff
```
### File Comments
At the top of every file there should be at least two comments, the first one containing the name of your team and project, separated by a dash. And, a second one with the name of the file. An optional third comment can contain a description of what the file contains. A fourth comment can contain the boilerplate to your license.
```cpp
// codeshaunted - example
// example.cc
```
### `todo` Comments
If something must be fixed or changed later, add a comment that starts with `todo: `, then explains what needs to be changed. This can easily be searched for later when someone has extra time to fix things.
```cpp
doExampleStuff(); // todo: fix example stuff
```
## Formatting and White Space
Excessive white space is forbidden. Despite what some may tell you, it can make your code harder to read.
### Tab Size
Use tabs made of two real space characters. Any modern text editor or IDE should allow you to do this.
### Indents
As a general rule, indent everything unless it is a preprocessor directive or within a namespace.
### Braces
There should be no lines containing only a single open brace, these should be integrated into their function, class, if statement, etc.
```cpp
// good
if (exampleInteger == 1) {
  doExampleStuff();
}

// bad
if (exampleInteger == 1)
{
  doExampleStuff();
}
```
### Blank Lines
Blank lines should be placed in the following places.
- After the beginning of a namespace or before the end.
- Between `#include` sections.
- After the file comment
- Between function, struct, class or enum declarations or definitions
- After the `#define` in your header guard and before the `#endif` in your header guard.
- Anywhere in actual code that makes sense (logical sections for routines).
## C++ Version
Generally, the latest widely implemented C++ version should be used. If there are features missing from a version on multiple common compilers, don't use it. Also consider the portability of older versions like C++11 if developing for more integrated platforms.
