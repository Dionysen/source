---
title: Leetcode
date: 2023-05-25 17:34:01
categories: [Programming, Algorithm]
tags: [Algorithm]
comment: true
sticky: 90
---

>  This is a Collection of code to solve the Leetcode problem.

<!-- more -->

## 公共前缀

```c++
#include <iostream>
#include <string>
#include <vector>
using namespace std;
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs)
    {
        if (!strs.size()) {
            return "";
        }
        string prefix = strs[0];
        int count = strs.size();
        for (int i = 1; i < count; ++i) {
            prefix = longestCommonPrefix(prefix, strs[i]);
            if (!prefix.size()) {
                break;
            }
        }
        return prefix;
    }
    string longestCommonPrefix(const string& str1, const string& str2)
    {
        int length = min(str1.size(), str2.size());
        int index = 0;
        while (index < length && str1[index] == str2[index]) {
            ++index;
        }
        return str1.substr(0, index);
    }
};
int main()
{
    vector<string> v;
    string flower = "fl";
    string flow = "f";
    string flight = "";
    //    string fly = "flower";
    v.push_back(flower);
    v.push_back(flow);
    v.push_back(flight);
    //    v.push_back(fly);
    Solution solution;
    cout << solution.longestCommonPrefix(v)
         << endl;
    return 0;
}
```

## 合并链表

```c++
#include <iostream>
#include <stack>
#include <string>
#include <unordered_map>
using namespace std;
struct ListNode {
    int val;
    ListNode* next;
    ListNode()
        : val(0)
        , next(nullptr)
    {
    }
    ListNode(int x)
        : val(x)
        , next(nullptr)
    {
    }
    ListNode(int x, ListNode* next)
        : val(x)
        , next(next)
    {
    }
};
void assign(ListNode* list, int val)
{
    ListNode* p = new ListNode(val);
    ListNode* last = list;
    if (last) {
        while (last->next) {
            last = last->next;
        }
        last->next = p;
    } else {
        list = p;
    }
}
void print(ListNode* head)
{
    ListNode* p;
    cout << "[";
    for (p = head; p; p = p->next) {
        cout << p->val;
        if (p->next) {
            cout << ", ";
        }
    }
    cout << "]" << endl;
}
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2)
    {
        if (l1 == nullptr) {
            return l2;
        } else if (l2 == nullptr) {
            return l1;
        } else if (l1->val < l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        } else {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
int main()
{
    ListNode* list1 = new ListNode();
    ListNode* list2 = new ListNode();
    assign(list1, 1);
    assign(list1, 2);
    assign(list1, 4);
    assign(list2, 1);
    assign(list2, 3);
    assign(list2, 5);
    print(list1);
    print(list2);
    Solution solution;
    print(solution.mergeTwoLists(list1, list2));
    return 0;
}
```

## 合并两个正序数组

```c++
class Solution {
public:
    void merge(vector<int> &nums1, int m, vector<int> &nums2, int n) {
        int i = nums1.size() - 1;
        m--;
        n--;
        while (n >= 0) {
            while (m >= 0 && nums1[m] > nums2[n]) {
                swap(nums1[i--], nums1[m--]);
            }
            swap(nums1[i--], nums2[n--]);
        }
    }
};
```

## 回文数

```c++
​```c++
#include <cmath>

#include <iostream>

using namespace std;

class Solution {

public:

    int index = 0;

    int first = 0, last = 0;

    int t = 0;

    bool isPalindrome(int x)

    {

        if (x < 0) {

            return false;

        }



        t = x;

        while (x != 0) {

            x = x / 10;

            index++;

        }



        for (int i = 1; i < index / 2 + 1; i++) {

            first = getNumOfPosition(t, i, index);

            last = getNumOfPosition(t, index + 1 - i, index);

            if (first != last) {

                return false;

            }

        }

        return true;

    }

    int getNumOfPosition(int x, int pos, int index)

    {

        x = x / pow(10, pos - 1);

        x = x % 10;

        return x;

    }

};



int main()

{

    int x = -121;

    Solution s;

    cout << s.isPalindrome(x) << endl;

    return 0;

}
```

```
## 括号匹配

​```c++
​```c++
#include <iostream>
#include <stack>
#include <string>
#include <unordered_map>
using namespace std;
class Solution {
public:
    bool isValid(string s)
    {
        if (s.size() % 2 == 1) {
            return false;
        }
        unordered_map<char, char> pairs = {
            { ')', '(' },
            { ']', '[' },
            { '}', '{' },
        };
        stack<char> stk;
        for (char ch : s) {
            if (pairs.count(ch)) { //判断遍历时所取的字符ch是否是pairs中的key,如果是则进入if，不是就把ch放入栈中
                if (stk.empty() || stk.top() != pairs[ch]) { //当ch为pairs中的key时（也即右括号），判断栈顶元素是否与此时ch（key）对应的value相等，如果不等就说明括号不匹配，返回false,另一种不匹配的情况是ch为右括号，但空栈，也返回false
                    return false;
                } else
                    stk.pop(); // 如果匹配，栈顶元素出栈
            } else
                stk.push(ch);
        }
        return stk.empty(); //遍历完成后，栈应为空，若不为空，说明括号不匹配
    }
};
int main()
{
    string s = "[(){}]()()()()";
    Solution solution;
    cout << solution.isValid(s)
         << endl;
    return 0;
}
```

```
## 链表

