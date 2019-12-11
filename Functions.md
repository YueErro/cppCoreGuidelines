# F: Functions
A function definition is a function declaration that also specifies the function's implementation, the function body. Try to name it meaningfully.

## Table of contents
* [F.def: Function definition](#fdef-function-definitions)
  * [F.5: If a function is very small and time-critical, declare it inline](#f5-if-a-function-is-very-small-and-time-critical-declare-it-inline)
  * [F.7: For general use, take T* or T& arguments rather than smart pointers](#7-for-general-use-take-t-or-t-arguments-rather-than-smart-pointers)
* [F.call: Parameter passing](#fcall-parameter-passing)
  * [F.21: To return multiple "out" values, prefer returning a struct or tuple](#f21-to-return-multiple-out-values-prefer-returning-a-struct-or-tuple)
  * [F.23: Use a not_null<T> to indicate that "null" is not a valid value](#f23-use-a-not_nullt-to-indicate-that-null-is-not-a-valid-value)

### F.def: Function definitions

#### F.5: If a function is very small and time-critical, declare it inline
```cpp
inline string cat(const string& s, const string& s2){
  return s + s2;
}  
```
_An inline function is part of the ABI_

#### F.7: For general use, take T* or T& arguments rather than smart pointers
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

#### F.21: To return multiple "out" values, prefer returning a struct or tuple
```cpp
tuple<int, string> f(const string& input){
  // ...
  return std::make_tuple( status, something() );
}
```

#### F.23: Use a not_null<T> to indicate that "null" is not a valid value
```cpp
// it is the caller's job to make sure p != nullptr
int length(not_null<Record*> p);
// the implementor of legth() must assume that p == nullptr is possible
int length(Record* p);
```
