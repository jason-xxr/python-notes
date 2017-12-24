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

3. Convert 2D matrix to list
   ```python
   grid = [[1,2],[3,4]]
   sum(grid, [])
   ```

4. Reversely iterate list with index
   ```
   [ (i, x) for i, x in reversed(list(enumerate(range(5,10))))]
   ```
5. Count frequency of each element in a lit
   ```
   collections.Counter(['a','b','a','b','b','c'])
   ```
6. Construct a Trie
   ```
   Trie = lambda: collections.defaultdict(Trie)
   trie = Trie()
   END = 'END'

   for word in words:
       # dict.__getitem__(k) creates a k-v pair for defaultdict,
       # while dict.get(k) does not.
       reduce(dict.__getitem__, word, trie)[END] = True
   ```