链表是一种数据结构，这种数据结构即其包含一个变量和一个指针，指针指向下一个节点。

以下为单向链表的封装：

​```c++
#include <iostream>
#include <stack>
#include <string>
#include <unordered_map>
using namespace std;
class ListNode {
public:
    struct Node {
        int val;
        Node* next;
        Node()
            : val(0)
            , next(nullptr)
        {
        }
        Node(int x)
            : val(x)
            , next(nullptr)
        {
        }
        Node(int x, Node* next)
            : val(x)
            , next(next)
        {
        }
    };
    ListNode()
    {
        this->head = nullptr;
    }
    Node* head;
    void assign(int val)
    {
        Node* p = new Node(val);
        Node* last = this->head;
        if (last) {
            while (last->next) {
                last = last->next;
            }
            last->next = p;
        } else {
            head = p;
        }
    }
    int at(int pos)
    {
        int index = 0;
        Node* p = this->head;
        while (index != pos) {
            p = p->next;
            index++;
        }
        return p->val;
    }
    void set(int pos, int val)
    {
        int index = 0;
        Node* p = this->head;
        while (index != pos) {
            p = p->next;
            index++;
        }
        p->val = val;
    }
    void del(int pos)
    {
        Node* p = this->head;
        if (pos == 0) {
            head = head->next;
            delete p;
        } else {
            int index = 0;
            Node* p = this->head;
            Node* q;
            for (q = nullptr; p; q = p, p = p->next) {
                if (index == pos - 1) {
                    q->next = p->next;
                    delete p;
                    break;
                }
                index++;
            }
        }
    }
    int find(int val)
    {
        Node* p;
        int index = 0;
        for (p = this->head; p; p = p->next) {
            if (p->val == val) {
                return index;
            }
            index++;
        }
        return -1;
    }
    void print()
    {
        Node* p;
        cout << "[";
        for (p = this->head; p; p = p->next) {
            cout << p->val;
            if (p->next) {
                cout << ", ";
            }
        }
        cout << "]" << endl;
    }
};
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2)
    {
        return list1;
    }
};
int main()
{
    ListNode* node = new ListNode;
    node->assign(1);
    node->assign(2);
    node->assign(3);
    node->assign(4);
    node->assign(5);
    node->print();
    cout << node->find(2) << endl;
    node->del(2);
    node->print();
    cout << node->at(2) << endl;
    node->set(2, 9);
    node->print();
    return 0;
}
```

## 罗马数字转整型

```c++
#include <cmath>
#include <iostream>
#include <string>
using namespace std;
class Solution {
public:
    int s_Num = 0;
    int pos = -1;
    int add = 0;
    int romanToInt(string s)
    {
        for (int i = 0; i < s.length(); ++i) {
            switch (s[i]) {
            case 'M':
                s_Num += 1000;
                break;
            case 'D':
                s_Num += 500;
                break;
            case 'C':
                s_Num += 100;
                break;
            case 'L':
                s_Num += 50;
                break;
            case 'X':
                s_Num += 10;
                break;
            case 'V':
                s_Num += 5;
                break;
            case 'I':
                s_Num += 1;
                break;
            default:
                break;
            }
        }
        findS(s, "CM", 200);
        findS(s, "CD", 200);
        findS(s, "XC", 20);
        findS(s, "XL", 20);
        findS(s, "IX", 2);
        findS(s, "IV", 2);
        return s_Num + add;
    }
    void findS(const string s, const string str, int div)
    {
        for (size_t i = 0; (i = s.find(str, i)) != std::string::npos; i++) {
            this->add -= div;
        }
    }
};
int main()
{
    string s = "MCMXCI";
    Solution solution;
    cout << solution.romanToInt(s) << endl;
    return 0;
}
```

## 爬楼梯

```c++
class Solution {
public:
    int climbStairs(int x) {
        int a = 1, b = 2, s = 0;
        if (x <= 2) {
            return x;
        } else {
            for (int ret = 3; ret <= x; ret++) {
                s = a + b;
                a = b;
                b = s;
            }
        }
        return s;
    }
};
```

## 删除链表中重复元素

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head==nullptr) return head;
        ListNode *p;
        ListNode *q;
        for (p = head, q = p->next; p->next != nullptr, q != nullptr; ) {
            if (p->val == q->val) {
                if(q->next != nullptr){
                    q = q->next;
                    p->next = q;
                }else {
                    p->next = nullptr;
                    break;
                }
            }else {
                p = p->next;
                q=q->next;
            }
        }
        return head;
    }
};
```

## 移除数组中相同的元素

```cpp
#include <iostream>
#include <vector>
using namespace std;
class Solution {
public:
    int removeDuplicates(vector<int>& nums)
    {
        int i = nums.front();
        for (auto it = nums.begin() + 1; it != nums.end();) {
            if (i == *it) {
                nums.erase(it);
            } else {
                i = *it;
                it++;
            }
        }
        for (auto it = nums.begin(); it != nums.end(); it++) {
            cout << *it << "  ";
        }
        cout << endl;
        return nums.size();
    }
};
int main()
{
    Solution s;
    vector<int> nums = { 0, 0, 1, 1, 1, 2, 2, 3, 3, 4 };
    cout << s.removeDuplicates(nums) << endl;
    return 0;
}
```