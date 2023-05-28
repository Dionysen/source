---
title: Algorithm
date: 2023-05-25 17:34:01
categories: [Programming, Algorithm]
tags: [Program]
comment: true
sticky: 999
---



> 同一个问题，不同的算法，结果一样而所消耗资源不一样

<!-- more -->

## 时间复杂度

大O表示法：算法的时间复杂度通常用大O符号表述，定义为 **T[n] = O(f(n))**。称函数T(n)以f(n)为界或者称T(n)受限于f(n)。
如果一个问题的规模是n，解这一问题的某一算法所需要的时间为T(n)。T(n)称为这一算法的“时间复杂度”。

* 常数阶 O(1)
  代码中没有影响到其所用时间的变量
  如：
  
  ```c++
  void swapTwoInts(int &a, int &b){
  int temp = a;
    a = b;
    b = temp;
  }
  ```

* 线性阶 O(n)
  代码中有变量可以影响到执行所用的时间，它们的关系是线性的
  如：
  
  ```c++
  int sum ( int n ){
    int ret = 0;
     for ( int i = 0 ; i <= n ; i ++){
        ret += i;
     }
     return ret;
  7}
  ```

* 平方阶 O(n${^2}$)

代码中的变量与执行代码所用时间的关系是平方
如遍历for循环嵌套for循环

* 对数阶 O(logn)
  在二分查找中，通过while循环，成2倍数的缩减搜索范围，也就是说需要经过log2^n次可以跳出循环。

```c++
 int binarySearch( int arr[], int n , int target){
   int l = 0, r = n - 1;
   while ( l <= r) {
     int mid = l + (r - l) / 2;
     if (arr[mid] == target) return mid;
     if (arr[mid] > target ) r = mid - 1;
     else l = mid + 1;
   }
   return -1;
}
```

以下代码也是O(logn)的时间复杂度

```c++
   // 整形转成字符串
   string intToString ( int num ){
    string s = "";
    // n 经过几次“除以10”的操作后，等于0
    while (num ){
     s += '0' + num%10;
     num /= 10;
    }
   reverse(s)
   return s;
  }
```

* 线性对数阶 O(nlogn)
  将时间复杂度为O(logn)的代码循环N遍的话，那么它的时间复杂度就是 n * O(logn)，也就是了O(nlogn)
  
  ```c++
  void hello (){
    for( m = 1 ; m < n ; m++){
      i = 1;
      while( i < n ){
          i = i * 2;
      }
    }
  }
  ```

空间复杂度

## 递归与迭代

递归：函数中调用函数本身
迭代：每次迭代的结果作为下一次迭代的初始值



> `Program` = `Data Structure` + `Algorithm`

## Complexity

How to judge a algorithm?
How to contrast diferent algorithms?
**Big O Notation:** The limiting case of the time or space required by the algorithm as the computational magnitude increases.

> T(n) = O(f(n))

### Time Complexity

- Example 1:

```c++
for (int i = 1; i <= n; i++){
 x++;
}
```

The time complexity of the `for` loop is *O(n)*.

- Example 2:

```c++
for (int i = 1; i <= n; i++){
 x++;
}
for (int i = 1; i <= n; i++){
    for (int j = 1; j <= n; j++){
    x++;
    }
}
```

There are a single `for` loop and a double `for` loop, and the complexity of the former is *O(n)* and the latter is *O(n$^{2}$)* . So when `n` tends to be infinite, the complexity of whole algorithm is *O(n$^{2}$)*.

### Analysis of Commonly used time complexity order

<img src="https://cdn.staticaly.com/gh/Dionysen/BlogCDN@master/img/big-o-running-time-complexity.webp" style="zoom: 80%;" />

#### Constant order *O(1)*

```c++
int x = 0;
int y = 1;
int temp = x;
x = y;
y = temp;
```

As long as there is no complex logic such as loops or recursion, the code is *O(1)* complex no matter how many lines of code are executed.

