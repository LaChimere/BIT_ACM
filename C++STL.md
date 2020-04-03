# C语言标准库函数回顾

## 1. \<[cstring](http://www.cplusplus.com/reference/cstring/)\>

### **`strlen()`**


> `size_t strlen ( const char * str );` 
>
> **Returns the length of the C string *str*.**

### **`strcmp()`**

> `int strcmp ( const char * str1, const char * str2 );`
>
> **Compares the C string *str1* to the C string *str2*.**
>
> This function starts comparing the first character of each string. If they are equal to each other, it continues with the following pairs until the characters differ or until a terminating null-character is reached.
>
> `strcmp()` < 0  => *str1* < *str2*
>
> `strcmp()` = 0  => *str1* == *str2*
>
> `strcmp()` > 0  => *str1* > *str2*

### **`strcpy()`**

> `char * strcpy ( char * destination, const char * source );`
>
> **Copies the C string pointed by *source* into the array pointed by *destination*, including the terminating null character (and stopping at that point).**
>
> To avoid overflows, the size of the array pointed by *destination* shall be long enough to contain the same C string as *source* (including the terminating null character), and should not overlap in memory with *source*.

### **`memset()`**

> `void * memset ( void * ptr, int value, size_t num );`
>
> **Sets the first *num* bytes of the block of memory pointed by *ptr* to the specified *value* (interpreted as an `unsigned char`).**
>
> *ptr* is returned.
>
> **Parameters:**
>
> * `ptr`  Pointer to the block of memory to fill.
> * `value`  Value to be set. The value is passed as an `int`, but the function fills the block of memory using the *unsigned char* conversion of this *value*.
> * `num`  Number of bytes to be set to the *value*. `size_t` is an unsigned integral type.

`memset()`在对数组进行初始赋值时比较常用, 如对`arr`数组赋初值:

```c++
int arr[MAXSIZE];
// 常用的初始赋值
memset(arr, 0, sizeof(arr));	// arr中每个元素被设为 0x00000000
memset(arr, 0x3f, sizeof(arr));	// arr中每个元素被设为 0x3f3f3f3f
memset(arr, -1, sizeof(arr));	// arr中每个元素被设为 0xffffffff
```

###  **`memcpy()`**

