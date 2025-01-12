# T: Templates and generic programming

Generic programming is programming using types and algorithms parameterized by types, values, and algorithms. In C++, generic programming is supported by the `template` language mechanisms.

## Table of contents

- [T: Templates and generic programming](#t-templates-and-generic-programming)
  - [Table of contents](#table-of-contents)
    - [T.con-use: Concept use](#tcon-use-concept-use)
      - [T.12: Prefer concept names over `auto` for local variables](#t12-prefer-concept-names-over-auto-for-local-variables)
    - [Template interfaces](#template-interfaces)
      - [T.43: Prefer `using` over `typedef` for defining aliases](#t43-prefer-using-over-typedef-for-defining-aliases)

### T.con-use: Concept use

#### T.12: Prefer concept names over `auto` for local variables

```cpp
vector<string> v{"abc", "xyz"};
String& s = v.front();  // do
auto& x = v.front();    // do not
```

### Template interfaces

#### T.43: Prefer `using` over `typedef` for defining aliases

```cpp
using PFI2 = int (*)(int) // do
typdef int (*PFI)(int)    // do not
```