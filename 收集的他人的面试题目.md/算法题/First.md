# 1 
# $\color{red} {手写 memmove} $

# 2 $\color{red} {反转str中单词，但注意 只在 , 间隔开的区域内各自反转：} $


例子：  Hello Wrold , Ni Hao  ---->  Wrold Hello , Hao Ni

**$\color{green} {getline(istream,str1,'char')}$**   将 istream用 char分割开，然后填给 str1    
所以可以  while(getline(cin,str1,',')) 来先把 str 划分，形成一个个 vector\<string> 集合,然后对 集合进行 reverse操作，接着 最后把集合 拼接！！

```
#include <iostream>
#include <vector>
#include <sstream>                //** 流操作 得包含此文件 而不是 istream！！
#include <string>
#include <algorithm>
using namespace std;

int main() {    
    string str1 = "Hello World,Ni hao";
    char splitChar = ',';
    
    stringstream ss(str1);
    string str_temp;
    string ret;
    while (getline(ss, str_temp, splitChar)) {
        stringstream was(str_temp);

        vector<string> words;
        string temp;
        while (was >> temp) {
            words.push_back(temp);
        }

        reverse(words.begin(), words.end());

        for (int i = 0; i < words.size(); i++) {
            ret += words[i];
            if (i != words.size() - 1) {
                ret += ' ';
            } else {
                ret += ',';
            }
        }
    }

    if (!ret.empty() && ret.back() == ',') {
        ret.pop_back();
    }

    cout << ret << endl;

    return 0;
}

```

### 变成流之后，可以使用流文件操作

**$\color{green} { stringstream ss(str1);}$**
**$\color{green} { while (getline(ss, str\_temp, splitChar))}$**

**$\color{green} { stringstream was(str\_temp);}$**
**$\color{green} {  while (was >> temp)}$**




# 3 $\color{red} {反转32位有符号整数，且若反转后，结果大于INT_MAX 或 小于 INT_MIN 则返回0} $

$\color{red} {ret设为int，然后它的越界是在 res更新的前一步进行判断的很秀} $
```
class Solution {
public:
    int reverse(int x) {
        int res = 0;
        while (x != 0) {
            int rem = x % 10;
            x /= 10;
            if (res > INT_MAX / 10 || (res == INT_MAX / 10 && rem > INT_MAX % 10)) {
                return 0;
            }
            if (res < INT_MIN / 10 || (res == INT_MIN / 10 && rem < INT_MIN % 10)) {
                return 0;
            }
            res = res * 10 + rem;
        }
        return res;
    }
};
```


$\color{blue} {ret设为long long，越界可以直接判断，但是 这里的话 long long 每次设计到数据转换，所以会性能上慢很多} $
```
class Solution {
public:
    int reverse(int x) {
        if(x==0)return 0;
        long long ret=0;

        while(x!=0){
            int temp=x%10;
            x/=10;
            if(ret==0&&temp==0)
                continue;
            ret=(ret*10+temp);
            if(ret>INT_MAX||ret<INT_MIN)return 0;
        }
        
        return ret;
    }
};
```