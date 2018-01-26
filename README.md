# python-notes

## Useful tricks

1. Dictionary comprehensive
    ```python
    d = {i: i**3 for i in range(5)}
    ```
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
   ```python
   [ (i, x) for i, x in reversed(list(enumerate(range(5,10))))]
   ```
5. Count frequency of each element in a lit
   ```python
   counter = collections.Counter('abcdeabcdabcaba')
   # get top k most common elements:
   top3 = counter.most_common(3)
   ```
6. Reduce
   ```python
   # reduce has three parts:
   # 1. a lambda/function that takes two argument, 1) an output which will be the next input; 2) input.
   # 2. a sequence for input and output
   # 3. an optional initial output
   reduce(lambda output, input: output + [(input, ord(input))], 'abc', [])
   ```
6. Multiply all items in a list
    ```python
    nums = [1,2,3,4,5]
    reduce(lambda o, i: o*i, nums)
    ```
6. Construct a Trie
   ```python
   Trie = lambda: collections.defaultdict(Trie)
   trie = Trie()
   END = 'END'

   for word in words:
       # dict.__getitem__(k) creates a k-v pair for defaultdict,
       # while dict.get(k) does not.
       reduce(dict.__getitem__, word, trie)[END] = True
   ```
7. Bit operations
    1. Get 1 at lowest 0, 110111 -> 1000, 10101 -> 10
        ```python
        (x + 1)&(~x)
        ```
    2. Count set bits in an integer
        ```python
        n = 0
        while x!=0:
            x = x&(x-1)
            n += 1
        print n
        ```
8. A timer / counter from itertools
    ```python
    countDown = itertools.count(start=100, step=-1)
    next(countDown)
    ```
