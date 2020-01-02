# C: Classes and class hierarchies
A class is a user-defined type.

## Table of contents
* [C.2: Use `class` if the class has an invariant; use `struct` if the data members can vary independently](#c2-use-class-if-the-class-has-an-invariant-use-struct-if-the-data-members-can-vary-independently)
* [C.7: Don't define a class or enum and declare a variable of its type in the same statement](#c7-dont-define-a-class-or-enum-and-declare-a-variable-of-its-type-in-the-same-statement)
* [C.ctor: Constructors](#cctor-constructors)
  * [C.40: Define a construtor if a class has an invariant](#c40-define-a-constructor-if-a-class-has-an-invariant)
* [C.hier: Class hierarchies (OOP)](#chier-class-hierarchies-oop)
 * [C.121: If a base class is used as an interface, make it a pure abstract class](#c121-if-a-base-class-is-used-as-an-interface-make-it-a-pure-abstract-class)
* [C.hierclass: Designing classes in a hierarchy](#chierclass-designing-classes-in-a-hierarchy)
  * [C.128: Virtual functions should specify exactly one of `virtual`, `override`, or `final`](#c128-virtual-functions-should-specify-exactly-one-of-virtual-override-or-final)
* [C.hier-access: Accessing objects in a hierarchy](#chier-access-accessing-objects-in-a-hierarchy)
  * [C.149: Use `unique_ptr` or `shared_ptr` to avoid forgetting to `delete` objects created using `new`](#c149-use-unique_ptr-or-shared_ptr-to-avoid-forgetting-to-delete-objects-created-using-new)
  * [C.150: Use `make_unique()` to construct objects owned by `unique_ptrs`](#c150-use-make_unique-to-construct-objects-owned-by-unique_ptrs)
  * [C.151: Use `make_shared()` to construct objects owned by `shared_ptrs`](#c151-use-make_shared-to-construct-objects-owned-by-shared_ptrs)

### C.2: Use `class` if the class has an invariant; use `struct` if the data members can vary independently
```cpp
struct Pair{
  // by default public
  string name;
  int volume;
};

class Date{
public:
  Date(int yy, Mont mm, char dd);
  // ..
private:
  int y;
  Month m;
  char d;
};
```
_C.8: Use class rather than struct if any member is non-public_

### C.7: Don't define a class or enum and declare a variable of its type in the same statement
```cpp
struct Data{};
Data data{};          // do
struct Data{} data{}; // do not
```

### C.ctor: Constructors

#### C.40: Define a constructor if a class has an invariant
```cpp
class Date{
  Date(int dd, int mm, int yy) : d{dd}, m{mm}, y{yy}{
    if ( !is_valid(d, m, y) ) throw Bad_date(); // enforce invariant
  }
private:
  int d, m, y;
};
```

### C.hier: Class hierarchies (OOP)

#### C.121: If a base class is used as an interface, make it a pure abstract class
```cpp
class My_interface{
public:
  // ... only pure virtual functions (virtual f() = 0) ...
  virtual ~My_interface(){} // or = default
};
```

### C.hierclass: Designing classes in a hierarchy

#### C.128: Virtual functions should specify exactly one of `virtual`, `override`, or `final`
- `virtual`: this is a new virtual function
- `override`: this is a non-final overrider (it matches its base clase)
- `final`: this is a final overrider (it can't be overridden)

### C.hier-access: Accessing objects in a hierarchy

#### C.149: Use `unique_ptr` or `shared_ptr` to avoid forgetting to `delete` objects created using `new`
```cpp
void use(int i){
  auto p = new int {7};         // bad: initialize local pointers with new
  auto q = make_unique<int>{9}; // ok: guarantee the release of the memory-allocated for 9
  if (0 < i) return;            // maybe return and leak
  delete p;                     // to late
}
```

#### C.150: Use `make_unique()` to construct objects owned by `unique_ptrs`
```cpp
auto q = make_unique<Foo>(7);     // do
unique_ptr<Foo> p { new Foo{7} }; // do not
```

#### C.151: Use `make_shared()` to construct objects owned by `shared_ptrs`
```cpp
void test(){
  auto q = make_shared<Bar>(7);     // do
  shared_ptr<Bar> p { new Bar{7} }; // do not
}
```
