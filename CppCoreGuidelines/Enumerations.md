# Enum: Enumerations

Used to define sets of integer values and for defining types for such sets of values.

## Table of contents

- [Enum: Enumerations](#enum-enumerations)
  - [Table of contents](#table-of-contents)
    - [Enum.1: Prefer enumerations over macros](#enum1-prefer-enumerations-over-macros)
    - [Enum.3: Prefer `enum class` over "plain" `enum`](#enum3-prefer-enum-class-over-plain-enum)
    - [Enum.4: Define operations on enumerations for safe simple use](#enum4-define-operations-on-enumerations-for-safe-simple-use)

### Enum.1: Prefer enumerations over macros

```cpp
// worst
// webcolors.h (third party header)
#define RED 0xFF0000
#define GREEN 0x00FF00
#define BLUE  0x0000FF
// productinfo.h
// The following define product subtypes based on color
#define RED    0
#define PURPLE 1
#define BLUE   2
int webby = BLUE;   // webby == 2; probably not what was desired

// better
enum class Web_color{red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF};
enum class Product_info{red = 0, purple = 1, blue = 2};
Web_color webby = Web_color::blue;
```

### Enum.3: Prefer `enum class` over "plain" `enum`

```cpp
enum class Product_info{ red = 0, purple = 1, blue = 2} // do
enum Product_info{ red = 0, purple = 1, blue = 2}       // do not
```

### Enum.4: Define operations on enumerations for safe simple use

```cpp
//        0    1    2    3    4    5    6
enum Day{mon, tue, wed, thu, fri, sat, sun};
Day& operator++(Day& d){
  return d = (d == Day::sun) ? Day::mon : static_cast<Day>( static_cast<int>(d)+1 )
}
Day today = Day::sat;
Day tomorrow = ++today;
```