#### Linear order *O(n)*

```c++
for (int i = 1; i <= n; i++) {
    x++;
}
```

In this code, the for loop is executed n times, so the computation time varies with n, so this kind of code can be expressed by *O(n)*.

#### The logarithmic order *O(log n)*

```c++
int i = 1;
while(i < n) {
    i = i * 2;
}
```

In the above loop, each time `i` is multiplied by `2`, which means each time `i` is a step closer to `n`. So how many cycles does it take for `i` to be equal to or greater than `n`, which is solving for 2${^x}$ is equal to `n`. The answer is **x = log 2${^n}$**. So after log 2${^n}$ cycles, `i` is going to be greater than or equal to `n`, and that's the end of the code. So the complexity of this code is *O(log n)*.

#### Linear log order *O(nlog n)*

```c++
for(int i = 0; i <= n: i++) {
    int x = 1;
    while(x < n) {
        x = x * 2;
    }
}
```

Linear log order *O(nlog n)* is easy to understand, which means you loop through *O(log n)* code `n` times. Each loop is order *O(log n)*,  `n * log n = n(log n)`.

#### Squared square order *O(n${^2}$)*

```c++
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
        x++;
    }
}
```

*O(n${^2}$)* is essentially `n*n`, if we change the number of cycles in the inner layer to `m`:

```c++
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {
        x++;
    }
}
```

The complexity becomes `n * m` = *O(nm)*.

### Space Complexity

Since "time complexity" is not the amount of time a program consumes, "space complexity" is not used to calculate the amount of space a program consumes. As the magnitude of the problem increases, the amount of memory a program needs to allocate may also increase, and "space complexity" reflects the tendency of memory space to increase.

#### *O(1)* Space complexity

```c++
int x = 0;
int y = 0;
x++;
y++;
```

The space allocated by `x` and `y` does not change with the amount of data processed, so the space complexity is *O(1)*.

#### *O(n)* Space complexity

```c++
int[] newArray = new int[n];
for (int i = 0; i < n; i++) {
    newArray[i] = i;
}
```

In this code, we create an array of length `n`and then assign values to the elements in the loop. Therefore, the "space complexity" of this code depends on the length of `newArray`, which is `n`, so *S(n) = O(n)*.

## Sorting Algorithms

Searching is a very important step in computers, but it is often difficult to find a specific number from unordered data. Binary searching, as we mentioned earlier, only works in sorted arrays. So the sorting algorithm is a very important job, and if we can sort the numbers, then we can save a lot of effort when we're looking for a particular number.

There are many sorting algorithms, and each has its own advantages and disadvantages:

| Algorithm      | Time Complexity(Best) | Time Complexity(Average) | Time Complexity(Worst) | Space Complexity |
| -------------- | --------------------- | ------------------------ | ---------------------- | ---------------- |
| Quiksort       | *Ω(n log(n))*         | *Θ(n log(n))*            | *Ω(n${^2}$)*           | *O(log(n))*      |
| Mergesort      | *Ω(n log(n))*         | *Θ(n log(n))*            | *O(n log(n))*          | *O(n)*           |
| Timesort       | *Ω(n)*                | *Θ(n log(n))*            | *O(n log(n))*          | *O(n)*           |
| Heapsort       | *Ω(n log(n))*         | *Θ(n log(n))*            | *O(n log(n))*          | *O(1)*           |
| Bubble Sort    | *Ω(n)*                | *Θ(n${^2}$)*             | *O(n${^2}$)*           | *O(1)*           |
| Insertion Sort | *Ω(n)*                | *Θ(n${^2}$)*             | *O(n${^2}$)*           | *O(1)*           |
| Selection Sort | *Ω(n${^2}$)*          | *Θ(n${^2}$)*             | *O(n${^2}$)*           | *O(1)*           |
| Tree Sort      | *Ω(n log(n))*         | *Θ(n log(n))*            | *O(n${^2}$)*           | *O(n)*           |
| Shell Sort     | *Ω(n log(n))*         | *Θ(n (log(n))${^2}$)*    | *O(n (log(n))${^2}$)*  | *O(1)*           |
| Bucket Sort    | *Ω(n+k)*              | *Θ(n+k)*                 | *O(n${^2}$)*           | *O(n)*           |
| Radix Sort     | *Ω(nk)*               | *Θ(nk)*                  | *O(nk)*                | *O(n+k)*         |
| Counting Sort  | *Ω(n+k)*              | *Θ(n+k)*                 | *O(n+k)*               | *O(k)*           |
| Cubesort       | *Ω(n)*                | *Θ(n log(n))*            | *O(n log(n))*          | *O(n)*           |

