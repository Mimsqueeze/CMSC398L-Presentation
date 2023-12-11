# CMSC398L Final Presentation - Minsi Hu

## Problem: Codeforces 238B - T-primes

### Problem Description:
We know that prime numbers are positive integers that have exactly two distinct positive divisors. Similarly, we'll call a positive integer *t* <u>T-prime</u>, if *t* has exactly three distinct positive divisors.

You are given an array of *n* positive integers. For each of them determine whether it is Т-prime or not.
### Problem Input:
The first line contains a single positive integer, *n* (1 ≤ *n* ≤ 10<sup>5</sup>), showing how many numbers are in the array. The next line contains n space-separated integers *x<sub>i</sub>* (1 ≤ *x<sub>i</sub>*≤ 10<sup>12</sup>).
### Problem Output:
Print *n* lines: the *i*-th line should contain "YES" (without the quotes), if number *x<sub>i</sub>* is Т-prime, and "NO" (without the quotes), if it isn't.
### Examples:
#### Input
```
3
4 5 6
```
#### Output
```
YES
NO
NO
```
## Analysis
I thought this problem was just a pretty cool number theory problem, and it gave me a good opportunity to use the sieve. 

We first notice that in order for a number to be considered t-prime, it must be a perfect square, since in order to have exactly three distinct divisors, those divisors must be 1, the number itself, and then its square root. Therefore, when checking if a number is t-prime, we can immediately rule out numbers that are not perfect squares.

We also realize that in order for the number to be t-prime, the square root of the number must be a prime number, to guarentee there only exists three distinct divisors. 

So, this problem just turns into checking if the number is a perfect square, and if it is, whether its square root is prime. Otherwise, the number is not t-prime.

Therefore, to solve the problem, I initialize an array of size 1000001 to store whether a number from 1 to 1000001 is prime. This is due to the limitation that the largest number we can get is 10<sup>12</sup>, and we only need to check if numbers up to the square root of 10<sup>12</sup>, namely from 1 to 10<sup>6</sup>, are prime.

Afterwards, the solution is simple. Check if the number is a perfect square by comparing its truncated square root value with its raw square root. If they are not equal, you know the number is not a perfect square so you return false. Otherwise, return whether the square root itself is prime.

The code used to solve the problem is shown below, and runs in 92 ms with 1000KB memory.

## Solution
```c++
#include <bits/stdc++.h>
#define fastio ios::sync_with_stdio(false); cin.tie(nullptr);
#define endl "\n"
#define int long long

using namespace std;

#define SIEVEMAX 1000001
bool primes[SIEVEMAX];

void sieve(int n) {
    memset(primes, true, sizeof(primes));
    primes[1]= false;
    for (int p = 2; p * p <= n; p++) {
        if (primes[p]) {
            for (int i = p * p; i <= n; i += p)
                primes[i] = false;
        }
    }
}

bool isTprime(int x) {
    // now find if square root of x is prime
    long double a = sqrt(x);

    // if it is not a perfect square, no chance of being t prime
    if (trunc(a) != a)
        return false;

    x = (int) a; // x is now the sqrt of x
    return primes[x];
}

void solve() {
    int x; cin >> x;
    if (isTprime(x)) cout << "YES";
    else cout << "NO";
    cout << endl;
}

signed main() {
    fastio
    int t = 1;
    cin >> t;
    sieve(SIEVEMAX);
    while (t--) {
        solve();
    }

    return 0;
}
```
