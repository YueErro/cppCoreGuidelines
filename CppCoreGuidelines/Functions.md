# F: Functions

A function definition is a function declaration that also specifies the function's implementation, the function body. Try to name it meaningfully.

## Table of contents

- [F: Functions](#f-functions)
  - [Table of contents](#table-of-contents)
    - [F.def: Function definitions](#fdef-function-definitions)
      - [F.5: If a function is very small and time-critical, declare it inline](#f5-if-a-function-is-very-small-and-time-critical-declare-it-inline)
      - [F.7: For general use, take `T*` or `T&` arguments rather than smart pointers](#f7-for-general-use-take-t-or-t-arguments-rather-than-smart-pointers)
    - [F.call: Parameter passing](#fcall-parameter-passing)
      - [F.18: For "will-move-from" parameters, pass by `X&`, `&` and `std::move` the parameter](#f18-for-will-move-from-parameters-pass-by-x--and-stdmove-the-parameter)
      - [F.23: For "forward" parameters, pass by `TP&&` and only `std::forward` the parameter](#f23-for-forward-parameters-pass-by-tp-and-only-stdforward-the-parameter)
      - [F.21: To return multiple "out" values, prefer returning a struct or tuple](#f21-to-return-multiple-out-values-prefer-returning-a-struct-or-tuple)
      - [F.23: Use a `not_null<T>` to indicate that "null" is not a valid value](#f23-use-a-not_nullt-to-indicate-that-null-is-not-a-valid-value)
      - [F.44: Return a `T&` when copy is undesirable and "returning no object" isn't needed](#f44-return-a-t-when-copy-is-undesirable-and-returning-no-object-isnt-needed)
      - [F.52: Prefer capturing by reference in lambdas that will be used locally, including passed algorithms](#f52-prefer-capturing-by-reference-in-lambdas-that-will-be-used-locally-including-passed-algorithms)
      - [F.53: Avoid capturing by reference in lambdas that will be used non-locally, including returned, stored on the heap, or passed to another thread](#f53-avoid-capturing-by-reference-in-lambdas-that-will-be-used-non-locally-including-returned-stored-on-the-heap-or-passed-to-another-thread)
      - [F.54: When writing a lambda that captures `this` or any class data member, don't use \[=\] default capture](#f54-when-writing-a-lambda-that-captures-this-or-any-class-data-member-dont-use--default-capture)


### F.def: Function definitions

#### F.5: If a function is very small and time-critical, declare it inline

```cpp
inline string cat(const string& s, const string& s2){
  return s + s2;
}
```

_An inline function is part of the ABI_.

#### F.7: For general use, take `T*` or `T&` arguments rather than smart pointers

```cpp
// accepts any int*
void f(int*);
// can only accept ints for which you want to transfer ownership
void g(unique_ptr<int>);
// can only accept ints for which you are willing to share ownership
void g(shared_ptr<int>);
// doesn't change ownership, but requires a particular ownership of the caller
void h(const unique_ptr<int>&)
// accepts any int
void h(int&);
```

### F.call: Parameter passing

#### F.18: For "will-move-from" parameters, pass by `X&`, `&` and `std::move` the parameter

```cpp
// Does not apply to smart pointers
void sink(vector<int>&& v)  // sink takes ownership of whatever the argument owned
{
  // Usually there might be const accesses of v here
  store_somewhere(std::move(v));
  // Usually no more use of v here; it is moved-from
}
```

#### F.23: For "forward" parameters, pass by `TP&&` and only `std::forward` the parameter

```cpp
template<class F, class... Args>
inline decltype(auto) invoke(F&& f, Args&&... args)
{
  return forward<F>(f)(forward<Args>(args)...);
}
```

```cpp
template<class PairLike>
inline auto test(PairLike&& pairlike)
{
  // ...
  f1(some, args, and, forward<PairLike>(pairlike).first);           // forward .first
  f2(and, forward<PairLike>(pairlike).second, in, another, call);   // forward .second
}
```

#### F.21: To return multiple "out" values, prefer returning a struct or tuple

```cpp
tuple<int, string> f(const string& input){
  // ...
  return std::make_tuple( status, something() );
}
```

#### F.23: Use a `not_null<T>` to indicate that "null" is not a valid value

```cpp
// it is the caller's job to make sure p != nullptr
int length(not_null<Record*> p);
// the implementor of legth() must assume that p == nullptr is possible
int length(Record* p);
```

#### F.44: Return a `T&` when copy is undesirable and "returning no object" isn't needed

```cpp
class Car{
public:
  wheel& getWheel(int i) {
    Expects(i < w.size());
    return w[i];
  }
private:
  array<wheel, 4> w;
  // ...
};

void use(){
  Car c;
  wheel& w0 = c.getWheel(0); // w0 has the same lifetime as c
}
```

#### F.52: Prefer capturing by reference in lambdas that will be used locally, including passed algorithms

```cpp
std::for_each(begin(sockets), end(sockets), [&message](auto& socket)
{
  socket.send(socket);
});
```

#### F.53: Avoid capturing by reference in lambdas that will be used non-locally, including returned, stored on the heap, or passed to another thread

```cpp
int local = 42;
// Want a copy of local
// Since a copy of local is made, it will always be available for the call
thread_poool.queue_work([=]
{
  process(local);
});
```

_Note: If a non-local pointer must be captured, consider using `unique_ptr`; this handles both lifetime and synchronization.
If the `this` pointer must be captured, consider using `[*this]` capture, which creates a copy of the entire object_.

#### F.54: When writing a lambda that captures `this` or any class data member, don't use [=] default capture

```cpp
class MyClass{
  int x = 0;
  // ...

  void f(){
    int i = 0;
    // ...

    // BAD: looks like copy/value capture
    auto lambda = [=]{ use(i, x); };

    x = 42;
    lambda(); // calls use(0, 42);
    x= 43;
    lambda(); // calls use (0, 439);

    //...

    // OK: most explicit and least confusing
    auto lambda2 = [i, this] { use(i, x); };

    // ...
  }
};
```

_Note if you inted to capture a copy of all class data members, consider C++17 `[*this]`._
