# SL: The Standard Library

## Table of contents

- [SL: The Standard Library](#sl-the-standard-library)
  - [Table of contents](#table-of-contents)
    - [SL.con: Containers](#slcon-containers)
      - [SL.con.1: Prefer using STL `array` or `vector` instead of a C array](#slcon1-prefer-using-stl-array-or-vector-instead-of-a-c-array)
    - [SL.io: Iostream](#slio-iostream)
      - [SL.io.50: Avoid `endl`](#slio50-avoid-endl)

### SL.con: Containers

#### SL.con.1: Prefer using STL `array` or `vector` instead of a C array

```cpp
std::vector<int> w(initial_size); // do
int* v = new int[initial_size];   // do not
delete[] v;
```

### SL.io: Iostream

#### SL.io.50: Avoid `endl`

```cpp
std::cout << "Hello, world!\n";             // do
std::cout << "Hello, world!" << std::endl;  // do not
```
