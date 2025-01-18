# R: Resource management

Rules related to resources such as leaks.

## Table of contents

- [R: Resource management](#r-resource-management)
  - [Table of contents](#table-of-contents)
    - [R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialization)](#r1-manage-resources-automatically-using-resource-handles-and-raii-resource-acquisition-is-initialization)
    - [R.5: Prefer scoped objects, don't heap-allocate unnecessarily](#r5-prefer-scoped-objects-dont-heap-allocate-unnecessarily)
    - [R.alloc: Allocation and deallocation](#ralloc-allocation-and-deallocation)
      - [R.12: Immediately give the result of an explicit resource allocation to a manager object](#r12-immediately-give-the-result-of-an-explicit-resource-allocation-to-a-manager-object)
      - [R.13: Perform at most one explicit resource allocation in a single expression statement](#r13-perform-at-most-one-explicit-resource-allocation-in-a-single-expression-statement)
      - [R.14: Avoid `[]` parameters, prefer `span`](#r14-avoid--parameters-prefer-span)
    - [R.smart: Smart pointers](#rsmart-smart-pointers)
      - [R.21: Prefer `unique_ptr` over `shared_ptr` unless you need to share ownership](#r21-prefer-unique_ptr-over-shared_ptr-unless-you-need-to-share-ownership)
      - [R.24: Use `std::weak_ptr` to break cycles of `shared_ptr`](#r24-use-stdweak_ptr-to-break-cycles-of-shared_ptr)
      - [R.32: Take a `unique_ptr<Widget>` parameter to express that a function assumes ownership of a `widget`](#r32-take-a-unique_ptrwidget-parameter-to-express-that-a-function-assumes-ownership-of-a-widget)
      - [R.33: Take a `unique_ptr<widget>&` parameter to express that a function reseats the `widget`](#r33-take-a-unique_ptrwidget-parameter-to-express-that-a-function-reseats-the-widget)

### R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialization)

```cpp
class Port{
  PortHandle port;
public:
  Port(cstring_span destination) : port{ open_port(destination) }{
  }
  ~Port(){
    close_port(port);
  }
};
```

### R.5: Prefer scoped objects, don't heap-allocate unnecessarily

```cpp
// do
void f(int n){
  Gadget g{n};
  // ...
}
// do not
void f(int n){
  auto p = new Gadget{n};
  // ...
  delete p;
}
```

### R.alloc: Allocation and deallocation

#### R.12: Immediately give the result of an explicit resource allocation to a manager object

```cpp
#include <fstream>

void func(const std::string & name){
  std::ifstream f{name}; // open the file
  std::vector<char> buf(1024);
}
```

#### R.13: Perform at most one explicit resource allocation in a single expression statement

```cpp
void fun(shared_ptr<Widget> sp1, shared_ptr<Widget> sp2);
fun( make_shared<Widget>(a, b), make_shared<Widget>(c, d) );                            // do
fun( shared_ptr<Widget>( new Widget(a, b) ), shared_ptr<Widget>( new Widget(c, d) ) );  // do not
```

#### R.14: Avoid `[]` parameters, prefer `span`

```cpp
void f(gsl::span<int>); // do
void f( int[] );        // do not
void f(int*);           // do not
```

### R.smart: Smart pointers

#### R.21: Prefer `unique_ptr` over `shared_ptr` unless you need to share ownership

```cpp
void f(){
  unique_ptr<Base> base = make_unique<Derived>(); // do
  shared_ptr<Base> bae = make_shared<Derived>();  // do not
} // destroy base
```

#### R.24: Use `std::weak_ptr` to break cycles of `shared_ptr`

```cpp
#include <memory>
class bar;
class foo{
public:
  explicit foo(const std::shared_ptr<bar>& forward_reference) : forward_reference_(forward_reference){
  }
private:
  std::shared_ptr<bar> forward_reference_;
};

class bar{
public:
  explicit bar(const std::weak_ptr<foo>& back_reference) : back_reference_(back_reference){
  }
  void do_something(){
    if ( auto shared_back_reference = back_reference_.lock() ){
      // Use *shared_back_reference
    }
  }
private:
  std::weak_ptr<foo> back_reference_;
};
```

#### R.32: Take a `unique_ptr<Widget>` parameter to express that a function assumes ownership of a `widget`

```cpp
void sink(unique_ptr<widget>);          // takes ownership of the widget
void uses(widget*);                     // just uses the widget
void thinko(const unique_ptr<widget>&); // usually not wanted
```

#### R.33: Take a `unique_ptr<widget>&` parameter to express that a function reseats the `widget`

```cpp
void reseat(unique_ptr<widget>&);       // "will" or "might" reseat pointer
void thinko(const unique_ptr<widget>&); // usually not wanted
```
