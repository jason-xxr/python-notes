# python-notes

## Useful tricks

1. Is a list sorted
    ```python
    isSorted = lambda l: all(l[i] <= l[i+1] for i in xrange(len(l)-1))
    ```
1. Regex, capture and group
    ```python
    pattern = re.compile('(\d+)\.(\d*)\((\d+)\)')
    string = '0.12(345)'
    groups = pattern.match(string).groups() # ('0', '12', '345')
    ```
1. Dictionary comprehensive
    ```python
    d = {i: i**3 for i in range(5)}
    ```
1. Default dict of a default dict of a list
    ```python
    dd = collections.defaultdict(lambda: collections.defaultdict(list))
    dd[0][1].append(2)
    dd
    ```
1. Unpacking
    ```python
    params = [1, 10, 2]
    range(*params) # [1, 3, 5, 7, 9]
    ```
1. Iterate a matrix by row or column
    ```python
    grid = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
    [row for row in grid]
    [col for col in zip(*grid)]
    ```
1. Transpose a matrix
    ```python
    grid = [[1,2],[3,4]]
    map(list, zip(*grid))
    ```
1. Check for any/all values in a grid
    ```python
    grid = [[1,1],[1,2]]
    any(3 in row for row in grid) # False
    any(2 in row for row in grid) # True
    all(1 in row for row in grid) # True
    all(x==1 for row in g for x in row) # False
    all(x>0 for row in g for x in row) # True
    ```
1. Build a directional graph dict using defaultdict
    ```python
    def addEdge(dd, a, b):
        # dd is a defaultdict
        # must return the dict for reduce
        dd[a].append(b)
        return dd
    
    pairs = [(1,2), (2,3), (1,4)]
    graph = reduce(lambda dd, t: addEdge(dd, t[0], t[1]), pairs, collections.defaultdict(list))
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
    1. Get binary complement
        ```python
        2**x.bit_length() - x - 1 if x else 1
        ```
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
        ```
    2. Parallelogram
        ```python
        # assume P1, P2, P3, P4 are counter-clockwise points of a parallelogram
        # vector21 = p2 - p1
        # vector31 = p3 - p1
        # vector41 = vector12 + vector13 = p4 - p1
        # p4 - p1 = p2 - p1 + p3 - p1
        # p2 + p3 - p1 - p4 = 0
        points = set([(-2, 1), (0, 0), (2, -1), (2, 1), (2, 3), (0, 2)])
        parallelograms = set()
        for triangle in itertools.combinations(points,3):
            for i in range(3):
                p1, p2, p3 = triangle[i-3], triangle[i-2], triangle[i-1]
                p4 = p2[0] + p3[0] - p1[0], p2[1] + p3[1] - p1[1]
                if p4 != p1 and p4 in points:
                    parallelograms.add(tuple(sorted([p1,p2,p3,p4])))
        print parallelograms
        # set([((-2, 1), (0, 0), (0, 2), (2, 1)), ((0, 0), (0, 2), (2, -1), (2, 1)), ((0, 0), (0, 2), (2, 1), (2, 3))])
        ```
    3. Two vectors are perpendicular
        Two vectors are perpendicular if their dot product is 0.
        ```python
        # v21 = p2 - p1
        # v43 = p4 - p3
        # v21 and v43 are perpendicular if dot(v21, v43) == 0
        dot = lambda v1, v2: (v1[0]*v2[0] + v1[1]*v2[1])
        perpendicular = lambda v1, v2: dot(v1, v2) == 0
        length = lambda v: (v[0]**2+v[1]**2)**0.5
        area = lambda v21, v31: length(v21)*length(v31)
        ```
