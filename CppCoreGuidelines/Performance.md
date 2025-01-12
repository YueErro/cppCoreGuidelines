# Per: Performance

Rules for people who need high performance or low-latency.

## Table of contents

- [Per: Performance](#per-performance)
  - [Table of contents](#table-of-contents)
    - [Per.11: Access memory predictably](#per11-access-memory-predictably)


### Per.11: Access memory predictably

```cpp
int matrix[rows][cols];
// do
for (int r = 0; r < rows; r++)
  for (int c = 0; c < cols; c++)
    sum += matrix[r][c];
// do not
for (int c = 0; c < cols; ++c)
  for (int r = 0; r < rows; ++r)
    sum += matrix[r][c];
```
