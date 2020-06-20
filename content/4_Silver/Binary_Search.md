---
id: binary-search
title: "Binary Search on the Answer"
author: Darren Yao
prerequisites: 
 - 
     - Silver - Introduction to Sorting  
---

<module-excerpt>

You should already be familiar with the concept of **binary searching** for a number in a sorted array. However, binary search can be extended to binary searching on the answer itself. 

</module-excerpt>

When we binary search on the answer, we start with a search space of size $N$ which we know the answer lies in. Then, each iteration of the binary search cuts the search space in half, so the algorithm tests $O(\log N)$ values. This is efficient and much better than testing each possible value in the search space.

Let's say we have a function `check(x)` that returns true if the answer of $x$ is possible, and false otherwise. Usually, in such problems, we'll want to find the maximum or minimum value of $x$ such that `check(x)` is true. Similarly to how binary search on an array only works on a sorted array, binary search on the answer only works if the answer function is [monotonic](https://en.wikipedia.org/wiki/Monotonic_function), meaning that it is always non-decreasing or always non-increasing. 

In particular, if we want to find the maximum `x` such that `check(x)` is true, then we can binary search if `check(x)` satisfies both of the following conditions:

 - If `check(x)` is `true`, then `check(y)` is true for all $y \leq x$.
 - If `check(x)` is `false`, then `check(y)` is false for all $y \geq x$.

In other words, we want to reduce the search space to something of the following form, using a check function as we described above.

<center>true true true true true false false false false</center>

We want to find the point at which `true` becomes `false`.

Below, we present two algorithms for binary search. The first implementation may be more intuitive, because it's closer to the binary search most students learned, while the second implementation is shorter.

(pseudocode)

If instead we're looking for the minimum `x` that satisfies some condition, then we can binary search if `check(x)` satisfies both of the following conditions:

 - If `check(x)` is true, then `check(y)` is true for all $y \geq x$.
 - If `check(x)` is false, then `check(y)` is false for all $y \leq x$.

The binary search function for this is very similar. Find the maximum value of $x$ such that `check(x)` is false with the algorithm above, and return $x+1$.

## Example: [CF 577 Div. 2 C](https://codeforces.com/contest/1201/problem/C)

Given an array `arr` of $n$ integers, where $n$ is odd, we can perform the following operation on it $k$ times: take any element of the array and increase it by $1$. We want to make the median of the array as large as possible, after $k$ operations.

Constraints: $1 \leq n \leq 2 \cdot 10^5, 1 \leq k \leq 10^9$ and $n$ is odd.

The solution is as follows: we first sort the array in ascending order. Then, we binary search for the maximum possible median. We know that the number of operations required to raise the median to $x$ increases monotonically as $x$ increases, so we can use binary search. For a given median value $x$, the number of operations required to raise the median to $x$ is

$$\sum_{i=(n+1)/2}^{n} \max(0, x - \text{arr[i]})$$

If this value is less than or equal to $k$, then $x$ can be the median, so our check function returns true. Otherwise, $x$ cannot be the median, so our check function returns false.

The solution codes use the second implementation of binary search.

Java:

```java
static int n;
static long k;
static long[] arr;
public static void main(String[] args) {
    
    n = r.nextInt(); k = r.nextLong();
    arr = new long[n];
    for(int i = 0; i < n; i++){
        arr[i] = r.nextLong();
    }
    Arrays.sort(arr);

    pw.println(search());
    pw.close();
}

// binary searches for the correct answer
static long search(){
    long pos = 0; long max = (long)2E9;
    for(long a = max; a >= 1; a /= 2){
        while(check(pos+a)) pos += a;
    }
    return pos;
}

// checks whether the number of given operations is sufficient
// to raise the median of the array to x
static boolean check(long x){
    long operationsNeeded = 0;
    for(int i = (n-1)/2; i < n; i++){
        operationsNeeded += Math.max(0, x-arr[i]);
    }
    if(operationsNeeded <= k){ return true; }
    else{ return false; }
}
```

C++:

```cpp
int n;
long long k;
vector<long long> v;

// checks whether the number of given operations is sufficient
// to raise the median of the array to x
bool check(long long x){
    long long operationsNeeded = 0;
    for(int i = (n-1)/2; i < n; i++){
        operationsNeeded += max(0, x-v[i]);
    }
    if(operationsNeeded <= k) return true; 
    else return false; 
}

// binary searches for the correct answer
long long search(){
    long long pos = 0; long long max = 2E9;
    for(long long a = max; a >= 1; a /= 2){
        while(check(pos+a)) pos += a;
    }
    return pos;
}

int main() {
    cin >> n >> k;
    for(int i = 0; i < n; i++){
        int t;
        cin >> t;
        v.push_back(t);
    }
    sort(v.begin(), v.end());

    cout << search() << '\n';
}
```

### Problems

 - USACO Silver
   - [Moo Buzz](http://www.usaco.org/index.php?page=viewproblem2&cpid=966)
   - [Cow Dance Show](http://www.usaco.org/index.php?page=viewproblem2&cpid=690)
     - binary search on $K$ and simulate
   - [Convention](http://www.usaco.org/index.php?page=viewproblem2&cpid=858)
     - determine whether $M$ buses suffice if every cow waits at most $T$ minutes
     - use a greedy strategy (applies to next two problems as well)
   - [Angry Cows](http://usaco.org/index.php?page=viewproblem2&cpid=594)
     - check in $O(N)$ how many haybales you can destroy with fixed radius $R$
   - [Social Distancing](http://www.usaco.org/index.php?page=viewproblem2&cpid=1038)
     - check in $O(N+M)$ how many cows you can place with distance $D$
   - [Loan Repayment](http://www.usaco.org/index.php?page=viewproblem2&cpid=991)
     - requires some rather tricky analysis to speed up naive $O(N\log N)$ solution
 - CF
   - [The Meeting Place Cannot Be Changed](http://codeforces.com/contest/782/problem/B) [](48)
   - [Preparing for Merge Sort](http://codeforces.com/contest/847/problem/B) [](53)
   - [Level Generation](http://codeforces.com/problemset/problem/818/F) [](54)
   - [Packmen](http://codeforces.com/contest/847/problem/E) [](57)
   - [Office Keys](http://codeforces.com/problemset/problem/830/A) [](60)