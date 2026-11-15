# Time Complexity — Complete Reference Notes

---

## 1. What Is Time Complexity?

Time complexity measures **how the running time of an algorithm grows as input size (n) increases**. It is about the *growth rate*, not the exact number of seconds — machine speed is ignored.

We describe it using **asymptotic notation**:

| Notation | Meaning | Use case |
|---|---|---|
| **O (Big O)** | Upper bound — worst case | Most commonly used |
| **Ω (Omega)** | Lower bound — best case | Rarely asked, good to know |
| **Θ (Theta)** | Tight bound — when best = worst | Precise average behavior |

In interviews/practice, "time complexity" almost always means **Big O (worst case)**.

---

## 2. The Complexity Classes (Fastest → Slowest)

| Order | Notation | Name | Example |
|---|---|---|---|
| 1 | O(1) | Constant | Array index access, hash map get |
| 2 | O(log n) | Logarithmic | Binary search |
| 3 | O(√n) | Square root | Prime checking |
| 4 | O(n) | Linear | Single loop, linear search |
| 5 | O(n log n) | Linearithmic | Merge sort, quick sort, heap sort |
| 6 | O(n²) | Quadratic | Nested loops, bubble/insertion/selection sort |
| 7 | O(n³) | Cubic | Triple nested loops, naive matrix multiplication |
| 8 | O(2ⁿ) | Exponential | Naive recursive Fibonacci, subsets generation |
| 9 | O(n!) | Factorial | Generating all permutations, brute-force TSP |

**Sanity check table (n = 10):**

| Complexity | Approx. value |
|---|---|
| O(1) | 1 |
| O(log n) | ~3 |
| O(√n) | ~3 |
| O(n) | 10 |
| O(n log n) | ~33 |
| O(n²) | 100 |
| O(n³) | 1,000 |
| O(2ⁿ) | 1,024 |
| O(n!) | 3,628,800 |

**Rule of thumb:**
- O(1), O(log n), O(n), O(n log n) → efficient, generally safe
- O(n²), O(n³) → okay for small/medium n, risky for large n
- O(2ⁿ), O(n!) → only usable when n is guaranteed very small (≤ ~20)

---

## 3. How to Calculate Complexity — The Method

1. Find the **basic operation** (the line that does the real work — comparison, assignment, print, etc.). Ignore one-time setup lines.
2. Count **how many times** that line executes, in terms of n.
3. **Drop constants**: O(2n) → O(n)
4. **Keep only the highest-order term** when terms are added: O(n² + n) → O(n²)
5. For recursion, use the **recursion patterns table** (Section 5) or the Master Theorem (Section 6).

---

## 4. Recognizing Patterns by Looking at Loops

### A. Loop counter behavior tells you the base pattern

| What the loop does | Complexity |
|---|---|
| No loop, direct statements | O(1) |
| `i++` / `i--` (changes by 1) | O(n) |
| `i = i * 2` or `i = i / 2` (multiply/divide) | O(log n) |
| `i * i <= n` style condition | O(√n) |

### B. How multiple loops combine

| Structure | Rule | Result |
|---|---|---|
| Loops one after another (sequential) | **Add**, keep the biggest | e.g., O(n) + O(n²) → O(n²) |
| Loops nested inside each other | **Multiply** | e.g., O(n) × O(n) → O(n²) |
| A normal loop with a log-loop nested inside | **Multiply** | O(n) × O(log n) → O(n log n) |

### Worked Java Examples

```java
// O(n) — single loop, i++
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

```java
// O(n²) — nested loops
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        System.out.println(i + "," + j);
    }
}
```

```java
// O(n³) — triple nested loops
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        for (int k = 0; k < n; k++)
            System.out.println(i + "," + j + "," + k);
