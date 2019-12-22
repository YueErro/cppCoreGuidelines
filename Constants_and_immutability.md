# Con: Constants and immutability

## Table of contents
* [Con.2: By default, make member functions `const`](#con2-by-default-make-member-functions-const)

### Con.2: By default, make member functions `const`
```cpp
class Point{
  int x, y;
public:
  int getX() const{ // do
  int getX(){       // do not
    return x;
  }
}
```
