- Memomization is a optimization technique used in order to make the code faster.
- It's also used in DP related problems.
- Mainly I used it in recursion problems.

- The main point of memomization is to store previously calculated values and when we need them again we will fetch them instead of calculating every value again.
- Its save computing time and also increase the speed of the algorithm.

### Code Which i wrote for Memomization for the calculation of nth term of a tribonacci

```c++
unordered_map<int, long long> memo;

int trib(int n){

    if(memo.find(n) != memo.end()) return memo[n];
    if(n == 0 || n == 1){
        return n;
    }
    if(n < 0) return 0;
    memo[n] = trib(n - 1) + trib(n - 2) + trib(n - 3);
    return memo[n];
}
```

- In this code we have to find the nth term in tribonacci sequence.
- First we will initialize a unordered_map with int as a key and long long a value.
- Second, we will calculate the tribonacci seq for a n once and then store that vale in the map.
- Third, if we encounter the same value of n which is already stored in the map, then we simply return the value of that key.
- Its will drastically improve the speed of the algorithm, and make it more optimize.
