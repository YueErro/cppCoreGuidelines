# NL: Naming and layout rules
Set of rules that you might use if you have no better ideas, but the real aim is consistency, rather than any particular rule set.

## Table of contents
* [NL.16 a convetional class member declaration order](#nl16-use-a-conventional-class-member-declaration-order)
* [NL.18: Use C++ style declarator layout](#nl18-use-c-style-declarator-layout)
* [NL.25: Don't use `void` as an argument type](#nl25-dont-use-void-as-an-argument-type)
* [NL.26: Use conventional `const` notation](#nl26-use-conventional-const-notation)

### NL.16 a convetional class member declaration order
```cpp
class X{
public:
  // interface
protected:
  // unchecked function for use by derived class implementations
private:
  // implementation details
};
```

### NL.18: Use C++ style declarator layout
```cpp
T& operator[](size_t);  // do
T &operator[](size_t);  // weird
T & operator[](size_t); // do not
```

### NL.25: Don't use `void` as an argument type
```cpp
void f();     // do
void g(void); // do not
```

### NL.26: Use conventional `const` notation
```cpp
const int x = 7;  // do
int const y = 9;  // do not
const int *const p = nullptr; // do
int const *const p = nullptr; // do not
```
