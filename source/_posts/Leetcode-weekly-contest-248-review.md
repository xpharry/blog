---
title: Leetcode Weekly Contest 248 Review
date: 2021-07-04 17:06:19
tags: Leetcode
---

## Summary

I was not doing well in this contest which was under my average performace. That makes me write a review on the contest rather than let it go so easily.

I solved only the first problem which usually a warm-up. I couldn't understand the second problem description so I jumped to the third one. I finished with one solution on the third one but could only get a result of "Time Limit Exceeded". I didn't even touch the last one which was the hardest one.

<!--more-->

## 1. Longest Common Subpath

**Easy**

### Problem description

Given a **zero-based permutation** `nums` (**0-indexed**), build an array `ans` of the **same length** where `ans[i] = nums[nums[i]]` for each `0 <= i < nums.length` and return it.

A **zero-based permutation** `nums` is an array of **distinct** integers from `0` to `nums.length - 1` (**inclusive**).

**Example 1:**

```
Input: nums = [0,2,1,5,3,4]
Output: [0,1,2,4,5,3]
Explanation: The array ans is built as follows:
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[0], nums[2], nums[1], nums[5], nums[3], nums[4]]
    = [0,1,2,4,5,3]
```

Example 2:

```
Input: nums = [5,0,1,2,3,4]
Output: [4,5,0,1,2,3]
Explanation: The array ans is built as follows:
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[5], nums[0], nums[1], nums[2], nums[3], nums[4]]
    = [4,5,0,1,2,3]
```

**Constraints:**

- ``1 <= nums.length <= 1000``
- ``0 <= nums[i] < nums.length``
- The elements in `nums` are **distinct**.

> **Follow-up:** Can you solve it without using an extra space (i.e., O(1) memory)?

### Review

The first problem in a Weekly Contest usually doesn't involve any typical algorithms. It is safe to translate the description into code directly in this problem.

### Solution

```cpp
class Solution {
public:
    vector<int> buildArray(vector<int>& nums) {
        vector<int> ans = nums;

        for(int i = 0; i < nums.size(); i++) {
            ans[i] = nums[nums[i]];
        }

        return ans;
    }
};
```

## 2. Eliminate Maximum Number of Monsters

**Medium**

### Problem description

You are playing a video game where you are defending your city from a group of `n` monsters. You are given a **0-indexed** integer array `dist` of size `n`, where `dist[i]` is the **initial distance** in meters of the `ith` monster from the city.

The monsters walk toward the city at a **constant** speed. The speed of each monster is given to you in an integer array speed of size n, where `speed[i]` is the speed of the `ith` monster in meters per minute.

The monsters start moving at **minute 0**. You have a weapon that you can **choose** to use at the start of every minute, including minute 0. You cannot use the weapon in the middle of a minute. The weapon can eliminate any monster that is still alive. You lose when any monster reaches your city. If a monster reaches the city **exactly** at the start of a minute, it counts as a **loss**, and the game ends before you can use your weapon in that minute.

Return the **maximum** number of monsters that you can eliminate before you lose, or `n` if you can eliminate all the monsters before they reach the city.

**Example 1:**

```
Input: dist = [1,3,4], speed = [1,1,1]
Output: 3
Explanation:
At the start of minute 0, the distances of the monsters are [1,3,4], you eliminate the first monster.
At the start of minute 1, the distances of the monsters are [X,2,3], you don't do anything.
At the start of minute 2, the distances of the monsters are [X,1,2], you eliminate the second monster.
At the start of minute 3, the distances of the monsters are [X,X,1], you eliminate the third monster.
All 3 monsters can be eliminated.
```

**Example 2:**

```
Input: dist = [1,1,2,3], speed = [1,1,1,1]
Output: 1
Explanation:
At the start of minute 0, the distances of the monsters are [1,1,2,3], you eliminate the first monster.
At the start of minute 1, the distances of the monsters are [X,0,1,2], so you lose.
You can only eliminate 1 monster.
```

**Example 3:**

