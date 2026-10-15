# Why Is Quicksort $O(n \log n)$?

The intuition behind why quicksort runs in **$O(n \log n)$ time** is:

> Quicksort processes about $n$ elements at each level of recursion, and it has about $\log_2 n$ levels.

Let's build that intuition step by step.

## 1. Think of Quicksort as Repeatedly Dividing an Array

Suppose we want to sort an array of 8 numbers. Imagine that each pivot divides its subarray into two roughly equal parts.

```
Level 0:                  [ 8 elements ]
                               |
Level 1:             [4 elements] [4 elements]
                         |             |
Level 2:           [2] [2]         [2] [2]
                      |               |
Level 3:          [1][1][1][1]    [1][1][1][1]
```

The subarrays become smaller at each level until each subarray contains only one element.

Here's the important observation:

**Although the number of subarrays increases, the total number of elements processed at each level stays roughly the same.**

## 2. Why Does Each Level Cost $O(n)$?

Consider an array of 8 elements.

| Recursion level | Subarray sizes                | Total elements |
| --------------- | ----------------------------- | -------------- |
| 0               | 8                             | 8              |
| 1               | 4 + 4                         | 8              |
| 2               | 2 + 2 + 2 + 2                 | 8              |
| 3               | 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 | 8              |

During partitioning, quicksort scans through each subarray to move elements to the appropriate sides of the pivot.

The total number of elements across all subarrays at a given level is approximately $n$. Therefore, the total work at each level is $O(n)$.

For 8 elements, a simplified estimate of the work is:

$$\
8 + 8 + 8 + 8 = 32\
$$

This represents approximately $n$ units of work per level, multiplied by the number of levels.

*Note: This is an intuitive work estimate, not an exact count of comparisons or swaps.*

## 3. Why Are There About $\log_2 n$ Levels?

With balanced partitions, every recursive call divides its subarray roughly in half.

Starting with $n$ elements, after $k$ levels, the subarray size is approximately

$$\
\frac{n}{2^k}\
$$

The recursion reaches subarrays of size 1 when

$$\
\frac{n}{2^k} = 1\
$$

Solving for $k$:

$$\
n = 2^k\
$$

Taking the base-2 logarithm of both sides gives

$$\
k = \log_2 n\
$$

For example, if $n = 1024$, the number of halvings is approximately 10 because

$$\
2^{10} = 1024\
$$

Therefore, a balanced recursion tree has approximately $\log_2 n$ levels.

## 4. Put the Two Observations Together

We have established two facts:

- **Work per level:** $O(n)$
- **Number of levels:** $O(\log n)$

Multiplying them gives

$$\
O(n) \times O(\log n) = O(n \log n)\
$$

Therefore, quicksort has **$O(n \log n)$ average-case time complexity** when the partitions are reasonably balanced.

Its best-case asymptotic time complexity is also $O(n \log n)$.

## 5. How Does the Work Grow as the Array Gets Larger?

Consider the simplified estimate $n \log_2 n$.

| Array size ($n$) | Approximate levels ($\log_2 n$) | Estimated work ($n\log_2 n$) |
| ---------------- | ------------------------------- | ---------------------------- |
| 8                | 3                               | 24                           |
| 16               | 4                               | 64                           |
| 32               | 5                               | 160                          |
| 1,024            | 10                              | 10,240                       |
| 2,048            | 11                              | 22,528                       |
| 4,096            | 12                              | 49,152                       |

These are simplified estimates, not exact operation counts.

Notice that doubling the array size increases the estimated work by a little more than a factor of 2, rather than a factor of 4 as with a quadratic-time algorithm.

This is one reason why $O(n \log n)$ algorithms scale well.

## 6. An Important Caveat: Quicksort Can Take $O(n^2)$

The $O(n \log n)$ reasoning assumes that the partitions remain reasonably balanced.

What happens if every pivot is the smallest or largest element in its subarray?

Instead of dividing the array in half, each recursive call reduces the problem size by only one element.

The recursion might look like this:

```
Level 0:  n elements
Level 1:  n - 1 elements
Level 2:  n - 2 elements
Level 3:  n - 3 elements
...
Last:     1 element
```

The total work becomes approximately

$$\
n + (n-1) + (n-2) + \cdots + 1\
$$

Using the formula for the sum of the first $n$ positive integers:

$$\
\frac{n(n+1)}{2}\
$$

Ignoring constant factors and lower-order terms, the result is

$$\
O(n^2)\
$$

This is quicksort's **worst-case time complexity**.

### Connection to Our Java Implementation

Our implementation chooses the element at the middle index as the pivot:

```
int middle = (left + right) / 2;
int pivot = array[middle];
```

Choosing the middle index does not guarantee a balanced partition. The value at that index could still be the smallest or largest value in the subarray.

The performance depends on how the pivot value divides the elements, not simply on the pivot's original index.

## 7. A Reusable Way to Analyze Divide-and-Conquer Algorithms

When analyzing quicksort or another divide-and-conquer algorithm, ask two questions:

1. **How much work is performed at each level?**

   For quicksort, partitioning all subarrays at one level takes approximately $O(n)$ total work.
2. **How many levels are there?**

   With balanced partitions, the problem size is approximately halved at each level, giving $O(\log n)$ levels.

Combining these results gives

$$\
\boxed{O(n\log n)}\
$$

### Key Takeaway

**Quicksort is $O(n\log n)$ on average because it does linear work across each level of recursion and has logarithmically many levels when partitions are reasonably balanced.**
