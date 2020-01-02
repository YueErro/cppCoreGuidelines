# P: Philosophy
Philosophical rules, good practice.

## Table of contents
* [P.1: Express ideas directly in code](#p1-express-ideas-directly-in-code)
* [P.3: Express intent](#p3-express-intent)
* [P.5: Prefer compile time checking to run-time checking](#p5-prefer-compile-time-checking-to-run-time-checking)
* [P.9: Don't waste time or space](#p9-dont-waste-time-or-space)

### P.1: Express ideas directly in code
```cpp
class Date{
  // ...
public:
  Month month() const; // do
  int month();         // do not
  // ...
};
```

### P.3: Express intent
```cpp
// for each statements
for (const auto& v : vec){} // get v
for (auto& v : vec){} // modify v
```

### P.5: Prefer compile time checking to run-time checking
<details>
  <summary>Compile time errors</summary>
  <ul>
    <li>Syntax error</li>
    <li>Semantic error</li>
    <li>Typos</li>
    <li>Undeclared variables</li>
    <li>Mixing types</li>
    <li>Missing headers</li>
    <li>Wrong number of parameters in a call</li>
  </ul>
</details>
<details>
  <summary>Run time errors</summary>
  <ul>
    <li>Linking error</li>
    <li>Division by zero</li>
    <li>Dereferencing a null pointer</li>
    <li>Out of memory</li>
    <li>System error</li>
  </ul>
</details>

### P.9: Don't waste time or space
```cpp
int vecSize = strlen(s);
for (int i = 0; i < vecLen; i++){}    // do
for (int i = 0; i < strlen(s); i++){} // do not
```
