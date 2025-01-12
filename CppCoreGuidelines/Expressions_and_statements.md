# ES: Expressions and statements

They are the lowest and most direct way of expressing actions and computation. Declarations in local scopes are statemets.

## Table of contents

- [ES: Expressions and statements](#es-expressions-and-statements)
  - [Table of contents](#table-of-contents)
    - [Es.dcl: Declarations](#esdcl-declarations)
      - [ES.11: Use `auto` to avoid redundat repetition of type names](#es11-use-auto-to-avoid-redundat-repetition-of-type-names)
      - [ES.12: Do not reuse names in nested scopes](#es12-do-not-reuse-names-in-nested-scopes)
      - [ES.23: Prefer the `{}`-initializer syntax](#es23-prefer-the--initializer-syntax)
      - [ES.27: Use `std::array` or `stack_array` for arrays on the stack](#es27-use-stdarray-or-stack_array-for-arrays-on-the-stack)
      - [ES.31: Don't use macros for constants or "functions"](#es31-dont-use-macros-for-constants-or-functions)
    - [ES.expr: Expressions](#esexpr-expressions)
      - [ES.49: If you must use a cast, use a named cast](#es49-if-you-must-use-a-cast-use-a-named-cast)
      - [ES.61: Delete arrays using `delete[]` and non-arrays using `delete`](#es61-delete-arrays-using-delete-and-non-arrays-using-delete)
      - [ES.65: Don't dereference an invalid pointer](#es65-dont-dereference-an-invalid-pointer)
    - [ES.stmt: Statements](#esstmt-statements)
      - [ES.70: Prefer a `switch`-statement to an `if`-statement when there is a choice](#es70-prefer-a-switch-statement-to-an-if-statement-when-there-is-a-choice)
      - [ES.85: Make empty statements visible](#es85-make-empty-statements-visible)
    - [Arithmetic](#arithmetic)
      - [ES.107: Don't use `unsigned` for subscripts, prefer `gsl::index`](#es107-dont-use-unsigned-for-subscripts-prefer-gslindex)

### Es.dcl: Declarations

#### ES.11: Use `auto` to avoid redundat repetition of type names

```cpp
auto p = v.begin(); // vector<int>::iterator
auto h = t.future();
auto q = make_unique<int[]>(s);
auto f = [](int x){return x + 10;};
```

#### ES.12: Do not reuse names in nested scopes

```cpp
// This is bad and is called Shadowing
struct S{
  int m;
  void f(int x);
};

void S::f(int x){
  m = 7;    // assign to member
  if (x) {
    int m = 9;
    // ...
    m = 99; // assign to local variable
    // ...
  }
}
```

#### ES.23: Prefer the `{}`-initializer syntax

```cpp
int x {f(99)};
int y = x;
vector<int> v{1, 2, 3, 4, 5, 6};
```

_Avoid `()` initialization, which allows parsing ambiguities._

_Use `=` only when you are sura thet there can be no narrowing conversions._

_For built-in arithmetic types, use `=` only with `auto`._

#### ES.27: Use `std::array` or `stack_array` for arrays on the stack

```cpp
const int n = 3;
int m = 9;

void f(){
  std::array<int, n> a1;       // [0, 1, 2]
  gsl::stack_array<int> a2(m);
  // ...
}
```

#### ES.31: Don't use macros for constants or "functions"

```cpp
#define PI 3.14             // do not
constexpr double pi = 3.14  // do
```

### ES.expr: Expressions

#### ES.49: If you must use a cast, use a named cast

- `static_cast`: `compile-time` cast, use it in cases like converting `float` to `int`, `char` to `int`, etc.
- `const_cast`: casts away the constness of variables.
- `reinterpret_cast`: converts one pointer of another pointer of any type.
- `dynamic_cast`: `run-time cast`, converts `base-class` pointers into `derived-class` pointers (downcasting).
- `std::move`: takes an object and allows you to treat it as an `rvalue`.
- `std::forward`: allows `rvalue` arguments to be passed on as `rvalue`, and `lvalue` to be passed on as `lvalue`.
- `gsl::narrow_cast`: same as `static_cast`.
- `gsl::narrow`: as `static_cast` but if not possible throws `narrowing_error`.

#### ES.61: Delete arrays using `delete[]` and non-arrays using `delete`

```cpp
void f(int n){
  auto p = new X[n];
  void* ptr;
  // ...
  delete[] p;
  delete ptr;
}
```

#### ES.65: Don't dereference an invalid pointer

```cpp
void f(not_null<int*> p){
  int x = *p;
}
```

### ES.stmt: Statements

#### ES.70: Prefer a `switch`-statement to an `if`-statement when there is a choice

```cpp
void use(int n){
  // do
  switch (n){
    case 0:
      // ...
      break;
    case 7:
      // ...
      break;
    default:
      // ...
      break;
  }
  ```

  _ES.78: Always end a non-empy `case` with a `break`._

  _ES.79: Use `default` to handle common cases (only)._

  ```cpp
  // do not if if-then-else chain comparing against a set of constants
  void use2{
    if (n == 2)
      // ...
    else if (n == 7)
      // ...
  }
}
```

#### ES.85: Make empty statements visible

```cpp
for (i = 0; i < max; i++);  // do not
for (auto x : v){           // do
}
v[i] = f( v[i] )
```

### Arithmetic

#### ES.107: Don't use `unsigned` for subscripts, prefer `gsl::index`

```cpp
std::vector<int> vec = /*...*/;
for (gsl::index i = 0; i < vec.size(); i++) // do
for (int i = 0; i < vec.size(); i++)        // may not be big enough
for (unsigned i = 0; i < vec.size(); i++)   // risk wraparound
for (auto i = 0; i < vec.size(); i++)       // bug
  std::cout << vec[i] << '\n';
```
