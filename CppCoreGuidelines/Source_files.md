# SF: Source files

Distinguish between declarations (used as interfaces) and definitions (used as implementations). Use header files to represent interfaces and to emphasize logical structure.

## Table of contents

- [SF: Source files](#sf-source-files)
  - [Table of contents](#table-of-contents)
    - [SF.4: Include `.h` files before other declarations in a file](#sf4-include-h-files-before-other-declarations-in-a-file)

### SF.4: Include `.h` files before other declarations in a file

_SF.1: Use a `.cpp` suffix for code files and `.h` for interface files if your project doesn’t already follow another convention_

```cpp
// foo.cpp
#include <foo.h>

#include <vector>
#include <algorithm>
#include <string>
// ... my code here ...
```

_SF.5: A `.cpp` file must include the `.h` file(s) that defines its interface_
