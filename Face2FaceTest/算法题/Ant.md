```
// 扫描时：换行和换列是  +=2的跳跃 （不用怕越界，因为for循环有界限限制；而且已经考虑到了跳到最后一列/行的情况了）
#include <iostream>
#include <unordered_map>
#include <unordered_set>
#include <vector>
using namespace std;
 
// 对于每块拼图而言：在各个方向上的合法的搭档
vector<unordered_set<char>>WLegal = {{'W', '*'}, {'D', 'A', 'W', '*'}, {'A', '*'}, {'D', '*'}};      // {U D L R}
vector<unordered_set<char>>DLegal = {{'W', '*'}, {'S', '*'}, {'S', 'D', 'W', '*'}, {'D', '*'}};
vector<unordered_set<char>>SLegal = {{'D', 'S', 'A', '*'}, {'S', '*'}, {'A', '*'}, {'D', '*'}};
vector<unordered_set<char>>ALegal = {{'W', '*'}, {'S', '*'}, {'A', '*'}, {'W', 'S', 'A', '*'}};
 
 ///  优雅编程：每次的 for循环遍历时，不用疯狂根据 拼图字符去 if else
unordered_map<char, vector<unordered_set<char>>>Frame = {{'W', WLegal}, {'D', DLegal}, {'S', SLegal}, {'A', ALegal}};
 
int main() {
    int n;
    cin >> n;
    int temp_n = n;
    while (n--) {
        int row, col;
        cin >> row >> col;
        vector<vector<char>>Record(row, vector<char>(col));
 
        int index = 0;
        int temp_row = 0;
        int temp_col = col;
        while (row--) {
            while (temp_col--) {
                cin >> Record[temp_row][index++];
            }
            index = 0;
            temp_col = col;
            temp_row++;
        }
 
        bool flag = true;
        for (int i = 0; i < Record.size(); i++) {
            if (flag == false)break;
            for (int j = 1; j < Record[i].size(); j+=2) {
                if (flag == false)break;
                if (Record[i][j] == '*')
                    continue;
                if (j == Record[i].size() - 1) {
                    if (Frame[Record[i][j]][2].find(Record[i][j - 1]) ==
                            Frame[Record[i][j]][2].end())flag = false;
                } else {
                    if (Frame[Record[i][j]][2].find(Record[i][j - 1]) ==
                            Frame[Record[i][j]][2].end())flag = false;
                    if (Frame[Record[i][j]][3].find(Record[i][j + 1]) ==
                            Frame[Record[i][j]][3].end())flag = false;
                }
            }
        }
 
        if (flag == false) {
            cout << "No" << endl;
            continue;
        }
 
        for (int i = 1; i < Record.size(); i+=2) {
            if (flag == false)break;
            for (int j = 0; j < Record[i].size(); j++) {
                if (flag == false)break;
                if (Record[i][j] == '*')
                    continue;
                if (i == Record.size() - 1) {
                    if (Frame[Record[i][j]][0].find(Record[i - 1][j]) == Frame[Record[i][j]][0].end())flag = false;
                } else {
                    if (Frame[Record[i][j]][0].find(Record[i - 1][j]) == Frame[Record[i][j]][0].end())flag = false;
                    if (Frame[Record[i][j]][1].find(Record[i + 1][j]) == Frame[Record[i][j]][1].end())flag = false;
                }
            }
        }
 
        if (flag == false) {
            cout << "No" << endl;
        } else {
            cout << "Yes" << endl;
        }
    }
 
    return 0;
}

```

# // 一个a=a，一个b=aa，一个c=bb   问 组成K个a，所需的最短的字符串的长度   如 5个a：ca=aaaaa
```
string test2() {
	int K;
	cin >> K;
	if (K <= 1) {
		cout << "a" << endl;
		return "a";
	}
	string target = "";
	for (int i = 0; i < K; i++) {
		target += 'a';
	}

	stack<char>s1;
	for (int i = 0; i < target.size(); i++) {
		char temp = target[i];
		while (!s1.empty() && s1.top() == temp && temp < 'z') {
			temp++;
			s1.pop();
		}
		s1.push(temp);
	}

	cout << s1.size() << endl;
	string ret = "";
	while (!s1.empty()) {
		ret = s1.top() + ret;
		s1.pop();
	}

	cout << ret << endl;

	return ret;
}
```
# // 其实可以当成2进制的题目来做：问 10进制K转成2进制数有几个1； （尽量让大的字母表示a，则表示K个a的字符串的长度是最短的） O(N) 复杂度快很多
```
int test3() {
	int K;
	cin >> K;

	int temp = 1; int ret = 0;
	while (temp <= K) {
		temp *= 2;
	}

	temp /= 2;
	while (temp != 0 && K != 0) {
		if (K >= temp) {
			ret++;
			K %= temp;
		}
		temp /= 2;
	}

	cout << ret << endl;
	return ret;
}
```

# n 个树，1-n个连续增长的子序列的个数 
```
/// 本质还是动态规划  不过dp维度较高
/// 7: 1 2 3 6 1 2 3   连续增长的子序列：长度为1的 有 7个： 1；2；3；4；6；1；2；3  长度为2的有 ： 1 2； 2 3； 3 6； 1 2； 2 3
int test1() {
	int n;
	cin >> n;
	if (n == 0 || n == 1) {
		cout << endl;
		return 0;
	}

	vector<int>nums(n);
	int index = 0;
	while (n--) {
		cin >> nums[index++];
	}

	vector<int>now(nums.size() + 1, 0);
	vector<int>pre(nums.size() + 1, 0);
	vector<int>ret(nums.size() + 1, 0);
	pre[1] = 1; ret[1] = 1;

	for (int i = 1; i < nums.size(); i++) {
		int j = 1;
		if (nums[i] > nums[i - 1]) {
			for (;;) {
				if (pre[j] == 0)break;
				now[j] = 1;
				ret[j]++;
				j++;
			}
			now[j] = 1;
			ret[j]++;
			pre = vector<int>(nums.size() + 1, 0);
			pre = now;
			now = vector<int>(nums.size() + 1, 0);
		}
		else {
			pre = vector<int>(nums.size() + 1, 0);
			pre[1] = 1;
			ret[1]++;
		}
	}

	for (int i = 1; i < ret.size(); i++) {
		cout << i << ":" << ret[i] << endl;
	}

	return 0;
}
```