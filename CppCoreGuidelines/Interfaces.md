# I: Interfaces

Contract between two parts of a program.

## Table of contents

- [I: Interfaces](#i-interfaces)
  - [Table of contents](#table-of-contents)
    - [I.6: Prefer `Expects()` for expressing preconditions](#i6-prefer-expects-for-expressing-preconditions)
    - [I.7: State postconditions](#i7-state-postconditions)
    - [I.9: If and interface is a template, document its parameters using concepts](#i9-if-and-interface-is-a-template-document-its-parameters-using-concepts)
    - [I.12: Declare a pointer that must not be null as `not_null`](#i12-declare-a-pointer-that-must-not-be-null-as-not_null)
    - [I.23: Keep the number of function arguments low](#i23-keep-the-number-of-function-arguments-low)
    - [I.25: Prefer abstract classes as interfaces to class hierarchies](#i25-prefer-abstract-classes-as-interfaces-to-class-hierarchies)

### I.6: Prefer `Expects()` for expressing preconditions

```cpp
Expects(height > 0 && width > 0); // good
if (height <= 0 || width <= 0)    // obscure
  my_error();
```

_Use assert() when it does not make sense to continue, otherwise use Expect()_

### I.7: State postconditions

```cpp
int area (int height, int width){
  auto res = height * width;
  Ensures(res > 0);      // good
  return res;
  return height * width; // obscure
}
```

### I.9: If and interface is a template, document its parameters using concepts

```cpp
// C++20
template<typename Iter, typename Val> requires input_iterator<Iter> &&  equality_comparable_with<iter_value_t<Iter>,Val>
Iter find(Iter first, Iter last, Val v)
{
  // ...
}
```

### I.12: Declare a pointer that must not be null as `not_null`

```cpp
int length(not_null<const char*> p);  // cannot be nullptr
int length(const char* p);            // not clear
```

### I.23: Keep the number of function arguments low

- Try to use fewer than four parameters
- Use better abstraction: Group arguments into meaningful objects and pass the objects

### I.25: Prefer abstract classes as interfaces to class hierarchies

```cpp
class Shape{ // It does never have constructor
public:
  virtual Point center() const = 0;  // do
  Point center() const{ return c; }; // do not
  virtual void draw() const = 0; // pure virtual (abstract) fuction
  // ... no data members ...
  virtual ~Shape() = default;
};
```