```

```java
// O(log n) — doubling loop
for (int i = 1; i < n; i = i * 2) {
    System.out.println(i);
}
```

```java
// O(√n) — grows by squaring condition
for (int i = 2; i * i <= n; i++) {
    System.out.println(i);
}
```

```java
// O(n log n) — normal loop with a doubling loop nested inside
for (int i = 0; i < n; i++) {
    for (int j = 1; j < n; j = j * 2) {
        System.out.println(i + "," + j);
    }
}
```

```java
// Triangular loop (still O(n²)) — very common trick question
for (int i = 0; i < n; i++) {
    for (int j = i; j < n; j++) {
        System.out.println(i + "," + j);
    }
}
// Total iterations = n + (n-1) + ... + 1 = n(n+1)/2 → drop constants → O(n²)
```

---

## 5. Recursion — Recognizing Patterns

Ask two questions about the recursive function:
1. **How many times does it call itself?**
2. **How much smaller does the input get each call?**

| Pattern | Calls itself | Input shrinks by | Complexity | Real example |
|---|---|---|---|---|
| A | once | −1 each time | O(n) | Simple countdown/factorial recursion |
| B | once | ÷2 each time | O(log n) | Binary search |
| C | twice | −1 each time | O(2ⁿ) | Naive Fibonacci |
| D | twice | ÷2 each time + O(n) combine work | O(n log n) | Merge sort |
| E | generates all arrangements | — | O(n!) | Permutations, brute-force TSP |

### Worked Java Examples

```java
// Pattern A — O(n)
void countDown(int n) {
    if (n == 0) return;
    System.out.println(n);
    countDown(n - 1);
}
```

```java
// Pattern B — O(log n)
void halveDown(int n) {
    if (n == 0) return;
    System.out.println(n);
    halveDown(n / 2);
}
```

```java
// Pattern C — O(2^n)
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```

```java
// Pattern D — O(n log n)
void mergeSort(int[] arr, int left, int right) {
    if (left >= right) return;
    int mid = (left + right) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);   // O(n) work to combine
}
```

---

## 6. Master Theorem (Quick Reference)

For recurrences of the form: **T(n) = a·T(n/b) + O(n^d)**

Where:
- `a` = number of recursive calls
- `n/b` = size of each subproblem
- `n^d` = work done outside recursion (combining results)

Compare `d` with `log_b(a)`:

| Condition | Result |
|---|---|
| d < log_b(a) | O(n^(log_b a)) |
| d = log_b(a) | O(n^d · log n) |
| d > log_b(a) | O(n^d) |

**Example — Merge Sort:** T(n) = 2T(n/2) + O(n) → a=2, b=2, d=1 → log_b(a) = 1 = d → **O(n log n)** ✔

---

## 7. Common Data Structure & Algorithm Complexities (Cheat Sheet)

### Arrays
| Operation | Complexity |
|---|---|
| Access by index | O(1) |
| Search (unsorted) | O(n) |
| Search (sorted, binary search) | O(log n) |
| Insert/Delete at end | O(1) |
| Insert/Delete at beginning/middle | O(n) |

### Linked List
| Operation | Complexity |
|---|---|
| Access by index | O(n) |
| Insert/Delete at head | O(1) |
| Insert/Delete at tail (no tail pointer) | O(n) |
| Search | O(n) |

### Stack / Queue
| Operation | Complexity |
|---|---|
| Push/Pop/Enqueue/Dequeue | O(1) |

### Hash Map / Hash Set
| Operation | Average | Worst case |
|---|---|---|
| Insert/Delete/Search | O(1) | O(n) (hash collisions) |

### Binary Search Tree (balanced)
| Operation | Average | Worst case (unbalanced) |
|---|---|---|
| Search/Insert/Delete | O(log n) | O(n) |

### Sorting Algorithms
| Algorithm | Best | Average | Worst | Space |
|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |

### Searching
| Algorithm | Complexity |
|---|---|
| Linear Search | O(n) |
| Binary Search | O(log n) |

### Graph Algorithms (V = vertices, E = edges)
| Algorithm | Complexity |
|---|---|
| BFS / DFS | O(V + E) |
| Dijkstra (with min-heap) | O((V + E) log V) |
| Bellman-Ford | O(V · E) |
| Floyd-Warshall | O(V³) |

---

## 8. Space Complexity (Bonus — Often Asked Together)

Space complexity measures **extra memory used** relative to input size, separate from the input itself.

| Situation | Space Complexity |
|---|---|
| Only a few variables, no data structure grows with n | O(1) |
| Creating an array/list of size n | O(n) |
| Recursion — each call adds a stack frame | O(depth of recursion) |
| 2D array of size n×n | O(n²) |

**Example:**
```java
// O(n) space — recursion depth = n
void countDown(int n) {
    if (n == 0) return;
    countDown(n - 1);
}
```

---

## 9. The Complete Method — Step-by-Step Recap

When you see a new piece of code:

1. **Is it recursive or iterative?**
2. **If iterative:** look at every loop — how does the counter change (+1, ×2, √)? Are loops nested (multiply) or sequential (add, keep biggest)?
3. **If recursive:** how many recursive calls per call, and how much does input shrink each time? Use the recursion pattern table or Master Theorem.
4. **Combine** all parts found, drop constants, keep only the dominant term.
5. **Double check with small n** by tracing manually (n=4 or n=10) if unsure — the pattern of growth will reveal itself.

---

## 10. Quick-Glance Summary Card

```
O(1)        < O(log n)   < O(√n)   < O(n)   < O(n log n) 
< O(n²)     < O(n³)      < O(2ⁿ)   < O(n!)

Sequential loops  → ADD, keep biggest
Nested loops      → MULTIPLY
i++/i--           → O(n)
i *= 2 or i /= 2  → O(log n)
i*i <= n          → O(√n)
Recursion calls itself once, -1 each time  → O(n)
Recursion calls itself once, ÷2 each time  → O(log n)
Recursion calls itself twice, -1 each time → O(2ⁿ)
Recursion calls itself twice, ÷2 + O(n) combine → O(n log n)
All permutations                           → O(n!)
```