```
Input: dist = [3,2,4], speed = [5,3,2]
Output: 1
Explanation:
At the start of minute 0, the distances of the monsters are [3,2,4], you eliminate the first monster.
At the start of minute 1, the distances of the monsters are [X,0,2], so you lose.
You can only eliminate 1 monster.
```

**Constraints:**

- ``n == dist.length == speed.length``
- ``1 <= n <= 105``
- ``1 <= dist[i], speed[i] <= 105``

### Review

I was not understanding why in the 1st example the player would choose to do noting in the 3rd step. Then I simply skipped the problem for the moment.

It turned out to be much easier than I was thinking. Instead of dealing with the monsters with dist, we could think about the problem in time space. That is to say, with given distances and the (constant) speeds, we can easily convert the `dist` array into the minutes needed to reach the city. The problem requires to eliminate the monster before it reach 0 distance to the city. So we use the distance as `dist[i] - 1` and it is devided by the speed respectively.

Sort the new time distance and use simulation to find the number that can be eliminated.

### Solution

```cpp
class Solution {
public:
    int eliminateMaximum(vector<int>& dist, vector<int>& speed) {
        for(int i = 0; i < dist.size();  i++) {
            // compute the time in minutes to reach the city
            dist[i] = (dist[i] - 1) / speed[i];
        }

        // sort based on the minutes
        sort(dist.begin(), dist.end());

        for(int i = 0; i < dist.size(); i++) {
            if(i > dist[i]) {
                return i;
            }
        }

        return dist.size();
    }
};
```

### Reference

