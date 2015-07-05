+++
date = "2015-07-05T13:38:24+09:00"
draft = false
title = "AOJ 0004 - 0009 を埋めた"
eyecatch = "aoj.png"

+++

# 0004

[Simultaneous Equation](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0004)


連立方程式の解は1意に定まると書いてあるので、楽ちん。```小数点以下第４位を四捨五入``` に注意する。あと、負のゼロが出る場合があるのでその時は ```0``` に直さなければならない。

## コード

```c++
#include <bits/stdc++.h>
using namespace std;

int main(void) {
  double x1, x2, y1, y2, X, Y;

  while (cin >> x1 >> y1 >> X >> x2 >> y2 >> Y) {
    double det = 1.0 / (x1 * y2 - x2 * y1);
    double ansX = det * (y2 * X - y1 * Y);
    double ansY = det * (x1 * Y - x2 * X);

    if (ansX == 0) ansX = abs(ansX);
    if (ansY == 0) ansY = abs(ansY);

    printf("%.3f %.3f\n", ansX, ansY);
  }

  return 0;
}

```

# 0005
[GCD and LCM](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0005)

GCD(最大公約数) とLCM(最大公約数) を求めるだけ。 lcm の計算順に注意する。```a * b``` を先にやってしまうと、オーバーフローする場合がある(サンプル2)。

## コード

```c++
#include <bits/stdc++.h>
using namespace std;

int main(void) {
  cin.tie(0);
  ios::sync_with_stdio(false);

  int a, b;
  while (cin >> a >> b) {
    int gcd = __gcd(a, b);
    int lcm = a / gcd * b;

    cout << gcd << " " << lcm << endl;
  }

  return 0;
}

```


# 0006
[Reverse Sequence](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0006)
与えられた文字列を逆順にして出力する。

## コード

```c++
#include <bits/stdc++.h>
using namespace std;

int main(void) {
  cin.tie(0);
  ios::sync_with_stdio(false);

  string str;
  cin >> str;
  reverse(begin(str), end(str));
  cout << str << endl;

  return 0;
}
```

# 0007
[Debt Hell](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0007)

利子を計算するだけだけど、1000円未満を切り上げることに注意。ループ毎に切り上げる。

## コード

```c++
#include <bits/stdc++.h>
using namespace std;

#define rep(i, n) for (int i = 0; i < n; i++)

int main(void) {
  cin.tie(0);
  ios::sync_with_stdio(false);

  int n;
  int money = 100000;
  cin >> n;
  rep(i, n) {
    money *= 1.05;
    if (money % 1000 == 0) continue;
    money += 1000 - money % 1000;
  }

  cout << money << endl;

  return 0;
}
```

# 0008
[Sum of 4 Integers](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0008)

4重ループ。

## コード

```c++
#include <bits/stdc++.h>
using namespace std;

#define rep(i, n) for (int i = 0; i < n; i++)

int main(void) {
  cin.tie(0);
  ios::sync_with_stdio(false);

  int n;
  while (cin >> n) {
    int cnt = 0;
    rep(i, 10) rep(j, 10) rep(k, 10) rep(l, 10) if (i + j + k + l == n) cnt++;

    cout << cnt << endl;
  }

  return 0;
}
```


# 0009
[Prime Number](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0009)

## コード
素数判定。よく, ```make_prime()``` を呼び忘れるので気をつけたい。

```c++
#include <bits/stdc++.h>
using namespace std;

#define rep(i, n) for (int i = 0; i < n; i++)
#define N 1000001

int prime[N];

void make_prime() {  // primeが0だったら素数
  fill(prime, prime + N, 0);
  prime[0] = prime[1] = 1;

  for (int i = 2; i * i < N; i++) {
    if (prime[i] == 0) {
      for (int j = 2 * i; j < N; j += i) {
        prime[j] = 1;
      }
    }
  }
}

int main(void) {
  cin.tie(0);
  ios::sync_with_stdio(false);

  int n;
  make_prime();
  while (cin >> n) {
    int cnt = 0;

    rep(i, n + 1) {
      if (prime[i] == 0) cnt++;
    }

    cout << cnt << endl;
  }

  return 0;
}
```

