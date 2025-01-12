# C: Classes and class hierarchies

A class is a user-defined type.

## Table of contents

- [C: Classes and class hierarchies](#c-classes-and-class-hierarchies)
  - [Table of contents](#table-of-contents)
    - [C.2: Use `class` if the class has an invariant; use `struct` if the data members can vary independently](#c2-use-class-if-the-class-has-an-invariant-use-struct-if-the-data-members-can-vary-independently)
    - [C.7: Don't define a class or enum and declare a variable of its type in the same statement](#c7-dont-define-a-class-or-enum-and-declare-a-variable-of-its-type-in-the-same-statement)
    - [C.ctor: Constructors](#cctor-constructors)
      - [C.40: Define a constructor if a class has an invariant](#c40-define-a-constructor-if-a-class-has-an-invariant)
    - [C.copy: Copy and move](#ccopy-copy-and-move)
      - [C.67: A polymorphic class should suppress public copy/move](#c67-a-polymorphic-class-should-suppress-public-copymove)
    - [C.other: Other default operation rules](#cother-other-default-operation-rules)
      - [C.89: Make a `hash` `noexcept`](#c89-make-a-hash-noexcept)
    - [C.hier: Class hierarchies (OOP)](#chier-class-hierarchies-oop)
      - [C.121: If a base class is used as an interface, make it a pure abstract class](#c121-if-a-base-class-is-used-as-an-interface-make-it-a-pure-abstract-class)
    - [C.hierclass: Designing classes in a hierarchy](#chierclass-designing-classes-in-a-hierarchy)
      - [C.128: Virtual functions should specify exactly one of `virtual`, `override`, or `final`](#c128-virtual-functions-should-specify-exactly-one-of-virtual-override-or-final)
      - [C.138: Create an overload set for a derived class and its bases with `using`](#c138-create-an-overload-set-for-a-derived-class-and-its-bases-with-using)
    - [C.hier-access: Accessing objects in a hierarchy](#chier-access-accessing-objects-in-a-hierarchy)
      - [C.147: Use `dynamic_cast` to a reference type when failure to find the required class is considered an error](#c147-use-dynamic_cast-to-a-reference-type-when-failure-to-find-the-required-class-is-considered-an-error)
      - [C.149: Use `unique_ptr` or `shared_ptr` to avoid forgetting to `delete` objects created using `new`](#c149-use-unique_ptr-or-shared_ptr-to-avoid-forgetting-to-delete-objects-created-using-new)
      - [C.150: Use `make_unique()` to construct objects owned by `unique_ptrs`](#c150-use-make_unique-to-construct-objects-owned-by-unique_ptrs)
      - [C.151: Use `make_shared()` to construct objects owned by `shared_ptrs`](#c151-use-make_shared-to-construct-objects-owned-by-shared_ptrs)

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

_C.8: Use class rather than struct if any member is non-public_.

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

### C.copy: Copy and move

#### C.67: A polymorphic class should suppress public copy/move

```cpp
class B{
public:
  B() = default;
  B(const B&) = delete;
  B& operator=(const B&) = delete;
  virtual char m() {return  'B';}
  // ...
};
class D: public B{
public:
  char m() override {return 'D';}
  // ...
};
void f(B& b){
  auto b2 = b; // OK: Compiler will detect inadvertent copying, and protest instead of returning 'B'
}

D d;
f(d);
```

### C.other: Other default operation rules

#### C.89: Make a `hash` `noexcept`

```cpp
struct MyKey{
  int val_int = 5;
  double val_dou = 5.5;
};
struct MyHash{
  std::size_t operator() (MyKey m) const{
    std::hash<int> has_val1;
    std::has<double> has_val2;
    return has_val1(m.val_int) ^ hash_val2(m.val_dou);
  }
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

#### C.138: Create an overload set for a derived class and its bases with `using`

```cpp

class B
{
public:
  virtual int f(int i){
    std::cout << "f(int): ";
    return i;
  }
  virtual double f(double d){
    std::cout << "f(double): ";
    return d;
  }
  virtual ~B() = default;
};
class D: public B{
public:
  int f(int i) override {
    std::cout << "f(int): ";
    return i + 1;
  }
  using B::f; // exposes (double)
};
```

### C.hier-access: Accessing objects in a hierarchy

#### C.147: Use `dynamic_cast` to a reference type when failure to find the required class is considered an error

```cpp
std::string f(Base& b)
{
  return dynamic_cast<Derived&>(b).to_string();
}
```

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