> `void * memcpy ( void * destination, const void * source, size_t num );`
>
> **Copies the values of *num* bytes from the location pointed to by *source* directly to the memory block pointed to by *destination*.**
>
> *destination* is returned.
>
> The underlying type of the objects pointed to by both the *source* and *destination* pointers are irrelevant for this function; The result is a binary copy of the data.
>
> The function does not check for any terminating null character in *source* - it always copies exactly *num* bytes.
>
> To avoid overflows, the size of the arrays pointed to by both the *destination* and *source* parameters, shall be at least *num* bytes, and should not overlap (for overlapping memory blocks, [memmove](http://www.cplusplus.com/memmove) is a safer approach).
>
> **Parameters:**
>
> * `destination`  Pointer to the destination array where the content is to be copied, type-casted to a pointer of type `void*`.
> * `source`  Pointer to the source of data to be copied, type-casted to a pointer of type `const void*`.
> * `num`  Number of bytes to copy.

### **`memcmp()`**

> `int memcmp ( const void * ptr1, const void * ptr2, size_t num );`
>
> **Compares the first *num* bytes of the block of memory pointed by *ptr1* to the first *num* bytes pointed by *ptr2*, returning zero if they all match or a value different from zero representing which is greater if they do not.**
>
> Notice that, unlike `strcmp`, the function does not stop comparing after finding a null character.

## 2. \<[cmath](http://www.cplusplus.com/reference/cmath/)\> 

## 3. \<[cstdlib](http://www.cplusplus.com/reference/cstdlib/)\>

### **`qsort()`**

> `void qsort (void* base, size_t num, size_t size, int (*compar)(const void*,const void*));`
>
> **Sorts the *num* elements of the array pointed to by *base*, each element *size* bytes long, using the *compar* function to determine the order.**
>
> The sorting algorithm used by this function compares pairs of elements by calling the specified *compar* function with pointers to them as argument.
>
> The function does not return any value, but modifies the content of the array pointed to by *base* reordering its elements as defined by *compar*.
>
> The order of equivalent elements is undefined.
>
> **Parameters:**
>
> * `base`  Pointer to the first object of the array to be sorted, converted to a `void*`.
>
> * `num`  Number of elements in the array pointed to by base.
>
> * `size`  Size in bytes of each element in the array.
>
> * `compar`
>
>     Pointer to a function that compares two elements.
>     This function is called repeatedly by `qsort` to compare two elements. It shall follow the following prototype: `int compar (const void* p1, const void* p2);`
>
>     Taking two pointers as arguments (both converted to `const void*`). The function defines the order of the elements by returning (in a stable and transitive manner):
>
>     | return value |                           meaning                            |
>     | :----------: | :----------------------------------------------------------: |
>     |     `<0`     | The element pointed to by p1 goes before the element pointed to by p2 |
>     |     `0`      | The element pointed to by p1 is equivalent to the element pointed to by p2 |
>     |     `>0`     | The element pointed to by p1 goes after the element pointed to by p2 |

### **`rand()`**

> `int rand (void);`
>
> **Returns a pseudo-random integral number in the range between `0` and `RAND_MAX`(This value is library-dependent, but is guaranteed to be at least `32767` on any standard library implementation.).**
>
> This number is generated by an algorithm that returns a sequence of apparently non-related numbers each time it is called. This algorithm uses a seed to generate the series, which should be initialized to some distinctive value using function `srand`.

生成随机数种子, 如:

```c++
#include <cstdio>
#include <cstdlib>
#include <ctime>

int main ()
{
    printf ("First number: %d\n", rand() % 100);
    srand (time(NULL));
    printf ("Random number: %d\n", rand() % 100);
    srand (1);
    printf ("Again the first number: %d\n", rand() % 100);

    return 0;
}
```

### **[`molloc()`](http://www.cplusplus.com/reference/cstdlib/malloc/?kw=malloc)**

### **[`free()`](http://www.cplusplus.com/reference/cstdlib/free/)**

## 4. \<[ctime](http://www.cplusplus.com/reference/ctime/)\>

### **`time()`**

> `time_t time (time_t* timer);`
>
> **Get the current calendar time as a value of type [`time_t`](http://www.cplusplus.com/time_t).**
>
> The function returns this value, and if the argument is not a *null pointer*, it also sets this value to the object pointed by *timer*.
>
> **Parameter:**
>
> * `timer`  Pointer to an object of type [`time_t`](http://www.cplusplus.com/time_t), where the time value is stored. Alternatively, this parameter can be a *null pointer*, in which case the parameter is not used (the function still returns a value of type [`time_t`](http://www.cplusplus.com/time_t) with the result).

### **`clock()`**

> `clock_t clock (void);`
>
> **Returns the processor time consumed by the program.**
>
> The value returned is expressed in *clock ticks*, which are units of time of a constant but system-specific length (with a relation of [`CLOCKS_PER_SEC`](http://www.cplusplus.com/CLOCKS_PER_SEC) *clock ticks* per second).

测试一段代码的执行时间, 如:

```c++
#include <iostream>
#include <ctime>
using namespace std;

int main()
{
    clock_t start = clock();
    for (...)
    {
        ...
    }
    cout << clock() - start << endl; 					// 以 ms 表示
    cout << (clock() - start) / CLOCK_PER_SEC << endl; 	  // 以 s 表示
}
```

## 5. \<[cctype](http://www.cplusplus.com/reference/cctype/)\>

# C++ STL 概述 (C++ 11)

## 1. Containers

### 1.1 \<[vector](http://www.cplusplus.com/reference/vector/)\>

#### 1.1.1 模板

`template < class T, class Alloc = allocator<T> > class vector; // generic template`

#### 1.1.2 构造函数

* *default*

    `explicit vector (const allocator_type& alloc = allocator_type());`

* *fill*  

    `explicit vector (size_type n);`

    `vector (size_type n, const value_type& val, const allocator_type& alloc = allocator_type());`

* *range*

    `template <class InputIterator> vector (InputIterator first, InputIterator last, const allocator_type& alloc = allocator_type());`

* *copy*

    `vector (const vector& x);`

    `vector (const vector& x, const allocator_type& alloc);`

* *move*

    `vector (vector&& x);`

    `vector (vector&& x, const allocator_type& alloc);`

* *initializer list*

    `vector (initializer_list<value_type> il, const allocator_type& alloc = allocator_type());`

#### 1.1.3 [成员函数](http://www.cplusplus.com/reference/vector/vector/)  ( 部分 )

##### **[`assign()`](http://www.cplusplus.com/reference/vector/vector/assign/)**

>* *range*
>
>    `template <class InputIterator> void assign (InputIterator first, InputIterator last);`
>
>* *fill*
>
>    `void assign (size_type n, const value_type& val);`
>
>* *initializer list*
>
>**Assigns new contents to the vector, replacing its current contents, and modifying its `size` accordingly.**

如:

```c++
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    vector<int> nums;
    cout << nums.size() << endl;	// 输出 0
    nums.assign(10, 8);			   // nums 为 {8, 8, 8, 8, 8, 8, 8, 8, 8, 8}
    nums.assign({1, 2, 3, 4, 5});	// nums 为 {1, 2, 3, 4, 5}
    vector<int> nums2;
    nums2.assign(nums.begin(), nums.begin() + 3);
    for (auto num : nums2)
        cout << num << " ";			// 输出 1 2 3
    
    return 0;
}
```



##### **[`emplace()`](http://www.cplusplus.com/reference/vector/vector/emplace/)** (C++ 11)

> `template <class... Args> iterator emplace (const_iterator position, Args&&... args);`
>
> **The container is extended by inserting a new element at *position*. This new element is constructed in place using *args* as the arguments for its construction.**
>
> This effectively increases the container `size` by one.

在迭代器 *position* 前插入一个元素 *args*, 其中 *args* 为右值引用, 返回指向新插入元素的迭代器.

```c++
#include <iostream>
#include <vector>

int main()
{
    vector<int> nums({1, 2, 3, 4, 5, 6, 7});
    auto it = nums.emplace(nums.begin() + 3, 100);
    for (auto num : nums)
        cout << num << " ";			// 输出 1 2 3 100 4 5 6 7
    cout << endl << *it << endl;	// 输出 100
    
    return 0;
}
```

##### **[`emplace_back()`](http://www.cplusplus.com/reference/vector/vector/emplace_back/)** (C++ 11)

> `template <class... Args> void emplace_back (Args&&... args);`
>
> **Inserts a new element at the end of the vector, right after its current last element. This new element is constructed in place using *args* as the arguments for its constructor.**

##### **[`erase()`](http://www.cplusplus.com/reference/vector/vector/erase/)**

> `iterator erase (const_iterator position);`
>
> `iterator erase (const_iterator first, const_iterator last);`
>
> **Removes from the vector either a single element (*position*) or a range of elements ( [`first, last`) ).**
>
> This effectively reduces the container `size` by the number of elements removed, which are destroyed.

##### **[`insert()`](http://www.cplusplus.com/reference/vector/vector/insert/)**

> * *single element*
>
>     `iterator insert (const_iterator position, const value_type& val);`
>
> * *fill*
>
>     `iterator insert (const_iterator position, size_type n, const value_type& val);`
>
> * *range*
>
>     `template <class InputIterator> iterator insert (const_iterator position, InputIterator first, InputIterator last);`
>
> * *move*
>
>     `iterator insert (const_iterator position, value_type&& val);`
>
> * *initializer list*
>
>     `iterator insert (const_iterator position, initializer_list<value_type> il);`
>
> **The vector is extended by inserting new elements before the element at the specified *position*, effectively increasing the container `size` by the number of elements inserted.**

##### **[`at()`](http://www.cplusplus.com/reference/vector/vector/at/)** 与 **[`operator[]()`](http://www.cplusplus.com/reference/vector/vector/operator[]/)**

发生访问越界时, `at()`会报越界错误, 而使用`[]`不会报错, 可能访问到错误的内容. (详见源码)

##### **[`vector::swap()`](http://www.cplusplus.com/reference/vector/vector/swap/)**

> `void swap (vector& x);`
>
> **Exchanges the content of the container by the content of *x*, which is another vector object of the same type. Sizes may differ.**
>
> After the call to this member function, the elements in this container are those which were in *x* before the call, and the elements of *x* are those which were in this. All iterators, references and pointers remain valid for the swapped objects.
>
> Notice that a non-member function exists with the same name, [swap](http://www.cplusplus.com/vector:swap), overloading that algorithm with an optimization that behaves like this member function.

##### **[`resize()`](http://www.cplusplus.com/reference/vector/vector/resize/)**

> `void resize (size_type n);`
>
> `void resize (size_type n, const value_type& val);`
>
> **Resizes the container so that it contains *n* elements.**
>
> If *n* is smaller than the current container `size`, the content is reduced to its first *n* elements, removing those beyond (and destroying them).
>
> If *n* is greater than the current container `size`, the content is expanded by inserting at the end as many elements as needed to reach a size of *n*. If *val* is specified, the new elements are initialized as copies of *val*, otherwise, they are value-initialized.
>
> If *n* is also greater than the current container [capacity](http://www.cplusplus.com/vector::capacity), an automatic reallocation of the allocated storage space takes place.
>
> Notice that this function changes the actual content of the container by inserting or erasing elements from it.

#####  **[`shrink_to_fit()`](http://www.cplusplus.com/reference/vector/vector/shrink_to_fit/)**

> `void shrink_to_fit();`
>
> **Requests the container to reduce its [capacity](http://www.cplusplus.com/vector::capacity) to fit its `size`.**

##### **[`clear()`](http://www.cplusplus.com/reference/vector/vector/clear/)**

> `void clear() noexcept;`
>
> **Removes all elements from the vector (which are destroyed), leaving the container with a `size` of `0`.**

### 1.2 \<[deque](http://www.cplusplus.com/reference/deque/deque/)\>

#### 1.2.1 模板

`template < class T, class Alloc = allocator<T> > class deque;`

#### 1.2.2 构造函数

* *default*

    `explicit deque (const allocator_type& alloc = allocator_type());`

* *fill*

    `explicit deque (size_type n);`

    `deque (size_type n, const value_type& val, const allocator_type& alloc = allocator_type());`

* *range*

    `template <class InputIterator> deque (InputIterator first, InputIterator last, const allocator_type& alloc = allocator_type());`

* *copy*

    `deque (const deque& x);`

    `deque (const deque& x, const allocator_type& alloc);`

* *move*

    `deque (deque&& x);`

    `deque (deque&& x, const allocator_type& alloc);`

* *initialize list*

    `deque (initializer_list<value_type> il, const allocator_type& alloc = allocator_type());`

#### 1.2.3 [成员函数](http://www.cplusplus.com/reference/deque/deque/) (部分)

##### **[`back()`](http://www.cplusplus.com/reference/deque/deque/back/)**

> `reference back();`
>
> `const_reference back() const;`
>
> **Returns a reference to the last element in the container.**
>
> Calling this function on an [empty](http://www.cplusplus.com/deque::empty) container causes undefined behavior.

##### **[`front()`](http://www.cplusplus.com/reference/deque/deque/front/)**

> `reference front();`
>
> `const_reference front() const;`
>
> **Returns a reference to the first element in the deque container.**
>
> Calling this function on an [empty](http://www.cplusplus.com/deque::empty) container causes undefined behavior.

##### **[`emplace()`](cplusplus.com/reference/deque/deque/emplace/)**

> `template <class... Args> iterator emplace (const_iterator position, Args&&... args);`
>
> **The container is extended by inserting a new element at *position*. This new element is constructed in place using *args* as the arguments for its construction.**
>
> This effectively increases the container `size` by one.

##### **[`emplace_back()`](http://www.cplusplus.com/reference/deque/deque/emplace_back/)** 与 **[`emplace_front()`](http://www.cplusplus.com/reference/deque/deque/emplace_front/)** (详见链接)

##### **[`erase()`](http://www.cplusplus.com/reference/deque/deque/erase/)**

> `iterator erase (const_iterator position);`
>
> `iterator erase (const_iterator first, const_iterator last);`
>
> **Removes from the deque] container either a single element (*position*) or a range of elements (`[first,last)`).**

##### **[`insert()`](http://www.cplusplus.com/reference/deque/deque/insert/)** 详见链接

##### **[`pop_back()`](http://www.cplusplus.com/reference/deque/deque/pop_back/)**

> `void pop_back();`
>
> **Removes the last element in the deque container, effectively reducing the container `size` by one.**
>
> This destroys the removed element.

##### **[`pop_front()`](http://www.cplusplus.com/reference/deque/deque/pop_front/)**

> `void pop_front();`
>
> **Removes the first element in the deque container, effectively reducing its `size` by one.**
>
> This destroys the removed element.

##### **[`push_back()`](http://www.cplusplus.com/reference/deque/deque/push_back/)** 与 **[`push_front()`](http://www.cplusplus.com/reference/deque/deque/push_front/)** 详见链接

##### **[`deque::swap()`](http://www.cplusplus.com/reference/deque/deque/swap/)**

> `void swap (deque& x);`
>
> **Exchanges the content of the container by the content of *x*, which is another deque object containing elements of the same type. Sizes may differ.**
>
> After the call to this member function, the elements in this container are those which were in *x* before the call, and the elements of *x* are those which were in this. All iterators, references and pointers remain valid for the swapped objects.
>
> Notice that a non-member function exists with the same name, [swap](http://www.cplusplus.com/deque:swap), overloading that algorithm with an optimization that behaves like this member function.

### 1.3 \<[stack](http://www.cplusplus.com/reference/stack/stack/)\>

#### 1.3.1 模板

`template <class T, class Container = deque<T> > class stack;`

#### 1.3.2 构造函数

* *initialize*

    `explicit stack (const container_type& ctnr);`

* *move-initialize*

    `explicit stack (container_type&& ctnr = container_type());`

* *allocator*

    `template <class Alloc> explicit stack (const Alloc& alloc);`

* *init + allocator*

    `template <class Alloc> stack (const container_type& ctnr, const Alloc& alloc);`

* *move-init + allocator*

    `template <class Alloc> stack (container_type&& ctnr, const Alloc& alloc);`

* *copy + allocator*

    `template <class Alloc> stack (const stack& x, const Alloc& alloc);`

* *move + allocator*

    `template <class Alloc> stack (stack&& x, const Alloc& alloc);`

当使用 `vector` 构造 `stack` 时, 需要写出来, 如:

```c++
#include <iostream>
#include <vector>
#include <deque>
#include <stack>
using namespace std;

int main()
{
    deque<int> dq({1, 2, 3, 4, 5});
    vector<int> v({1, 2, 3, 4, 5});
    
    stack<int> st1(dq);
    stack<int, vector<int>> st2(v);
    
    while (!st1.empty())
    {
        cout << st1.top() << " ";	// 输出 5 4 3 2 1
        st1.pop();
    }
    cout << endl;
    while (!st2.empty())
    {
        cout << st2.top() << " ";	// 输出 5 4 3 2 1
        st2.pop();
    }
    
    return 0;
}
```

#### 1.3.3 [成员函数](http://www.cplusplus.com/reference/stack/stack/)

##### **[`emplace()`](http://www.cplusplus.com/reference/stack/stack/emplace/)**

> `template <class... Args> void emplace (Args&&... args);`
>
> **Adds a new element at the top of the stack, above its current *top element*. This new element is constructed in place passing *args* as the arguments for its constructor.**

##### **[`empty()`](http://www.cplusplus.com/reference/stack/stack/empty/)**

> `bool empty() const;`
>
> **Returns whether the `stack` is empty: i.e. whether its `size` is *zero*.**

##### **[`pop()`](http://www.cplusplus.com/reference/stack/stack/pop/)**

> `void pop();`
>
> **Removes the element on top of the stack, effectively reducing its `size` by one.**
>
> The element removed is the latest element inserted into the stack, whose value can be retrieved by calling member [`stack::top`](http://www.cplusplus.com/stack::top).
>
> This calls the removed element's destructor.
>
> This member function effectively calls the member function [`pop_back`](http://www.cplusplus.com/deque::pop_back) of the *underlying container* object.

##### **[`push()`](http://www.cplusplus.com/reference/stack/stack/push/)**

> `void push (const value_type& val);`
>
> `void push (value_type&& val);`
>
> **Inserts a new element at the top of the stack, above its current *top element*. The content of this new element is initialized to a copy of *val*.**
>
> This member function effectively calls the member function [`push_back`](http://www.cplusplus.com/deque::push_back) of the *underlying container* object.

##### **[`stack::swap()`](http://www.cplusplus.com/reference/stack/stack/swap/)**

> `void swap (stack& x) noexcept(/*see below*/);`
>
> **Exchanges the contents of the container adaptor (`*this`) by those of *x*.**
>
> This member function calls the non-member function [swap](http://www.cplusplus.com/swap) (unqualified) to swap the *underlying containers*.
>
> The `noexcept` specifier matches the [swap](http://www.cplusplus.com/deque:swap) operation on the *underlying container*.

##### **[`size()`](http://www.cplusplus.com/reference/stack/stack/size/)** (详见链接)

##### **[`top()`](http://www.cplusplus.com/reference/stack/stack/top/)**

> `reference& top();`
>
> `const_reference& top() const;`
>
> **Returns a reference to the *top element* in the stack.**
>
> Since stacks are last-in first-out containers, the top element is the last element inserted into the stack.
>
> This member function effectively calls member [`back`](http://www.cplusplus.com/deque::back) of the *underlying container* object.

### 1.4 \<[queue](http://www.cplusplus.com/reference/queue/)\>

#### 1.4.1 [queue](http://www.cplusplus.com/reference/queue/queue/)

##### 1.4.1.1 模板

`template <class T, class Container = deque<T> > class queue;`

##### 1.4.1.2 构造函数

* *initialize*

    `explicit queue (const container_type& ctnr);`

* *move-initialize*

    `explicit queue (container_type&& ctnr = container_type());`

* *allocator*

    `template <class Alloc> explicit queue (const Alloc& alloc);`

* *init + allocator*

    `template <class Alloc> queue (const container_type& ctnr, const Alloc& alloc);`

* *move-init + allocator*

    `template <class Alloc> queue (container_type&& ctnr, const Alloc& alloc);`

* *copy + allocator*

    `template <class Alloc> queue (const queue& x, const Alloc& alloc);`

* *move + allocator*

    `template <class Alloc> queue (queue&& x, const Alloc& alloc);`

类似地, 在使用非 `deque` 作为底层容器时需要指明, 如:

```c++
#include <iostream>
#include <deque>
#include <queue>
#include <list>
using namespace std;

int main()
{
    deque<int> dq({1, 2, 3, 4, 5});
    list<int> l({1, 2, 3, 4, 5});
    
    queue<int> q1(dq);
    queue<int, list<int>> q2(l);
    
    while (!q1.empty())
    {
        cout << q1.front() << " ";	// 输出 1 2 3 4 5
        q1.pop();
    }
    cout << endl;
    while (!q2.empty())
    {
        cout << q2.front() << " ";	// 输出 1 2 3 4 5
        q2.pop();
    }
    
    return 0;
}
```

##### 1.4.1.3 [成员函数](http://www.cplusplus.com/reference/queue/queue/queue/) (详见链接)

###### **[`back()`](http://www.cplusplus.com/reference/queue/queue/back/)**

###### **[`emplace()`](http://www.cplusplus.com/reference/queue/queue/emplace/)**

###### **[`empty()`](http://www.cplusplus.com/reference/queue/queue/empty/)**

###### **[`front()`](http://www.cplusplus.com/reference/queue/queue/front/)**

###### **[`pop()`](http://www.cplusplus.com/reference/queue/queue/pop/)**

###### **[`push()`](http://www.cplusplus.com/reference/queue/queue/push/)**

###### **[`size()`](http://www.cplusplus.com/reference/queue/queue/size/)**

###### **[`queue::swap()`](http://www.cplusplus.com/reference/queue/queue/swap/)**



#### 1.4.2 [priority_queue](http://www.cplusplus.com/reference/queue/priority_queue/)

Priority queues are a type of container adaptors, specifically designed such that its first element is always the greatest of the elements it contains, according to some *strict weak ordering* criterion.

Priority queues are implemented as *container adaptors*, which are classes that use an encapsulated object of a specific container class as its *underlying container*, providing a specific set of member functions to access its elements.

The standard container classes `vector` and `deque` fulfill the requirements of operations of priority queues. **By default, if no container class is specified for a particular `priority_queue` class instantiation, the standard container `vector` is used.**

##### 1.4.2.1 模板

`template <class T, class Container = vector<T>, class Compare = less<typename Container::value_type> > class priority_queue;`

##### 1.4.2.2 构造函数

* *initialize*

    `priority_queue (const Compare& comp, const Container& ctnr);`

* *range*

    `template <class InputIterator> priority_queue (InputIterator first, InputIterator last, const Compare& comp, const Container& ctnr);`

* *move-initialize*

    `explicit priority_queue (const Compare& comp = Compare(), Container&& ctnr = Container());`

* *move-range*

    `template <class InputIterator> priority_queue (InputIterator first, InputIterator last, const Compare& comp, Container&& ctnr = Container());`

* *allocator versions*

    `template <class Alloc> explicit priority_queue (const Alloc& alloc);`

    `template <class Alloc> priority_queue (const Compare& comp, const Alloc& alloc);`

    `template <class Alloc> priority_queue (const Compare& comp, const Container& ctnr, const Alloc& alloc);`

    `template <class Alloc> priority_queue (const Compare& comp, Container&& ctnr, const Alloc& alloc);`

    `template <class Alloc> priority_queue (const priority_queue& x, const Alloc& alloc);`

    `template <class Alloc> priority_queue (priority_queue&& x, const Alloc& alloc);`

##### 1.4.2.3 成员函数

###### **[`emplace()`](cplusplus.com/reference/queue/priority_queue/emplace/)** (详见链接)

###### **[`empty()`](cplusplus.com/reference/queue/priority_queue/empty/)** (详见链接)



###### **[`pop()`](http://www.cplusplus.com/reference/queue/priority_queue/pop/)**

> `void pop();`
>
> **Removes the element on top of the `priority_queue`, effectively reducing its `size` by one. The element removed is the one with the highest value.**
>
> The value of this element can be retrieved before being popped by calling member [`priority_queue::top`](http://www.cplusplus.com/priority_queue::top).
>
> This member function effectively calls the [`pop_heap`](http://www.cplusplus.com/pop_heap) algorithm to keep the *heap property* of `priority_queue` and then calls the member function [`pop_back`](http://www.cplusplus.com/vector::pop_back) of the underlying container object to remove the element.
>
> This calls the removed element's destructor.

###### **[`push()`](http://www.cplusplus.com/reference/queue/priority_queue/push/)** (详见链接)

###### **[`size()`](http://www.cplusplus.com/reference/queue/priority_queue/size/)** (详见链接)

###### **[`priority_queue::swap`](http://www.cplusplus.com/reference/queue/priority_queue/swap/)** (详见链接)



###### **[`top()`](http://www.cplusplus.com/reference/queue/priority_queue/top/)**

> `const_reference top() const;`
>
> **Returns a constant reference to the *top element* in the `priority_queue`.**
>
> The top element is the element that compares higher in the `priority_queue`, and the next that is removed from the container when `priority_queue::pop` is called.
>
> This member function effectively calls member [`front`](http://www.cplusplus.com/vector::front) of the underlying container object.

### 1.5 \<[set](http://www.cplusplus.com/reference/set/)\>

#### 1.5.1 [set](http://www.cplusplus.com/reference/set/set/)

**Sets are containers that store unique elements following a specific order.**

In a `set`, the value of an element also identifies it (the value is itself the *key*, of type `T`), and each value must be unique. The value of the elements in a `set` cannot be modified once in the container (the elements are always const), but they can be inserted or removed from the container.

Internally, the elements in a `set` are always sorted following a specific *strict weak ordering* criterion indicated by its internal [comparison object](http://www.cplusplus.com/set::key_comp) (of type `Compare`).

`set` containers are generally slower than [`unordered_set`](http://www.cplusplus.com/unordered_set) containers to access individual elements by their *key*, but they allow the direct iteration on subsets based on their order.

Sets are typically implemented as *binary search trees*.

##### 1.5.1.1 模板

```c++
template < class T,                        // set::key_type/value_type
           class Compare = less<T>,        // set::key_compare/value_compare
           class Alloc = allocator<T>      // set::allocator_type
           > class set;
```

##### 1.5.1.2 [构造函数](http://www.cplusplus.com/reference/set/set/set/) (详见链接)

##### 1.5.1.3 [成员函数](http://www.cplusplus.com/reference/set/set/set/) (部分)

###### **[`count()`](http://www.cplusplus.com/reference/set/set/count/)**

> `size_type count (const value_type& val) const;`
>
> **Searches the container for elements equivalent to *val* and returns the number of matches.**
>
> Because all elements in a set container are unique, the function can only return *1* (if the element is found) or zero (otherwise).
>
> Two elements of a set are considered equivalent if the container's [comparison object](http://www.cplusplus.com/set::key_comp) returns `false` reflexively (i.e., no matter the order in which the elements are passed as arguments).

###### **[`equal_range()`](http://www.cplusplus.com/reference/set/set/equal_range/)**

> `pair<const_iterator,const_iterator> equal_range (const value_type& val) const;`
>
> `pair<iterator,iterator> equal_range (const value_type& val);`
>
> **Returns the bounds of a range that includes all the elements in the container that are equivalent to *val*.**
>
> **Because all elements in a set container are unique, the range returned will contain a single element at most.**
>
> If no matches are found, the range returned has a length of zero, with both iterators pointing to the first element that is considered to go after *val* according to the container's [internal comparison object](http://www.cplusplus.com/set::key_comp) ([key_comp](http://www.cplusplus.com/set::key_comp)).
>
> Two elements of a set are considered equivalent if the container's [comparison object](http://www.cplusplus.com/set::key_comp) returns `false` reflexively (i.e., no matter the order in which the elements are passed as arguments).

###### **[`erase()`](http://www.cplusplus.com/reference/set/set/erase/)**

> (1) `iterator erase (const_iterator position);`
>
> (2) `size_type erase (const value_type& val);`
>
> (3) `iterator erase (const_iterator first, const_iterator last);`
>
> **Removes from the set container either a single element or a range of elements (`[first,last)`).**
>
> **Return value:**
>
> For the value-based version (2), the function returns the number of elements erased.

###### **[`find()`](http://www.cplusplus.com/reference/set/set/find/)**

> `const_iterator find (const value_type& val) const;`
>
> `iterator find (const value_type& val);`
>
> **Searches the container for an element equivalent to *val* and returns an iterator to it if found, otherwise it returns an iterator to [set::end](http://www.cplusplus.com/set::end).**

###### **[`insert()`](http://www.cplusplus.com/reference/set/set/insert/)**

> (1) *single element*
>
> `pair<iterator,bool> insert (const value_type& val);`
>
> `pair<iterator,bool> insert (value_type&& val);`
>
> (2) *with hint*
>
> `iterator insert (const_iterator position, const value_type& val);`
>
> `iterator insert (const_iterator position, value_type&& val);`
>
> (3) *range*
>
> `template <class InputIterator> void insert (InputIterator first, InputIterator last);`
>
> (4) *initializer list*
>
> `void insert (initializer_list<value_type> il);`
>
> **Extends the container by inserting new elements, effectively increasing the container `size` by the number of elements inserted.**
>
> Because elements in a set are unique, the insertion operation checks whether each inserted element is equivalent to an element already in the container, and if so, the element is not inserted, returning an iterator to this existing element (if the function returns a value).
>
> **Return value:**
>
> The single element versions (1) return a [`pair`](http://www.cplusplus.com/pair), with its member `pair::first` set to an iterator pointing to either the newly inserted element or to the equivalent element already in the set. The `pair::second` element in the `pair` is set to `true` if a new element was inserted or `false` if an equivalent element already existed.
>
> The versions with a hint (2) return an iterator pointing to either the newly inserted element or to the element that already had its same value in the set.

###### **[`lower_bound()`](http://www.cplusplus.com/reference/set/set/lower_bound/)**

> `iterator lower_bound (const value_type& val);`
>
> `const_iterator lower_bound (const value_type& val) const;`
>
> **Returns an iterator pointing to the first element in the container which is not considered to go before *val* (i.e., either it is equivalent or goes after).**

###### **[`upper_bound()`](http://www.cplusplus.com/reference/set/set/upper_bound/)**

> `iterator upper_bound (const value_type& val);`
>
> `const_iterator upper_bound (const value_type& val) const;`
>
> **Returns an iterator pointing to the first element in the container which is considered to go after *val*.**

#### 1.5.2 [multiset](http://www.cplusplus.com/reference/set/multiset/)

**Multisets are containers that store elements following a specific order, and where multiple elements can have equivalent values.**

In a `multiset`, the value of an element also identifies it (the value is itself the *key*, of type `T`). The value of the elements in a `multiset` cannot be modified once in the container (the elements are always const), but they can be inserted or removed from the container.

##### 1.5.2.1 模板

```c++
template < class T,                        // multiset::key_type/value_type
           class Compare = less<T>,        // multiset::key_compare/value_compare
           class Alloc = allocator<T> >    // multiset::allocator_type
           > class multiset;
```

##### 1.5.2.2 [构造函数](http://www.cplusplus.com/reference/set/multiset/multiset/) (详见链接)

##### 1.5.2.3 [成员函数](http://www.cplusplus.com/reference/set/multiset/) (部分)

###### **[`count()`](http://www.cplusplus.com/reference/set/multiset/count/)**

> `size_type count (const value_type& val) const;`
>
> **Searches the container for elements equivalent to *val* and returns the number of matches.**

###### **[`equal_range()`](http://www.cplusplus.com/reference/set/multiset/equal_range/)**

>`pair<const_iterator,const_iterator> equal_range (const value_type& val) const;`
>
>`pair<iterator,iterator> equal_range (const value_type& val);`
>
>**Returns the bounds of a range that includes all the elements in the container that are equivalent to *val*.**
>
>**Return value:**
>
>The function returns a [`pair`](http://www.cplusplus.com/pair), whose member `pair::first` is the lower bound of the range (the same as [`lower_bound`](http://www.cplusplus.com/multiset::lower_bound)), and `pair::second` is the upper bound (the same as [`upper_bound`](http://www.cplusplus.com/multiset::upper_bound)).

###### **[`erase()`](http://www.cplusplus.com/reference/set/multiset/erase/)**

> (1) `iterator  erase (const_iterator position);`
>
> (2) `size_type erase (const value_type& val);`
>
> (3) `iterator  erase (const_iterator first, const_iterator last);`
>
> **Removes elements from the multiset container.**
>
> This effectively reduces the container size by the number of elements removed, which are destroyed.
>
> **Return value:**
>
> For the value-based version (2), the function returns the number of elements erased.

###### **[`find()`](http://www.cplusplus.com/reference/set/multiset/find/)**

> `const_iterator find (const value_type& val) const;`
>
> `iterator find (const value_type& val);`
>
> **Searches the container for an element equivalent to *val* and returns an iterator to it if found, otherwise it returns an iterator to [`multiset::end`](http://www.cplusplus.com/multiset::end).**
>
> Notice that this function returns an iterator to a single element (of the possibly multiple equivalent elements). To obtain the entire range of equivalent elements, see [`multiset::equal_range`](http://www.cplusplus.com/multiset::equal_range).

###### **[`insert()`](http://www.cplusplus.com/reference/set/multiset/insert/)** (详见链接)

###### **[`lower_bound()`](http://www.cplusplus.com/reference/set/multiset/lower_bound/)** 与 **[`upper_bound()`](http://www.cplusplus.com/reference/set/multiset/upper_bound/)** (详见链接)



### 1.6 \<[unordered_set](http://www.cplusplus.com/reference/unordered_set/)\> (C++ 11) (详见链接)

### 1.7 \<[map](http://www.cplusplus.com/reference/map/)\>



## 2. Other

### 2.1 \<[string](http://www.cplusplus.com/reference/string/string/)\>

#### 2.1.1 定义

`typedef basic_string<char> string;`

#### 2.1.2 构造函数

* *default*

    `string();`

* *copy*

    `string (const string& str);`

* *substring*

    `string (const string& str, size_t pos, size_t len = npos);`

* *from c-string*

    `string (const char* s);`

* *from buffer*

    `string (const char* s, size_t n);`

* *fill*

    `string (size_t n, char c);`

* *range*

  `template <class InputIterator> string  (InputIterator first, InputIterator last);`
  
* *initializer list*

    `string (initializer_list<char> il);`

* *move*

    `string (string&& str) noexcept;`

#### 2.1.3 [成员函数](http://www.cplusplus.com/reference/string/string/) (部分)

##### **[`append()`](http://www.cplusplus.com/reference/string/string/append/)**

> * *string*
>
>     `string& append (const string& str);`
>
> * *substring*
>
>     `string& append (const string& str, size_t subpos, size_t sublen);`
>
> * *c-string*
>
>     `string& append (const char* s);`
>
> * *buffer*
>
>     `string& append (const char* s, size_t n);`
>
> * *fill*
>
>     `string& append (size_t n, char c);`
>
> * *range*
>
>     `template <class InputIterator> string& append (InputIterator first, InputIterator last);`
>
> * *initializer list*
>
>     `string& append (initializer_list<char> il);`
>
> **Extends the string by appending additional characters at the end of its current value**

##### **[`assign()`](http://www.cplusplus.com/reference/string/string/assign/)**

> * *string*
>
>     `string& assign (const string& str);`
>
> * *substring*
>
>     `string& assign (const string& str, size_t subpos, size_t sublen);`
>
> * *c-string*
>
>     `string& assign (const char* s);`
>
> * *buffer*
>
>     `string& assign (const char* s, size_t n);`
>
> * *fill*
>
>     `string& assign (size_t n, char c);`
>
> * *range*
>
>     `template <class InputIterator> string& assign (InputIterator first, InputIterator last);`
>
> * *initializer list*
>
>     `string& assign (initializer_list<char> il);`
>
> * *move*
>
>     `string& assign (string&& str) noexcept;`
>
> **Assigns a new value to the string, replacing its current contents.**

##### **[`compare()`](http://www.cplusplus.com/reference/string/string/compare/)**

> * *string*
>
>     `int compare (const string& str) const noexcept;`
>
> * *substrings*
>
>     `int compare (size_t pos, size_t len, const string& str) const;`
>
>     `int compare (size_t pos, size_t len, const string& str, size_t subpos, size_t sublen) const;`
>
> * *c-string*
>
>     `int compare (const char* s) const;`
>
>     `int compare (size_t pos, size_t len, const char* s) const;`
>
> * *buffer*
>
>     `int compare (size_t pos, size_t len, const char* s, size_t n) const;`
>
> **Compares the value of the string object (or a substring) to the sequence of characters specified by its arguments.**
>
> The compared string is the value of the string object or -if the signature used has a *pos* and a *len* parameters- the substring that begins at its character in position *pos* and spans *len* characters.
>
> This string is compared to a comparing string, which is determined by the other arguments passed to the function.

##### **[`copy()`](http://www.cplusplus.com/reference/string/string/copy/)**

> `size_t copy (char* s, size_t len, size_t pos = 0) const;`
>
> **Copies a substring of the current value of the string object into the array pointed by *s*. This substring contains the *len* characters that start at position *pos*.**
>
> The function does not append a null character at the end of the copied content.
>
> **Return value:**
>
> The number of characters copied to the array pointed by *s*. This may be equal to *len* or to `length() - pos` (if the string value is shorter than `pos + len`).

将string对象 *pos* 起始的 *len* 个字符拷贝到字符型数组s中, 并返回拷贝的字符数. 本函数不会在s中自动添加`'\0'`, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    char buffer[20];
    string str("hello world!");
    size_t len = str.copy(buffer, 5, 6);
    buffer[len] = '\0';
    cout << buffer << endl;		// 输出 world
    
    return 0;
}
```

##### **[`c_str()`](http://www.cplusplus.com/reference/string/string/c_str/)**

> `const char* c_str() const noexcept;`
>
> **Returns a pointer to an array that contains a null-terminated sequence of characters (i.e., a C-string) representing the current value of the string object.**
>
> This array includes the same sequence of characters that make up the value of the string object plus an additional terminating null-character (`'\0'`) at the end.
>
> **Return value:**
>
> A pointer to the c-string representation of the string object's value.

可用于将 string 对象转换为 c-string 后利用`sscanf`进行格式化输入, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string date;
    cin >> date;				// 输入格式为 yyyy-mm-dd, 如 "1998-06-29"
    int year, month, day;
    sscanf(date.c_str(), "%d-%d-%d", &year, &month, &day);
    cout << "year: " << year
         << ", month: " << month
         << ", day: " << day
         << endl;				// 输出 year: 1998, month: 6, day: 29
    
    return 0;
}
```

##### **[`data()`](http://www.cplusplus.com/reference/string/string/data/)**

> `const char* data() const noexcept;`
>
> **Returns a pointer to an array that contains a null-terminated sequence of characters (i.e., a C-string) representing the current value of the string object.**
>
> This array includes the same sequence of characters that make up the value of the string object plus an additional terminating null-character (`'\0'`) at the end.
>
> The pointer returned points to the internal array currently used by the string object to store the characters that conform its value.
>
> Both `string::data` and `string::c_str` are synonyms and return the same value.

用法和 `string::c_str` 大致相同, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string str("hello world");
    char* s = new char[str.length() + 1];
    strcpy(s, str.data());
    cout << s << endl;		// 输出 hello world
    delete[] s;
    
    return 0;
}
```

##### **[`erase()`](http://www.cplusplus.com/reference/string/string/erase/)**

> * *sequence*
>
>     `string& erase (size_t pos = 0, size_t len = npos);`
>
> * *character*
>
>     `iterator erase (const_iterator p);`
>
> * *range*
>
>     `iterator erase (const_iterator first, const_iterator last);`
>
> **Erases part of the string, reducing its `length`**:
>
> 1. sequence
>
>     Erases the portion of the string value that begins at the character position *pos* and spans *len* characters (or until the *end of the string*, if either the content is too short or if *len* is [string::npos](http://www.cplusplus.com/string::npos).
>     Notice that the default argument erases all characters in the string (like member function [clear](http://www.cplusplus.com/string::clear)).
>
> 2. character
>
>     Erases the character pointed by *p*.
>
> 3. range
>
>     Erases the sequence of characters in the range `[first,last)`.

除利用迭代器进行删除字符之外, 还可指定起始位置 *pos* 和 要删除的连续字符数 *len*, 若仅指定 *pos*, 则删除 *pos* 及其后面的所有字符, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string s("This is an example sentence.");
    s.erase(9, 9);
    cout << s << endl;		// 输出 This is a sentence.
    s.erase(9);
    s.append("n apple");
    cout << s << endl;		// 输出 This is an apple
    
    return 0;
}
```

##### **[`find()`](http://www.cplusplus.com/reference/string/string/find/)**

> * *string*
>
>     `size_t find (const string& str, size_t pos = 0) const noexcept;`
>
> * *c-string*
>
>     `size_t find (const char* s, size_t pos = 0) const;`
>
> * *buffer*
>
>     `size_t find (const char* s, size_t pos, size_type n) const;`
>
> * *character*
>
>     `size_t find (char c, size_t pos = 0) const noexcept;`
>
> **Searches the string for the first occurrence of the sequence specified by its arguments.**
>
> When *pos* is specified, the search only includes characters at or after position *pos*, ignoring any possible occurrences that include characters before *pos*.
>
> Notice that unlike member [find_first_of](http://www.cplusplus.com/string::find_first_of), whenever more than one character is being searched for, it is not enough that just one of these characters match, but the entire sequence must match.
>
> **Return value:**
>
> The position of the first character of the first match. If no matches were found, the function returns [string::npos](http://www.cplusplus.com/string::npos).

##### **[`rfind()`](http://www.cplusplus.com/reference/string/string/rfind/)**

> * *string*
>
>     `size_t rfind (const string& str, size_t pos = npos) const noexcept;`
>
> * *c-string*
>
>     `size_t rfind (const char* s, size_t pos = npos) const;`
>
> * *buffer*
>
>     `size_t rfind (const char* s, size_t pos, size_t n) const;`
>
> * *character*
>
>     `size_t rfind (char c, size_t pos = npos) const noexcept;`
>
> **Searches the string for the last occurrence of the sequence specified by its arguments.**
>
> When *pos* is specified, the search only includes sequences of characters that begin at or before position *pos*, ignoring any possible match beginning after *pos*.
>
> **Return value:**
>
> The position of the first character of the last match. If no matches were found, the function returns [string::npos](http://www.cplusplus.com/string::npos).

若未指定 *pos*, 则从 `*this` 的最后向前, 否则在 `*this` 中从 *pos* 起向前查找 *str* 或 *s*, 若找到则返回其首字符所在位置, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main ()
{
    string str("The sixth sick sheik's sixth sheep's sick.");
    string key("sixth");

    size_t found = str.rfind(key);
    if (found != string::npos)
        str.replace(found, key.length(), "seventh");
    cout << str << '\n';	// 输出 The sixth sick sheik's seventh sheep's sick.

    return 0;
}
```



##### **[`find_first_of()`](http://www.cplusplus.com/reference/string/string/find_first_of/)**

> * *string*
>
>     `size_t find_first_of (const string& str, size_t pos = 0) const noexcept;`
>
> * *c-string*
>
>     `size_t find_first_of (const char* s, size_t pos = 0) const;`
>
> * *buffer*
>
>     `size_t find_first_of (const char* s, size_t pos, size_t n) const;`
>
> * *character*
>
>     `size_t find_first_of (char c, size_t pos = 0) const noexcept;`
>
> **Searches the string for the first character that matches any of the characters specified in its arguments.**
>
> When *pos* is specified, the search only includes characters at or after position *pos*, ignoring any possible occurrences before *pos*.
>
> Notice that it is enough for one single character of the sequence to match (not all of them). See [string::find](http://www.cplusplus.com/string::find) for a function that matches entire sequences.
>
> **Return value:**
>
> The position of the first character that matches. If no matches are found, the function returns [string::npos](http://www.cplusplus.com/string::npos).

该函数会在 `*this` 中查找 *str* 或 *s* 中的字符, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main ()
{
	string str ("Please, replace the vowels in this sentence by asterisks.");
	size_t found = str.find_first_of("aeiou");
	while (found != string::npos)
	{
        str[found] = '*';
        found = str.find_first_of("aeiou", found + 1);
    }
    cout << str << '\n';	// 输出 Pl**s*, r*pl*c* th* v*w*ls *n th*s s*nt*nc* by *st*r*sks.

    return 0;
}
```



##### **[`find_first_not_of()`](http://www.cplusplus.com/reference/string/string/find_first_not_of/)**

> * *string*
>
>     `size_t find_first_not_of (const string& str, size_t pos = 0) const noexcept;`
>
> * *c-string*
>
>     `size_t find_first_not_of (const char* s, size_t pos = 0) const;`
>
> * *buffer*
>
>     `size_t find_first_not_of (const char* s, size_t pos, size_t n) const;`
>
> * *character*
>
>     `size_t find_first_not_of (char c, size_t pos = 0) const noexcept;`
>
> **Searches the string for the first character that does not match any of the characters specified in its arguments.**
>
> When *pos* is specified, the search only includes characters at or after position *pos*, ignoring any possible occurrences before that character.

用于在 `*this` 中查找未出现在 *str* 或 *s* 中的字符, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main ()
{
	string str ("look for non-alphabetic characters...");
	size_t found = str.find_first_not_of("abcdefghijklmnopqrstuvwxyz ");

    if (found != string::npos)
    {
        // 以下输出 The first non-alphabetic character is - at position 12
        cout << "The first non-alphabetic character is " << str[found];
        cout << " at position " << found << '\n';
    }
    
    return 0;
}
```



##### **[`find_last_of()`](http://www.cplusplus.com/reference/string/string/find_last_of/)**

> * *string*
>
>     `size_t find_last_of (const string& str, size_t pos = npos) const noexcept;`
>
> * *c-string*
>
>     `size_t find_last_of (const char* s, size_t pos = npos) const;`
>
> * *buffer*
>
>     `size_t find_last_of (const char* s, size_t pos, size_t n) const;`
>
> * *character*
>
>     `size_t find_last_of (char c, size_t pos = npos) const noexcept;`
>
> **Searches the string for the last character that matches any of the characters specified in its arguments.**
>
> When *pos* is specified, the search only includes characters at or before position *pos*, ignoring any possible occurrences after *pos*.

若给定 *pos*, 则在 `*this` 的 *pos* 及其后面的字符中从后向前查找出现在 *str* 或 *s* 中的字符, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

void SplitFilename (const string& str)
{
    cout << "Splitting: " << str << '\n';
    size_t found = str.find_last_of("/\\");
    cout << " path: " << str.substr(0, found) << '\n';
    cout << " file: " << str.substr(found + 1) << '\n';
}

int main ()
{
    string str1 ("/usr/bin/man");
    string str2 ("c:\\windows\\winhelp.exe");
    SplitFilename (str1);
    SplitFilename (str2);
    /*
     * 输出结果为
     * Splitting: /usr/bin/man
     * path: /usr/bin
     * file: man
     * Splitting: c:\windows\winhelp.exe
     * path: c:\windows
     * file: winhelp.exe
    **/
    return 0;
}
```



##### **[`find_last_not_of()`](http://www.cplusplus.com/reference/string/string/find_last_not_of/)**

> * *string*
>
>     `size_t find_last_not_of (const string& str, size_t pos = npos) const noexcept;`
>
> * *c-string*
>
>     `size_t find_last_not_of (const char* s, size_t pos = npos) const;`
>
> * *buffer*
>
>     `size_t find_last_not_of (const char* s, size_t pos, size_t n) const;`
>
> * *character*
>
>     `size_t find_last_not_of (char c, size_t pos = npos) const noexcept;`
>
> **Searches the string for the last character that does not match any of the characters specified in its arguments.**
>
> When *pos* is specified, the search only includes characters at or before position *pos*, ignoring any possible occurrences after *pos*.

##### **[`insert()`](http://www.cplusplus.com/reference/string/string/insert/)**

> * *string*
>
>     `string& insert (size_t pos, const string& str);`
>
> * *substring*
>
>     `string& insert (size_t pos, const string& str, size_t subpos, size_t sublen);`
>
> * *c-string*
>
>     `string& insert (size_t pos, const char* s);`
>
> * *buffer*
>
>     `string& insert (size_t pos, const char* s, size_t n);`
>
> * *fill*
>
>     `string& insert (size_t pos, size_t n, char c);`
>
>     `iterator insert (const_iterator p, size_t n, char c);`
>
> * *single character*
>
>     `iterator insert (const_iterator p, char c);`
>
> * *range*
>
>     `template <class InputIterator> iterator insert (iterator p, InputIterator first, InputIterator last);`
>
> * *initializer list*
>
>     `string& insert (const_iterator p, initializer_list<char> il);`
>
> **Inserts additional characters into the string right before the character indicated by *pos* (or *p*):**
>
> 1. *string*
>
>     Inserts a copy of *str*.
>
> 2. *substring*
>
>     Inserts a copy of a substring of *str*. The substring is the portion of *str* that begins at the character position *subpos* and spans *sublen* characters (or until the end of *str*, if either *str* is too short or if *sublen* is [npos](http://www.cplusplus.com/string::npos)).
>
> 3. *c-string*
>
>     Inserts a copy of the string formed by the null-terminated character sequence (C-string) pointed by *s*.
>
> 4. *buffer*
>
>     Inserts a copy of the first *n* characters in the array of characters pointed by *s*.
>
> 5. *fill*
>
>     Inserts *n* consecutive copies of character *c*.
>
> 6. *single character*
>
>     Inserts character *c*.
>
> 7. *range*
>
>     Inserts a copy of the sequence of characters in the range `[first,last)`, in the same order.
>
> 8. *initializer list*
>
>     Inserts a copy of each of the characters in *il*, in the same order.
>     
>     **Return value:**
>     
>     The signatures returning a reference to string, return `*this`.
>     Those returning an `iterator`, return an iterator pointing to the first character inserted.
>     
>     Member type `iterator` is a [random access iterator](http://www.cplusplus.com/RandomAccessIterator) type that points to characters of the string.

##### **[`operator+=()`](http://www.cplusplus.com/reference/string/string/operator+=/)**

> * *string*
>
>     `string& operator+= (const string& str);`
>
> * *c-string*
>
>     `string& operator+= (const char* s);`
>
> * *character*
>
>     `string& operator+= (char c);`
>
> * *initializer list*
>
>     `string& operator+= (initializer_list<char> il);`
>
> **Extends the string by appending additional characters at the end of its current value.**

##### **[`replace()`](http://www.cplusplus.com/reference/string/string/replace/)**

> * *string*
>
>     `string& replace (size_t pos, size_t len, const string& str);`
>
>     `string& replace (const_iterator i1, const_iterator i2, const string& str);`
>
> * *substring*
>
>     `string& replace (size_t pos, size_t len, const string& str, size_t subpos, size_t sublen);`
>
> * *c-string*
>
>     `string& replace (size_t pos, size_t len, const char* s);`
>
>     `string& replace (const_iterator i1, const_iterator i2, const char* s);`
>
> * *buffer*
>
>     `string& replace (size_t pos, size_t len, const char* s, size_t n);`
>
>     `string& replace (const_iterator i1, const_iterator i2, const char* s, size_t n);`
>
> * *fill*
>
>     `string& replace (size_t pos, size_t len, size_t n, char c);`
>
>     `string& replace (const_iterator i1, const_iterator i2, size_t n, char c);`
>
> * *range*
>
>     `template <class InputIterator> string& replace (const_iterator i1, const_iterator i2, InputIterator first, InputIterator last);`
>
> * *initializer list*
>
>     `string& replace (const_iterator i1, const_iterator i2, initializer_list<char> il);`
>
> **Replaces the portion of the string that begins at character *pos* and spans *len* characters (or the part of the string in the range between `[i1,i2)`) by new contents:**
>
> 1. *string*
>
>     Copies *str*.
>
> 2. *substring*
>
>     Copies the portion of *str* that begins at the character position *subpos* and spans *sublen* characters (or until the end of *str*, if either *str* is too short or if *sublen* is [string::npos](http://www.cplusplus.com/string::npos)).
>
> 3. *c-string*
>
>     Copies the null-terminated character sequence (C-string) pointed by *s*.
>
> 4. *buffer*
>
>     Copies the first *n* characters from the array of characters pointed by *s*.
>
> 5. *fill*
>
>     Replaces the portion of the string by *n* consecutive copies of character *c*.
>
> 6. *range*
>
>     Copies the sequence of characters in the range `[first,last)`, in the same order.
>
> 7. *initializer list*
>
>     Copies each of the characters in *il*, in the same order.

##### **[`resize()`](http://www.cplusplus.com/reference/string/string/resize/)**

> `void resize (size_t n);`
>
> `void resize (size_t n, char c);`
>
> **Resizes the string to a `length` of *n* characters.**
>
> If *n* is smaller than the current string length, the current value is shortened to its first *n* character, removing the characters beyond the *n*th.
>
> If *n* is greater than the current string length, the current content is extended by inserting at the end as many characters as needed to reach a size of *n*. If *c* is specified, the new elements are initialized as copies of *c*, otherwise, they are *value-initialized characters* (null characters).

##### **[`substr()`](http://www.cplusplus.com/reference/string/string/substr/)**

> `string substr (size_t pos = 0, size_t len = npos) const;`
>
> **Returns a newly constructed string object with its value initialized to a copy of a substring of this object.**
>
> The substring is the portion of the object that starts at character position pos and spans len characters (or until the end of the string, whichever comes first).

##### **[`string::swap()`](http://www.cplusplus.com/reference/string/string/swap/)**

> `void swap (string& str);`
>
> **Exchanges the content of the container by the content of *str*, which is another string object. Lengths may differ.**
>
> After the call to this member function, the value of this object is the value *str* had before the call, and the value of *str* is the value this object had before the call.
>
> Notice that a non-member function exists with the same name, [swap](http://www.cplusplus.com/string:swap), overloading that algorithm with an optimization that behaves like this member function.

##### **[`stoi()`](http://www.cplusplus.com/reference/string/stoi/)** (static) (C++ 11)

> `int stoi (const string&  str, size_t* idx = 0, int base = 10);`
>
> `int stoi (const wstring& str, size_t* idx = 0, int base = 10);`
>
> **Parses *str* interpreting its content as an integral number of the specified *base*, which is returned as an `int` value.**
>
> If *idx* is not a null pointer, the function also sets the value of *idx* to the position of the first character in *str* after the number.
>
> The function uses [strtol](http://www.cplusplus.com/strtol) (or [wcstol](http://www.cplusplus.com/wcstol)) to perform the conversion (see [strtol](http://www.cplusplus.com/strtol) for more details on the process).

该函数参数中 *idx* 可以取得数字之后的首字符位置, 如:

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
	string str("8888...!!!");
    size_t p;
    cout << stoi(str, &p) << endl << str.substr(p) << endl;
    /*
     * 输出如下
     * 8888
     * ...!!!
    **/
    return 0;
}
```



##### **[`to_string()`](http://www.cplusplus.com/reference/string/to_string/)** (static) (C++ 11)

> ```c++
> string to_string (int val);
> string to_string (long val);
> string to_string (long long val);
> string to_string (unsigned val);
> string to_string (unsigned long val);
> string to_string (unsigned long long val);
> string to_string (float val);
> string to_string (double val);
> string to_string (long double val);
> ```
>
> **Returns a string with the representation of *val*.**
>
> The format used is the same that [printf](http://www.cplusplus.com/printf) would print for the corresponding type.

#### 2.1.4 非成员函数

##### **[`getline(string)`](http://www.cplusplus.com/reference/string/string/getline/)**

> (1) `istream& getline (istream&  is, string& str, char delim);`
>
> ​	 `istream& getline (istream&& is, string& str, char delim);`
>
> (2) `istream& getline (istream&  is, string& str);`
>
> ​	 `istream& getline (istream&& is, string& str);`
>
> **Extracts characters from *is* and stores them into *str* until the delimitation character *delim* is found (or the newline character, `'\n'`, for *(2)*).**
>
> The extraction also stops if the end of file is reached in *is* or if some other error occurs during the input operation.
>
> If the delimiter is found, it is extracted and discarded (i.e. it is not stored and the next input operation will begin after it).
>
> Note that any content in str before the call is replaced by the newly extracted sequence.
>
> Each extracted character is appended to the [string](http://www.cplusplus.com/string) as if its member [push_back](http://www.cplusplus.com/string::push_back) was called.

### 2.2 \<[algorithm](http://www.cplusplus.com/reference/algorithm/)\>

#### **[`sort()`](http://www.cplusplus.com/reference/algorithm/sort/)**

> * *default*
>
>     `template <class RandomAccessIterator> void sort (RandomAccessIterator first, RandomAccessIterator last);`
>
> * *custom*
>
>     `template <class RandomAccessIterator, class Compare> void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);`
>
> **Sorts the elements in the range `[first,last)` into ascending order.**
>
> The elements are compared using `operator<` for the first version, and comp for the second.
>
> Equivalent elements are not guaranteed to keep their original relative order (see [stable_sort](http://www.cplusplus.com/stable_sort))

`sort()` 函数可以通过重载 `operator<` 来对结构体或类对象进行排序, 如:

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

template <class T>
void printElems(const T& t)
{
    if (t.empty())
    {
        cout << "Empty" << endl;
        return;
    }
    for (int i = 0; i < t.size(); i++)
    {
        cout << t[i];
        if (i < t.size() - 1)
            cout << " ";
    }
    cout << endl;
}

class point
{
private:
    double x, y;
public:
    explicit point(double x = 0, double y = 0) : x(x), y(y) {}
    [[nodiscard]] double getX() const { return x; }
    [[nodiscard]] double getY() const { return y; }
    void setX(double x) { this->x = x; }
    void setY(double y) { this->y = y; }
};

inline bool operator < (const point& p1, const point& p2)
{
    double x1 = p1.getX(), x2 = p2.getX();
    double y1 = p1.getY(), y2 = p2.getY();
    return x1 != x2 ? x1 < x2 : y1 < y2;
}

inline ostream & operator << (ostream& os, const point& p)
{
    os << "(" << p.getX() << ", " << p.getY() << ")";
    return os;
}

int main()
{
    vector<point> points;
    points.emplace_back(1.2, 2.6);
    points.emplace_back(3.7, 2.1);
    points.emplace_back(1.2, 1.8);
    points.emplace_back(0.4, 5.2);
    sort(points.begin(), points.end());
    printElems(points);		// 输出 (0.4, 5.2) (1.2, 1.8) (1.2, 2.6) (3.7, 2.1)

    return 0;
}
```

#### **[`min()`](http://www.cplusplus.com/reference/algorithm/min/)** 与 **[`max()`](http://www.cplusplus.com/reference/algorithm/max/)** (详见链接)

#### **[`min_element()`](http://www.cplusplus.com/reference/algorithm/min_element/)** 与 **[`max_element()`](http://www.cplusplus.com/reference/algorithm/max_element/)**

> * *default*
>
>     `template <class ForwardIterator> ForwardIterator min_element (ForwardIterator first, ForwardIterator last);`
>
>     `template <class ForwardIterator> ForwardIterator max_element (ForwardIterator first, ForwardIterator last);`
>
> * *custom*
>
>     `template <class ForwardIterator, class Compare> ForwardIterator min_element (ForwardIterator first, ForwardIterator last, Compare comp);`
>
>     `template <class ForwardIterator, class Compare> ForwardIterator max_element (ForwardIterator first, ForwardIterator last, Compare comp);`
>
> **Returns an iterator pointing to the element with the smallest / largest value in the range `[first,last)`.**
>
> The comparisons are performed using either `operator<` for the first version, or comp for the second; An element is the smallest / largest if no other element does not compare greater / less than it. If more than one element fulfills this condition, the iterator returned points to the first of such elements.

For instance, the behavior of this function template is equivalent to:

```c++
template <class ForwardIterator>
ForwardIterator min_element (ForwardIterator first, ForwardIterator last)
{
    if (first==last) return last;
    ForwardIterator smallest = first;

    while (++first != last)
        if (*first < *smallest)
// or: if (comp(*first, *smallest)) for version (2)
            smallest = first;
    return smallest;
}
```



#### **[`nth_element()`](http://www.cplusplus.com/reference/algorithm/nth_element/)**

> * *default*
>
>     `template <class RandomAccessIterator> void nth_element (RandomAccessIterator first, RandomAccessIterator nth, RandomAccessIterator last);`
>
> * *custom*
>
>     `template <class RandomAccessIterator, class Compare> void nth_element (RandomAccessIterator first, RandomAccessIterator nth, RandomAccessIterator last, Compare comp);`
>
> **Rearranges the elements in the range `[first,last)`, in such a way that the element at the *nth* position is the element that would be in that position in a sorted sequence.**

类似于快排的 `partition` 函数, 在 `[first, last)` 内中, 将排序后应在 *nth* 所指的位置的元素放在该处, 其左边元素均不大于它, 右边元素均不小于它, 如: 

```c++
#include <bits/stdc++.h>
using namespace std;

template <class T>
void printElems(const T& t)
{
    if (t.empty())
    {
        cout << "Empty" << endl;
        return;
    }
    for (int i = 0; i < t.size(); i++)
    {
        cout << t[i];
        if (i < t.size() - 1)
            cout << " ";
    }
    cout << endl;
}

int main()
{
    vector<int> nums{1, 2, 3, 4, 5, 6, 7, 8};
    random_shuffle(nums.begin(), nums.end());
    printElems(nums);	// 如, 输出 5 2 7 3 1 6 8 4
    nth_element(nums.begin(), nums.begin() + 3, nums.end());
    printElems(nums);
    // 如, 输出 2 1 3 4 5 6 8 7, 其中 4 为指定的 nth

    return 0;
}
```



#### **[`swap()`](http://www.cplusplus.com/reference/algorithm/swap/)** (详见源码)

#### **[`reverse()`](http://www.cplusplus.com/reference/algorithm/reverse/)**

> `template <class BidirectionalIterator> void reverse (BidirectionalIterator first, BidirectionalIterator last);`
>
> **Reverses the order of the elements in the range `[first,last)`.**

#### **[`reverse_copy()`](http://www.cplusplus.com/reference/algorithm/reverse_copy/)**

> `template <class BidirectionalIterator, class OutputIterator> OutputIterator reverse_copy (BidirectionalIterator first, BidirectionalIterator last, OutputIterator result);`
>
> **Copies the elements in the range `[first,last)` to the range beginning at result, but in reverse order.**

The behavior of this function template is equivalent to:

```c++
template <class BidirectionalIterator, class OutputIterator>
OutputIterator reverse_copy (BidirectionalIterator first, BidirectionalIterator last, OutputIterator result)
{
    while (first != last) {
        --last;
        *result = *last;
        ++result;
    }
    return result;
}
```

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
    int arr[] = {1, 2, 3, 4, 5, 6, 7, 8};
    vector<int> nums;
    nums.resize(8);
    reverse_copy(arr, arr + 8, nums.begin());
    for (const int& num : nums)
        cout << num << " ";		// 输出 8 7 6 5 4 3 2 1
    
    return 0;
}
```



#### **[`rotate()`](http://www.cplusplus.com/reference/algorithm/rotate/)**

> `template <class ForwardIterator> ForwardIterator rotate (ForwardIterator first, ForwardIterator middle, ForwardIterator last);`
>
> **Rotates the order of the elements in the range `[first,last)`, in such a way that the element pointed by middle becomes the new first element.**

The behavior of this function template (C++ 98) is equivalent to:

```c++
template <class ForwardIterator>
void rotate (ForwardIterator first, ForwardIterator middle, ForwardIterator last)
{
    ForwardIterator next = middle;
    while (first!=next)
    {
        swap (*first++,*next++);
        if (next == last) next = middle;
        else if (first == middle) middle = next;
    }
}
```

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main ()
{
    vector<int> nums{1, 2, 3, 4, 5, 6, 7, 8};
    rotate(nums.begin(), nums.begin() + 3, nums.end());

    for (auto it = nums.begin(); it != nums.end(); ++it)
        cout << *it << " ";		// 输出 4 5 6 7 8 1 2 3

    return 0;
}
```



#### **[`rotate_copy()`](http://www.cplusplus.com/reference/algorithm/rotate_copy/)** (详见链接)

#### **[`unique()`](http://www.cplusplus.com/reference/algorithm/unique/)**

> * *equality*
>
>     `template <class ForwardIterator> ForwardIterator unique (ForwardIterator first, ForwardIterator last);`
>
> * *predicate*
>
>     `template <class ForwardIterator, class BinaryPredicate> ForwardIterator unique (ForwardIterator first, ForwardIterator last, BinaryPredicate pred);`
>
> **Removes all but the first element from every consecutive group of equivalent elements in the range `[first,last)`.**
>
> The function cannot alter the properties of the object containing the range of elements (i.e., it cannot alter the size of an array or a container): The removal is done by replacing the duplicate elements by the next element that is not a duplicate, and signaling the new size of the shortened range by returning an iterator to the element that should be considered its new *past-the-end* element.
>
> The relative order of the elements not removed is preserved, while the elements between the returned iterator and last are left in a valid but unspecified state.
>
> The function uses `operator==` to compare the pairs of elements (or pred, in version *(2)*).
>
> **Return value:**
>
> An iterator to the element that follows the last element not removed. The range between first and this iterator includes all the elements in the sequence that were not considered duplicates.

The behavior of this function template is equivalent to:

```c++
template <class ForwardIterator>
ForwardIterator unique (ForwardIterator first, ForwardIterator last)
{
    if (first == last) return last;

    ForwardIterator result = first;
    while (++first != last)
    {
        if (!(*result == *first))
// or: if (!pred(*result, *first)) for version (2)
            *(++result) = *first;
    }
    return ++result;
}
```

由上易看出, 若欲利用 `unique()` 函数进行去重, 需要先使用 `sort()` 函数排序后再使用, 如:

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

template <class T>
void printElems(const T& t)
{
    if (t.empty())
    {
        cout << "Empty" << endl;
        return;
    }
    for (int i = 0; i < t.size(); i++)
    {
        cout << t[i];
        if (i < t.size() - 1)
            cout << " ";
    }
    cout << endl;
}

int main()
{
    vector<int> nums{1, 2, 2, 2, 3, 3, 2, 2, 1};
    unique(nums.begin(), nums.end());
    printElems(nums);
    // 输出 1 2 3 2 1 3 2 2 1, 其中 1 2 3 2 1 为结果

    sort(nums.begin(), nums.end());
    auto it = unique(nums.begin(), nums.end());
    printElems(nums);
    // 输出 1 2 3 2 2 2 2 3 3, 其中 1 2 3 为结果
    
    nums.resize(distance(nums.begin(), it));
    printElems(nums);
    // 输出 1 2 3
    
    return 0;
}
```

#### **[`adjacent_find()`](http://www.cplusplus.com/reference/algorithm/adjacent_find/)**

> * *equality*
>
>     `template <class ForwardIterator> ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last);`
>
> * *predicate*
>
>     `template <class ForwardIterator, class BinaryPredicate> ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last, BinaryPredicate pred);`
>
> **Searches the range `[first,last)` for the first occurrence of two consecutive elements that match, and returns an iterator to the first of these two elements, or last if no such pair is found.**
>
> Two elements match if they compare equal using `operator==` (or using *pred*, in version *(2)*).

The behavior of this function template is equivalent to:

```c++
template <class ForwardIterator>
ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last)
{
    if (first != last)
    {
        ForwardIterator next = first; ++next;
        while (next != last) {
            if (*first == *next)
// or: if (pred(*first,*next)), for version (2)
                return first;
            ++first; ++next;
        }
    }
    return last;
}
```



#### **[`all_of()`](http://www.cplusplus.com/reference/algorithm/all_of/)**

> `template <class InputIterator, class UnaryPredicate> bool all_of (InputIterator first, InputIterator last, UnaryPredicate pred);`
>
> **Returns `true` if pred returns `true` for all the elements in the range `[first,last)` or if the range is empty, and `false` otherwise.**

判断 `[first, last)` 中所有元素是否满足某条件 *pred*, 如:

```c++
#include <iostream>
#include <algorithm>
#include <array>
using namespace std;

int main()
{
    array<int, 8> foo = {3, 5, 7, 11, 13, 17, 19, 23};
    if (all_of(foo.begin(), foo.end(), [](int i){return i % 2;}))
        cout << "All odds\n";	// 输出 All odds
    return 0;
}
```



#### **[`any_of()`](http://www.cplusplus.com/reference/algorithm/any_of/)** (详见链接)

#### **[`binary_search()`](cplusplus.com/reference/algorithm/binary_search/)** (详见链接)

#### **[`copy()`](http://www.cplusplus.com/reference/algorithm/copy/), [`copy_if`](http://www.cplusplus.com/reference/algorithm/copy_if/), [`copy_backward()`](http://www.cplusplus.com/reference/algorithm/copy_backward/), [`copy_n()`](http://www.cplusplus.com/reference/algorithm/copy_n/)** (详见链接)

#### **[`count_if()`](http://www.cplusplus.com/reference/algorithm/count_if/)**

> `template <class InputIterator, class UnaryPredicate> typename iterator_traits<InputIterator>::difference_type count_if (InputIterator first, InputIterator last, UnaryPredicate pred);`
>
> **Returns the number of elements in the range `[first,last)` for which *pred* is true.**

The behavior of this function template is equivalent to:

```c++
template <class InputIterator, class UnaryPredicate>
typename iterator_traits<InputIterator>::difference_type count_if (InputIterator first, InputIterator last, UnaryPredicate pred)
{
    typename iterator_traits<InputIterator>::difference_type ret = 0;
    while (first != last) {
        if (pred(*first)) ++ret;
        ++first;
    }
    return ret;
}
```



#### **[`fill()`](http://www.cplusplus.com/reference/algorithm/fill/)**

> `template <class ForwardIterator, class T> void fill (ForwardIterator first, ForwardIterator last, const T& val);`
>
> **Assigns val to all the elements in the range `[first,last)`.**

The behavior of this function template is equivalent to:

```c++
template <class ForwardIterator, class T>
void fill (ForwardIterator first, ForwardIterator last, const T& val)
{
    while (first != last) {
        *first = val;
        ++first;
    }
}
```



#### **[`fill_n()`](http://www.cplusplus.com/reference/algorithm/fill_n/)** (详见链接)

#### **[`find_if()`](http://www.cplusplus.com/reference/algorithm/find_if/)**

> `template <class InputIterator, class UnaryPredicate> InputIterator find_if (InputIterator first, InputIterator last, UnaryPredicate pred);`
>
> **Returns an iterator to the first element in the range `[first,last)` for which *pred* returns `true`. If no such element is found, the function returns *last*.**

The behavior of this function template is equivalent to:

```c++
template<class InputIterator, class UnaryPredicate>
InputIterator find_if (InputIterator first, InputIterator last, UnaryPredicate pred)
{
    while (first != last) {
        if (pred(*first)) return first;
        ++first;
    }
    return last;
}
```



#### **[`lower_bound()`](http://www.cplusplus.com/reference/algorithm/lower_bound/)**

> * *default*
>
>     `template <class ForwardIterator, class T> ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last, const T& val);`
>
> * *custom*
>
>     `template <class ForwardIterator, class T, class Compare> ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last, const T& val, Compare comp);`
>
> **Returns an iterator pointing to the first element in the range `[first,last)` which does not compare less than *val*.**
>
> The elements are compared using `operator<` for the first version, and comp for the second. The elements in the range shall already be [sorted](http://www.cplusplus.com/is_sorted) according to this same criterion (`operator<` or comp), or at least [partitioned](http://www.cplusplus.com/is_partitioned) with respect to val.
>
> The function optimizes the number of comparisons performed by comparing non-consecutive elements of the sorted range, which is specially efficient for [random-access iterators](http://www.cplusplus.com/RandomAccessIterator).
>
> Unlike [upper_bound](http://www.cplusplus.com/upper_bound), the value pointed by the iterator returned by this function may also be equivalent to val, and not only greater.
>
> **Return value:**
>
> An iterator to the lower bound of *val* in the range. If all the element in the range compare less than *val*, the function returns *last*.

The behavior of this function template is equivalent to:

```c++
template <class ForwardIterator, class T>
ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last, const T& val)
{
    ForwardIterator it;
    iterator_traits<ForwardIterator>::difference_type count, step;
    count = distance(first, last);
    while (count > 0)
    {
        it = first; step = count / 2; advance(it, step);
        if (*it < val) {
// or: if (comp(*it, val)), for version (2)
            first = ++it;
            count -= step + 1;
        }
        else count = step;
    }
    return first;
}
```

由上代码可看出 `lower_bound()` 函数由二分查找实现, 由该函数返回的迭代器, 结合其他方法可实现一系列灵活的操作, 如:

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()
{
    vector<int> nums{1, 5, 3, 3, 5, 1, 1, 5};
    sort(nums.begin(), nums.end());	// 1 1 1 3 3 5 5 5
    auto low = lower_bound(nums.begin(), nums.end(), 3);
    nums.insert(low, 2);
    auto up = upper_bound(nums.begin(), nums.end(), 3);
    nums.insert(up, 4);

    for (const int& num : nums)
        cout << num << " ";	// 输出 1 1 1 2 3 3 4 5 5 5

    return 0;
}
```



