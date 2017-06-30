---
title: (Untitled)
permalink: temp
id: 78
updated: '2017-06-15 02:28:12'
date: 2017-06-15 02:01:05
---

## Initializer
```cpp
#include <iostream>
#include <vector>
using namespace std;
struct s {
    int x;
    std::string std;
    std::vector<int> vec;
};

s func (){
    return {1, "abcde", {2,4,6,8,10}};
}

int main() {

}

```
c++98 编译失败
```bash
$ g++ -std=c++98 1.cpp
1.cpp: In function ‘s func()’:
1.cpp:11:10: warning: extended initializer lists only available with -std=c++11 or -std=gnu++11
   return {1, "abcde", {2,4,6,8,10}};
          ^
1.cpp:11:35: warning: extended initializer lists only available with -std=c++11 or -std=gnu++11
   return {1, "abcde", {2,4,6,8,10}};
                                   ^
1.cpp:11:35: error: could not convert ‘{1, "abcde", {2, 4, 6, 8, 10}}’ from ‘<brace-enclosed initializer list>’ to ‘s’

```
C++98 支持的版本
```cpp
#include <iostream>
#include <vector>
using namespace std;
struct s {
    int x;
    std::string std;
    std::vector<int> vec;
    s(int _x, std::string _std, std::vector<int> v) :
        x(_x), std(_std), vec(v) {}
};

s func (){
    std::vector<int> v;
    int t[] = {2,4,6,8,10};
    for (size_t i = 0; i < 5; i++) {
        v.push_back(t[i]);
    }
    return s(1, "abcde", v);
}

int main() {

}

```
更优雅一点的方式
```cpp
#include <iostream>
#include <vector>
using namespace std;
struct s {
    int x;
    std::string std;
    std::vector<int> vec;
    s(int _x, std::string _std, std::vector<int> v) :
        x(_x), std(_std), vec(v) {}
};

s func (){
    int t[] = {2,4,6,8,10};
    std::vector<int> v(t, t+5);
    return s(1, "abcde", v);
}

int main() {

}

```


## `shared_ptr` & `weak_ptr`

```cpp
#include <iostream>
#include <memory>
using namespace std;
int main() {
    shared_ptr<int> sp1(new int(22));
    weak_ptr<int> wp = sp1;
    cout << wp.use_count() << endl;  // 1

    cout << wp.lock() << endl,        // 0xd92c20
        sp1.reset(),
        cout << wp.lock() << endl;    // 0xd92c20

    cout << wp.lock() << endl;        // 0
}

```

## 字面量
[http://lib.csdn.net/article/cplusplus/30881](http://lib.csdn.net/article/cplusplus/30881)