# python-notes

## Useful tricks

1. Is a list sorted
    ```python
    isSorted = lambda l: all(l[i] <= l[i+1] for i in range(len(l)-1))
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
    list(range(*params)) # [1, 3, 5, 7, 9]
    ```
1. Iterate a matrix by row or column
    ```python
    grid = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
    [row for row in grid]
    [col for col in zip(*grid)]
    list(zip(*grid))
    ```
1. Transpose a matrix
    ```python
    grid = [[1,2],[3,4]]
    # unpack multiple list into list of tuples
    # then map each tuple to a list in the parent list
    list(map(list, zip(*grid)))
    ```
1. Flatten a 2D matrix to 1D list
    ```python
    grid = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
    sum(grid, []) # same as below list
    list(x for row in grid for x in row)
    ```
3. Check for any/all values in a grid
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
    def addd_edge(dd, a, b):
        # dd is a defaultdict(<class 'set'>, {})
        # must return the dict for reduce, optional if use for-loop
        dd[a].add(b)
        return dd
    
    pairs = [(1,2), (2,3), (1,4)]
    graph = functools.reduce(lambda dd, t: addEdge(dd, t[0], t[1]), pairs, collections.defaultdict(set))
    # same as
    graph2 = collections.defaultdict(set)
    for a, b in pairs:
        add_edge(graph2, a, b)
    ```
    
2. Get C-like binary bits for 32-bit int in str
    ```python
    bin(-10 & 0xffffffff) # '0b11111111111111111111111111110110'
    '{0:032b}'.format(-10 & 0xffffffff) # '11111111111111111111111111110110'
    # note the direct format to binary are signed, e.g.
    bin(-10) # '-0b1010'
    '{0:032b}'.format(-10) # '-0000000000000000000000000001010'
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
   l = functools.reduce(lambda output, input: output + [(input, ord(input))], 'abc', [])
   # same as the for-loop
   l = []
   for c in 'abc':
       l += (c, ord(c)),
   ```
6. Multiply all items in a list
    ```python
    nums = [1,2,3,4,5]
    functools.reduce(lambda o, i: o*i, nums)
    # same as
    product = 1
    for i in nums:
        product *= i
    ```
6. Construct a Trie
   ```python
   Trie = lambda: collections.defaultdict(Trie)
   trie = Trie()
   END = 'END'

   words = ["cat", "dog", "catch", "a"]
   for word in words:
       # dict.__getitem__(k) creates a k-v pair for defaultdict,
       # while dict.get(k) does not.
       functools.reduce(dict.__getitem__, word, trie)[END] = True
       # or
       functools.reduce(lambda dic, ch: dic[ch], word, trie)[END] = True
   # same as
   branch = trie
   for word in words:
       for ch in word:
           branch = branch[ch]
       branch[END] = True
       branch = trie
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
    # GCD, greatest common divisor btween two integers
    GCD = lambda s, l: GCD(l % s, s) if l % s else s
    # GCD of a list of numbers, sorted
    gcd = functools.reduce(GCD, [30, 40, 60])
    # LCD, least common denominator
    LCD = lambda a, b: a*b // GCD(a, b)
    lcd = functools.reduce(LCD, [30, 40, 60])
    ```
10. Integer factorization
    ```python
    def trial_division(n):
        a = []
        while n%2 == 0:
            a.append(2)
            n //= 2
        f = 3
        while n > 1:
            if n%f == 0:
                a.append(f)
                n //= f
            else:
                # Ideally go through prime number list
                # At least only odd number is possible
                f += 2
        return a
    ```
11. Geometry
    1. Compare polar angle, `centered` is a list of points in `[(x1, y1), (x2, y2), ...]`. We can compare the polar angle by either of the two:
        ```python
        centered.sort(cmp=lambda a, b: -a[0]*b[1]+b[0]*a[1])
        centered.sort(key=lambda p: math.atan2(p[1], p[0]))
        ```
    3. Triangle
       Assuming we have a free triangle (in blue-solid line) on the plane X-Y, and we have the (x, y) coordinates of the 3 vertices. 
       The area can be computed as below.
       We can see the triangle area is the same as the area of 3 red triangles, which is the half of the corresponding 3 sub rectangular panels.
       The area of the 3 sub rectangular panels is the (big rectangular - lower-left rectangular).
       Thus the blue triangle area is 1/2 of that.

        <img src="https://raw.githubusercontent.com/jason-xxr/python-notes/master/TriangleArea.svg">

        ```python
        # signed area of triangle
        # https://algs4.cs.princeton.edu/99hull/
        # https://algs4.cs.princeton.edu/99hull/Point2D.java.html
        # https://en.wikipedia.org/wiki/Triangle#Using_coordinates
        class Point(object):
            def __init__(self, x, y):
                self.x, self.y = x, y
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
12. Other
    1. Map cmp to dict values
        ```python
        cmp = lambda a, b: (a>b)-(a<b)
        print(['EQ', 'GT', 'LT'][cmp(x, 0)])
        ```
