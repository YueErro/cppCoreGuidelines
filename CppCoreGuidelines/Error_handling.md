# E: Error handling
Detect, transmit information about the error, preserve a valid state of the program and avoid resource leaks.

## Table of contents
* [E.12: Use `noexcept` when existing a function because of a `throw` is impossible or unacceptable](#e12-use-noexcept-when-exiting-a-function-because-of-a-throw-is-impossible-or-unacceptable)
* [E.15: Catch exceptions from a hierarchy by reference](#e15-catch-exceptions-from-a-hierarchy-by-reference)

### E.12: Use `noexcept` when existing a function because of a `throw` is impossible or unacceptable
```cpp
double compute(double d) noexcept{
  return log( sqrt(d <= 0 ? 1 : d) )
}
```

### E.15: Catch exceptions from a hierarchy by reference
```cpp
void f(){
  try{
    // ...
  }
  catch (const exception& e){ // do
  catch (exception& e){       // not bad
  catch (exception e){        // do not
    // ...
  }
}
```
