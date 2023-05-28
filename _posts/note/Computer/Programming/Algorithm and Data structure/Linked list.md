---
title: Singly Linked list
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Termux, Linux, WSL2, Windows]
comment: true
sticky: 90
---

Now we implement a singly linked list with C++.

> Linked list is a Data structure that contains a variable and a pointer pointing to the next node.

<!--more-->

### Storage of data

First of all, a data structure is needed to store the variable and the pointer. It can be coded:

```cpp
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
```

Three constructors are packaged in the `struct Node`, which can help us create a **head** of a linked list conveniently.

```cpp
Node *head = new Node();
Node *head = new Node(0);
Node *head = new Node(0, nullptr);
```

There are three ways to create a pointer `head`. Using first way, you can create a pointer `head` pointing to a node that has a variable `val = 0` and a `nullptr` (a pointer pointing nothing). And the second way, you create a same `head` but you must assign a value to `val` of the node pointed to by `head`. The last, you even can link a node to the `head` requiring you give a pointer pointing to the next node.

Then, how to handle the linked list?

### Linked list class

Object-oriented is a

```cpp
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
 void assign(int val);
    int at(int pos);
    void set(int pos, int val);
    void del(int pos);
    int find(int val);
    void print();
};
```

```cpp
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

### Merge Two LinkedList

```cpp
Node *mergeList(ListNode *l1, ListNode *l2) {
    Node *p, *q, *temp, *newList;
    if (l1->head->val >
        l2->head->val) { // make ptr "p" is the head of small list
        newList = l2->head;
        p = l2->head;
        q = l1->head;
    } else {
        newList = l1->head;
        p = l1->head;
        q = l2->head;
    }
    while (p != nullptr && q != nullptr) {
        if (q->val >= p->val) {
            temp = q->next;
            q->next = p->next;
            p->next = q;
            q = temp;
            p = p->next->next;
        } else {
            temp = p->next;
            p->next = q->next;
            q->next = p;
            p = temp;
            q = q->next->next;
        }
    }
    if (q != nullptr) {
        p->next = q;
    }
    return newList;
}
```



