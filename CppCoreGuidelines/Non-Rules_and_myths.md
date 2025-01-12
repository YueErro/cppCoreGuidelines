# N: Non-Rules and myths

Not to follow the rule below.

## Table of contents

- [N: Non-Rules and myths](#n-non-rules-and-myths)
  - [Table of contents](#table-of-contents)
    - [NR.2: Don't: Have only a single `return`-statement in a function](#nr2-dont-have-only-a-single-return-statement-in-a-function)

### NR.2: Don't: Have only a single `return`-statement in a function

```cpp
template<class T>
string sign(T x){
  if (x < 0)
    return "negative";
  else if (x > 0)
    return "positive";
  return "zero";
}
```
