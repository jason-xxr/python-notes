# python-notes

## Useful tricks

1. Transpose a matrix
    ```python
    grid = [[1,2],[3,4]]
    map(list, zip(*grid))
    ```

2. Get C-like binary bits for 32-bit int in str
    ```python
    x = -10
    '{0:032b}'.format(x & 0xffffffff)
    ```