- [Leetcode Discuss - Sort by arrival]( https://leetcode.com/problems/eliminate-maximum-number-of-monsters/discuss/1314550/Sort-by-arrival)

##  3. Count Good Numbers

**Medium**

### Problem description

A digit string is **good** if the digits (**0-indexed**) at **even** indices are **even** and the digits at **odd** indices are **prime** (`2`, `3`, `5`, or `7`).

- For example, `"2582"` is good because the digits (`2` and `8`) at even positions are even and the digits (`5` and `2`) at odd positions are prime. However, `"3245"` is not good because `3` is at an even index but is not even.

Given an integer `n`, return the total number of good digit strings of length `n`. Since the answer may be large, **return it modulo** `1e9 + 7`.

A **digit string** is a string consisting of digits `0` through `9` that may contain leading zeros.

**Example 1:**

```
Input: n = 1
Output: 5
Explanation: The good numbers of length 1 are "0", "2", "4", "6", "8".
```

**Example 2:**

```
Input: n = 4
Output: 400
```

**Example 3:**

```
Input: n = 50
Output: 564908303
```

**Constraints:**

- ``1 <= n <= 1015``

### Review

The problem with an extra requiremnt to deal with the too big number as in the following,

    "Since the answer may be large, return it modulo 1e9 + 7."

always makes the problem trickier.

Another challenging point is to save the time of the power compuation. For example, to find `20^20`, we don't have to do `20` times mulplications which have lots of repeatations. Once we have `20*20=400`, we can do one more mulplication for `20^4` as `400*400`. The similar for other cases.

### Solution (`Time Limit Exceeded`)

```cpp
class Solution {
public:
    int countGoodNumbers(long long n) {
        if(n == 0) return 1;
        if(n == 1) return 5;
        if(n == 2) return 20;

        long long ans = 1, M = 1e9+7;

        for(int i = 0; i < n/2; i++) {
            ans = ans * 20 % M;
        }

        if(n%2 == 1) {
            ans = ans * 5 % M;
        }

        return ans % M;
    }
};
```

### Solution (Accepted)

```cpp
typedef long long ll;

class Solution {
public:
    int countGoodNumbers(long long n) {
        ll num_of_four = n/2;
        ll num_of_five = n - n/2;
        return (power(4, num_of_four) % p) * (power(5, num_of_five) % p) % p;
    }

    // find x^y efficiently
    ll power(ll x, ll y) {
        if(x == 0) return 0;

        ll ans = 1;

        x = x % p;

        while(y > 0) {
            if(y&1) ans = ans * x % p;
            y = y >> 1; // same as y/2
            x = x * x % p;
        }

        return ans;
    }

    int p = 1e9 + 7; // modulo
};
```

> Should we use "#define ll long long" or "typedef long long ll"?
> **Answer:** There is no difference in time. But using `#define ll long long` is not recommended by specifications and `typedef` is better.

### Reference

- [Leetcode Discuss -
[c++] Full Explanation, power, fast power, modular power](https://leetcode.com/problems/count-good-numbers/discuss/1314319/c%2B%2B-Full-Explanation-power-fast-power-modular-power)

## 4. Longest Common Subpath

**Hard**

### Problem description

There is a country of `n` cities numbered from `0` to `n - 1`. In this country, there is a road connecting **every pair** of cities.

There are `m` friends numbered from `0` to `m - 1` who are traveling through the country. Each one of them will take a path consisting of some cities. Each path is represented by an integer array that contains the visited cities in order. The path may contain a city **more than** once, but the same city will not be listed consecutively.

Given an integer `n` and a 2D integer array `paths` where `paths[i]` is an integer array representing the path of the `ith` friend, return the length of the **longest common subpath** that is shared by **every** friend's path, or `0` if there is no common subpath at all.

A **subpath** of a path is a contiguous sequence of cities within that path.

**Example 1:**

```
Input: n = 5, paths = [[0,1,2,3,4],
                       [2,3,4],
                       [4,0,1,2,3]]
Output: 2
Explanation: The longest common subpath is [2,3].
```

**Example 2:**

```
Input: n = 3, paths = [[0],[1],[2]]
Output: 0
Explanation: There is no common subpath shared by the three paths.
```

**Example 3:**

```
Input: n = 5, paths = [[0,1,2,3,4],
                       [4,3,2,1,0]]
Output: 1
Explanation: The possible longest common subpaths are [0], [1], [2], [3], and [4]. All have a length of 1.
```

**Constraints:**

- `1 <= n <= 105`
- `m == paths.length`
- `2 <= m <= 105`
- `sum(paths[i].length) <= 105`
- `0 <= paths[i][j] < n`
- The same city is not listed multiple times consecutively in `paths[i]`.

### Review (Incomplete)

Some reference solutions in Leetcode Discuss solved it with the so-called "Rolling Hash" which I have never heard.

The idea as I observe is to encode the path into a hash value. By rolling the hash values from one path to the next, we can find the overlapped elements.

I know currently the analysis about the problem is remained less concrete and needs more depth consideration. It will done later.

### Solution

```
typedef long long ll;

class Solution {
public:
    int longestCommonSubpath(int n, vector<vector<int>>& paths) {
        int l = 0, r = min_element(paths.begin(), paths.end(), [](const vector<int> &lhs, const vector<int> &rhs) {
            return lhs.size() < rhs.size();
        })->size();

        int ans = 0;

        while(l <= r) {
            int mid = l + (r-l)/2;
            if(solve(paths, mid)) {
                ans = mid;
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }

        return l-1;
    }

    bool solve(vector<vector<int>> &paths, int mid) {
        ll hash = 1; // why '1'?
        ll base = 1000003; // how to pick this number as base?

        for(int i = 0; i < mid-1; i++) {
            hash = (hash * base) % mod;
        }

        unordered_set<ll> prev;

        for(int k = 0; k < paths.size(); k++) {
            auto path = paths[k];

            int n = path.size();

            unordered_set<ll> curr;

            ll val = 0; // why '0'?

            int i;

            for(i = 0; i < mid; i++) {
                val = (val * base + path[i]) % mod;
            }

            for(int j = i; j <= n; j++) {
                if(k == 0 || prev.count(val)) {
                    curr.insert(val);
                }
                if(j != n) {
                    val = ((base * (val - path[j-i] * hash % mod + mod) % mod) + path[j]) % mod;
                }
            }

            swap(prev, curr);
        }

        if(prev.empty()) return false;

        return true;
    }

    ll mod = 1000000000077; // ?
};
```

### Reference

- [Rolling Hash + Collision Check](https://leetcode.com/problems/longest-common-subpath/discuss/1314826/Rolling-Hash-%2B-Collision-Check)