### Insertion Sort

Insertion sort is a simple and intuitive sorting algorithm. In insertion sort, we process the unsorted elements from front to back. For each element, we compare it to the previously sorted elements, find the corresponding position, and insert.

Essentially, for each element to be processed, we only care about its relationship to the previous element, and we only deal with the elements after the current element in the next round.

**Specific steps:**

1. Starting with the second element (the first new element to be sorted), the sequence of previous elements is scanned backwards
2. If the current scanned element is larger than the current element, the scanned element is moved to the next bit
3. Repeat Step 2 until you find a position less than or equal to the new element
4. Insert the new element at that location
5. Repeat Steps 1 through 4 for subsequent elements

```c++
void inseritonSort(int arr[], int arrayLength) {
    for (int i = 1; i < arrayLength; i++) {
        int cur = arr[i];
        int inseritonIndex = i - 1;
        while (inseritonIndex >= 0 && arr[inseritonIndex] > cur) {
            arr[inseritonIndex + 1] = arr[inseritonIndex];
            inseritonIndex--;
        }
        arr[inseritonIndex + 1] = cur;
    }
}
```

> *T(n) = O(n${^2}$)*
>
> *S(n) = O(1)*

### Quick Sort

Quicksort is a **Divide and Conquer** algorithm in which we turn big problems into small ones, and solve the small ones one by one, so that when the small ones are solved, the big ones will be solved.

The basic idea of quicksort is to pick a target element and then put the target element in the correct position in the array. The array is then split into two subarrays based on the sorted elements, using the same method and the same operation for the unsorted range.

**Specific steps:**

1. For the current array, we're going to use the last element as our **pivot**
2. All elements smaller than the base number are ranked before the base number, and those larger than the base number are ranked after the base number
3. Once the base number is in place, the element is split into two front and back subarrays based on the cardinality position
4. Use steps 1 through 4 recursively for subarrays until the subarray is 1 or less in length

```c++
void swap(int arr[], int a, int b) {
    int temp = arr[a];
    arr[a] = arr[b];
    arr[b] = temp;
}
int partition(int arr[], int left, int right) {
    int pivot = arr[right];
    int leftIndex = left;
    int rightIndex = right - 1;
    while (1) {
        while (leftIndex < right && arr[leftIndex] <= pivot) {
            leftIndex++;
        }
        while (rightIndex >= left && arr[rightIndex] > pivot) {
            rightIndex--;
        }
        if (leftIndex > rightIndex) break;
        swap(arr, leftIndex, rightIndex);
    }
    swap(arr, leftIndex, right);
    return leftIndex;
}

void quickSort(int arr[], int left, int right) {
    if (left >= right) return;
    int partitionIndex = partition(arr, left, right);
    quickSort(arr, left, partitionIndex - 1);
    quickSort(arr, partitionIndex + 1, right);
}
void printArray(int arr[], int arrayLength) {
    for (int i = 0; i < arrayLength; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << "\n";
}
```

> *T(n) = O(n${^2}$)*.  Average time complexity is *O(n(log n))*.
> *S(n) = O(n)*. Average time complexity is *O(log n).*