---
title: Data Structure
date: 2023-05-25 17:34:01
categories: [Programming, Data Structure]
tags: [Data]
comment: true
sticky: 90
---

Data Strutcure

<!-- more -->

## Array

Array has fixed size and contiguous memory. New elements cannot be appended. You can use memory address to access elements of Array. 

```c++
char a[5] = {'h', 'e', 'l', 'l', 'o',}; 
```
<img src="https://cdn.staticaly.com/gh/Dionysen/BlogCDN@master/img/2022-10-0191157.png" style="zoom: 33%;" />
C++ counts food tags from `0`, so `a[0] = 'h'` and `a[1] = 'e'`.
Random access using `a[i]` has `O(1)` time complexity.
Units of array can be modified. 
````c++
a[0] = 'b';
````
result:
```
bello
```
<img src="https://cdn.staticaly.com/gh/Dionysen/BlogCDN@master/img/屏幕截图 2022-10-02 191857.png" style="zoom:33%;" />
### Dynamic Allocation of Arrays

A size-`n` array can be created in this way:

```c++
char a[n];
```

But when writing the code, `n` must be known.

If `n` is unknown, how dose the program run?

```c++
char* a = NULL;
int n; // array size 
cin >> n; // read in the size. e.g., get n = 5
a = new char[n];
```

Now `a` is an **empty** array whose size is `5`.

```c++
// store somrthing in the array
a[0] = 'h';
a[1] = 'e';
a[2] = 'l';
a[3] = 'l';
a[4] = 'o';
```

When done, **free memory**. Otherwise, memory leak can happen.

```c++
delete [] a;
a = NULL;
```



Removing an element in the middle has `O(n)` time complexity. Require moving the remaining items leftward.

## Vector

Vector is almost the same as array.

The main difference is that vector's capacity can automatically grow.

New elements can be appended using `push_back()` in `O(1)` time(on average). 

The last element can be removed using `pop_back()` in `O(1)` time.

```c++
std::vector<char> v = {'h', 'e', 'l', 'l', 'o'}; 
v.push_back();
v.pop_back();
v.erase(v.begin() + 1);
```

Vector can delete an element in the middle using `erase()` in `O(n)` time. So it is not better to do this.

```c++
std::vector<char> v(100);
cout << v.size();		// print "100"
cout << v.capacity();	// print "100"
// then
v.push_back('x');
cout << v.size();		// print "101"
cout << v.capacity();	// print "200"
```

**When size is going to exceed capacity, program will create a new array of capacity 200, copy the 100 elements from the old array to the new, put  the new element in the 101st position and free the old array from memory.**

## List

###  A Node

A node contains a data and two pointers that one points to the previous node and another points to the next node.

### Doubly Linked List

```c++
std::list<char> l = {'h', 'e', 'l', 'l', 'o'}; 
```

```c++
cout << l[2];		// does not work
l[0] = 'a';			// does not work
```

```c++
list<char>::iterator iter = l.begin();
cout << *iter;		// print 'h'
iter++;
cout << *iter;		// print 'e'
*iter = 'a';

push_back();
push.front();
```

## Diference

|        | Array      | Vector                    | List                      |
| ------ | ---------- | ------------------------- | ------------------------- |
| Size   | fixed      | can increase and decrease | can increase and decrease |
| Memory | contiguous | contiguous                | not contiguous            |

|               | Array  | Vector          | List   |
| ------------- | ------ | --------------- | ------ |
| Rand Access   | `O(1)` | `O(1)`          | —      |
| `push_back()` | —      | `O(1)`(average) | `O(1)` |
| `pop_back()`  | —      | `O(1)`          | `O(1)` |
| `insert()`    | —      | `O(n)`(average) | `O(1)` |
| `erase()`     | —      | `O(n)`(average) | `O(1)` |

Which shall we use?

**Array:** Fixed size throughout.

**Vector:** 

- Random access(i.e., read or write the `i`-th element) is fast.
- Insertion and deletion at the end are fas.
- insertion and deletion in the front and midddle are slow.

**List:**

- Sequentially visiting elements is fast; random access is not ***allowed***.
- Frequent insertion and deletion at any position are OK.



# Binary Search

```c++
int arr[] = {3, 5, 12, 16, 17, 26, 32, 51, 53, 64};
```

**Inputs:** (i) an array whose elements are in the accending order and (ii) a key.

**Goal:** Search for the key in the array. If found, return its index; if not found, return -`1`.



Examle 1:

- Search for the elemnt `53`.
- Return `8`.

Example 2:

- Search for the element 9.
- Return `-1`.

Example: `key = 26`.  Use two variables `left` and `right` pointing to the front of the array and the back respectively. 

```c++
int search(int arr[], int left, int right, int key)
{
    while (left <= right) {
        int mid = (left + right) / 2;
        if (key == arr[mid])
            return mid;
        if (key > arr[mid])
            left = mid + 1;
        else
            right = mid - 1;
    }
    return -1;
}
```

How to suport both **search** and **insertion**?

```c++
std::vector<int> v = {3, 5, 12, 16, 17, 26, 32, 51, 53, 64};
```

The ascending order must be kept; otherwisem search would take *O(n)* time.
Inserting an item into the middle has *O(n)* time complexity(on average).
Can we perform binary search in the list?
No, Given `left` and `right`, we cannot get `mid` efficiently.

|               | Search     | Insertion  |
| ------------- | ---------- | ---------- |
| Vector        | *O(log n)* | *O(n)*     |
| List          | *O(n)*     | *O(1)*     |
| **Skip List** | *O(log n)* | *O(log n)* |

# Skip List

Linked list does not support binary search.

> Skip list allows fast search and fast inertion.

**Search:** *O(log n)* time complexcity on average.

**Insertion:** *O(log n)* time complexcity on average.

## Build a skip list（待补充）

Initially, a linked list contains *n* numbers in ascending order.

## Search

## Insertion
