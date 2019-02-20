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
       # or
       reduce(lambda dic, ch: dic[ch], word, trie)[END] = True
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
9. Math related problems
    ```python
    # GCD, greatest common divisor
    GCD = lambda a, b: (GCD(b, a % b) if a % b else b)
    # LCD, least common denominator
    LCD = lambda a, b: a*b / GCD(a, b)
    ```
10. Integer factorization
    ```python
    def trial_division(n):
        a = []
        while n%2 == 0:
            a.append(2)
            n /= 2
        f = 3
        while n > 1:
            if (n % f == 0):
                a.append(f)
                n /= f
            else:
                #Only odd number is possible
                f += 2
        return a
    ```
11. Geometry
    1. Triangle
    ```python
    # signed area of triangle
    # https://algs4.cs.princeton.edu/99hull/
    # https://algs4.cs.princeton.edu/99hull/Point2D.java.html
    # https://en.wikipedia.org/wiki/Triangle#Using_coordinates
    class Point(object):
        def __init__(self, x, y):
            self.x, self.y = x, y
        def x(self):
            return self.x
        def y(self):
            return self.y
    signedArea2 = lambda a, b, c: (b.x - a.x)*(c.y - a.y) - (b.y - a.y)*(c.x - a.x)
    # signedArea2 >  0: a->b->c is couterclockwise
    # signedArea2 <  0: a->b->c is clockwise
    # signedArea2 == 0: a->b->c is linear
    area = lambda a, b, c: abs(signedArea2(a, b, c))/2
