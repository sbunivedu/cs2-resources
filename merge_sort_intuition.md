# Why Is Merge Sort $n \log n$?

The intuition behind why merge sort runs in $n \log n$ time is:

> Merge sort divides the array into smaller pieces, creating about $\log_2 n$ levels of recursion. At each level, merging all the pieces back together takes a total of $O(n)$ work.

Let's build that intuition step by step.

## 1. Think of Merge Sort as Divide, Then Merge

Suppose we want to sort an array of 8 numbers:

```
Original array:
[38, 12, 27, 43,  9, 31, 18, 25]

             Divide
                |
       [38, 12, 27, 43]    [9, 31, 18, 25]
            /      \           /      \
       [38, 12] [27, 43]  [9, 31] [18, 25]
         /  \     /  \     /  \     /  \
       [38][12] [27][43] [9][31] [18][25]

             Merge
                |
       [12, 38] [27, 43] [9, 31] [18, 25]
            \      /           \      /
       [12, 27, 38, 43]    [9, 18, 25, 31]
                \              /
          [9, 12, 18, 25, 27, 31, 38, 43]
```

The process has two main operations:

1. Divide: Keep splitting the array in half until each subarray contains one element.
2. Merge: Combine the small sorted subarrays into larger sorted subarrays.

## 2. Why Does Each Level Take $O(n)$ Work?

Here's the key observation: at each level, the total number of elements being merged is approximately $n$.

For our example with 8 elements:

| Merge level | Subarray sizes being merged                | Total elements |
| ----------- | ------------------------------------------ | -------------- |
| 1           | Eight single elements into pairs           | 8              |
| 2           | Four pairs into groups of four             | 8              |
| 3           | Two groups of four into one group of eight | 8              |

At each level, every element participates in a merge. Merging two sorted subarrays takes time proportional to their combined size because the algorithm compares elements and copies them into the correct order.

For example, at the last level, merge sort combines two sorted groups of four elements. It needs to process at most 8 elements to produce the final sorted array.

So, even though the merges happen on different-sized subarrays, the total work per level is $O(n)$.

For an array of 8 elements, we can roughly estimate the work as:

$$ 8 + 8 + 8 = 24 $$

This is a simplified estimate, not an exact count of comparisons or copies.

## 3. Why Are There About $\log_2 n$ Levels?

Each division cuts a subarray's size approximately in half.

Starting with $n$ elements, after $k$ levels of splitting, each subarray has roughly

$$\
\frac{n}{2^k}\
$$

elements. We stop splitting when each subarray contains one element:

$$\
\frac{n}{2^k} = 1\
$$

Solving for $k$ gives

$$ n = 2^k $$

Therefore,

$$ k = \log_2 n $$

For example, an array of 1,024 elements takes about 10 rounds of splitting because $2^{10} = 1024$.

| Array size | Approximate splitting levels |
| ---------- | ---------------------------- |
| 8          | 3                            |
| 16         | 4                            |
| 32         | 5                            |
| 1,024      | 10                           |
| 1,048,576  | 20                           |

The number of levels grows slowly: doubling the array size adds only about one more level.

## 4. Put the Two Observations Together

We now have two important facts:

- Work per level: $O(n)$
- Number of levels: $O(\log n)$

Multiply them together:

$$ O(n) \times O(\log n) = O(n \log n) $$

Therefore, merge sort runs in $O(n \log n)$ time.

## 5. Why Is This Better Than $O(n^2)$?

Compare merge sort with a simple quadratic-time sorting algorithm, such as bubble sort.

| Array size ($n$) | $n\log_2 n$ | $n^2$       |
| ---------------- | ----------- | ----------- |
| 100              | 664         | 10,000      |
| 1,000            | 9,966       | 1,000,000   |
| 10,000           | 132,877     | 100,000,000 |

These values are simplified measures of growth, not exact operation counts.

Notice that as the input grows, the quadratic expression grows much faster than $n\log n$. That's why merge sort is effective for large arrays.

## 6. An Important Difference Between Merge Sort and Quicksort

Both algorithms commonly have $O(n\log n)$ performance, but their guarantees differ.

- Merge sort: Its worst-case running time is $O(n\log n)$ because it consistently divides the array into nearly equal halves and performs linear total merging work per level.
- Quicksort: Its average-case running time is $O(n\log n)$, but its worst-case running time can be $O(n^2)$ if its partitions are repeatedly very unbalanced.

Merge sort typically requires $O(n)$ additional memory for arrays, whereas many quicksort implementations sort in place using much less auxiliary array space.

## Key takeaway

When analyzing merge sort, ask two questions:

1. How much work happens at each level? $O(n)$, because merging all subarrays processes a total of about $n$ elements.
2. How many levels are there? $O(\log n)$, because the array is repeatedly divided in half.

Multiply the two:

$$\
\boxed{O(n\log n)}\
$$
