# C语言标准库函数回顾

## 1 \<[cstring](http://www.cplusplus.com/reference/cstring/)\>

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

## 2 \<[cmath](http://www.cplusplus.com/reference/cmath/)\> 

## 3 \<[cstdlib](http://www.cplusplus.com/reference/cstdlib/)\>

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

## 4 \<[ctime](http://www.cplusplus.com/reference/ctime/)\>

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

## 5 \<[cctype](http://www.cplusplus.com/reference/cctype/)\>

# C++ STL 概述 (C++ 11)

## 1. \<[vector](http://www.cplusplus.com/reference/vector/)\>

### 1.1 模板

`template < class T, class Alloc = allocator<T> > class vector; // generic template`

### 1.2 构造函数

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

### 1.3 [成员函数](http://www.cplusplus.com/reference/vector/vector/)  ( 部分 )

#### **[`assign()`](http://www.cplusplus.com/reference/vector/vector/assign/)**

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



#### **[`emplace()`](http://www.cplusplus.com/reference/vector/vector/emplace/)** (C++ 11)

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

#### **[`emplace_back()`](http://www.cplusplus.com/reference/vector/vector/emplace_back/)** (C++ 11)

> `template <class... Args> void emplace_back (Args&&... args);`
>
> **Inserts a new element at the end of the vector, right after its current last element. This new element is constructed in place using *args* as the arguments for its constructor.**

#### **[`erase()`](http://www.cplusplus.com/reference/vector/vector/erase/)**

> `iterator erase (const_iterator position);`
>
> `iterator erase (const_iterator first, const_iterator last);`
>
> **Removes from the vector either a single element (*position*) or a range of elements ( [`first, last`) ).**
>
> This effectively reduces the container `size` by the number of elements removed, which are destroyed.

#### **[`insert()`](http://www.cplusplus.com/reference/vector/vector/insert/)**

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

#### **[`at()`](http://www.cplusplus.com/reference/vector/vector/at/)** 与 **[`operator[]()`](http://www.cplusplus.com/reference/vector/vector/operator[]/)**

发生访问越界时, `at()`会报越界错误, 而使用`[]`不会报错, 可能访问到错误的内容. (详见源码)

#### **[`vector::swap()`](http://www.cplusplus.com/reference/vector/vector/swap/)**

> `void swap (vector& x);`
>
> **Exchanges the content of the container by the content of *x*, which is another vector object of the same type. Sizes may differ.**
>
> After the call to this member function, the elements in this container are those which were in *x* before the call, and the elements of *x* are those which were in this. All iterators, references and pointers remain valid for the swapped objects.
>
> Notice that a non-member function exists with the same name, [swap](http://www.cplusplus.com/vector:swap), overloading that algorithm with an optimization that behaves like this member function.

#### **[`resize()`](http://www.cplusplus.com/reference/vector/vector/resize/)**

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

####  **[`shrink_to_fit()`](http://www.cplusplus.com/reference/vector/vector/shrink_to_fit/)**

> `void shrink_to_fit();`
>
> **Requests the container to reduce its [capacity](http://www.cplusplus.com/vector::capacity) to fit its `size`.**

#### **[`clear()`](http://www.cplusplus.com/reference/vector/vector/clear/)**

> `void clear() noexcept;`
>
> **Removes all elements from the vector (which are destroyed), leaving the container with a `size` of `0`.**

## 2. \<[string](http://www.cplusplus.com/reference/string/string/)\>

### 2.1 定义

`typedef basic_string<char> string;`

### 2.2 构造函数

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

### 2.3 [成员函数](http://www.cplusplus.com/reference/string/string/) (部分)

#### **[`append()`](http://www.cplusplus.com/reference/string/string/append/)**

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

#### **[`assign()`](http://www.cplusplus.com/reference/string/string/assign/)**

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

#### **[`compare()`](http://www.cplusplus.com/reference/string/string/compare/)**

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

#### **[`copy()`](http://www.cplusplus.com/reference/string/string/copy/)**

> `size_t copy (char* s, size_t len, size_t pos = 0) const;`
>
> **Copies a substring of the current value of the string object into the array pointed by *s*. This substring contains the *len* characters that start at position *pos*.**
>
> The function does not append a null character at the end of the copied content.
>
> **Return value:**
>
> The number of characters copied to the array pointed by *s*. This may be equal to *len* or to `length() - pos` (if the string value is shorter than `pos + len`).

将string对象pos起始的len个字符拷贝到字符型数组s中, 并返回拷贝的字符数. 本函数不会在s中自动添加`'\0'`, 如:

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

#### **[`c_str()`](http://www.cplusplus.com/reference/string/string/c_str/)**

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

#### **[`data()`](http://www.cplusplus.com/reference/string/string/data/)**

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

#### **[`erase()`](http://www.cplusplus.com/reference/string/string/erase/)**

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

#### **[`find()`](http://www.cplusplus.com/reference/string/string/find/)**

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

#### **[`rfind()`](http://www.cplusplus.com/reference/string/string/rfind/)**

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



#### **[`find_first_of()`](http://www.cplusplus.com/reference/string/string/find_first_of/)**

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



#### **[`find_first_not_of()`](http://www.cplusplus.com/reference/string/string/find_first_not_of/)**

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



#### **[`find_last_of()`](http://www.cplusplus.com/reference/string/string/find_last_of/)**

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



#### **[`find_last_not_of()`](http://www.cplusplus.com/reference/string/string/find_last_not_of/)**

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

#### **[`insert()`](http://www.cplusplus.com/reference/string/string/insert/)**

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

#### **[`operator+=()`](http://www.cplusplus.com/reference/string/string/operator+=/)**

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

#### **[`replace()`](http://www.cplusplus.com/reference/string/string/replace/)**

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

#### **[`resize()`](http://www.cplusplus.com/reference/string/string/resize/)**

> `void resize (size_t n);`
>
> `void resize (size_t n, char c);`
>
> **Resizes the string to a `length` of *n* characters.**
>
> If *n* is smaller than the current string length, the current value is shortened to its first *n* character, removing the characters beyond the *n*th.
>
> If *n* is greater than the current string length, the current content is extended by inserting at the end as many characters as needed to reach a size of *n*. If *c* is specified, the new elements are initialized as copies of *c*, otherwise, they are *value-initialized characters* (null characters).

#### **[`substr()`](http://www.cplusplus.com/reference/string/string/substr/)**

> `string substr (size_t pos = 0, size_t len = npos) const;`
>
> **Returns a newly constructed string object with its value initialized to a copy of a substring of this object.**
>
> The substring is the portion of the object that starts at character position pos and spans len characters (or until the end of the string, whichever comes first).

#### **[`string::swap()`](http://www.cplusplus.com/reference/string/string/swap/)**

> `void swap (string& str);`
>
> **Exchanges the content of the container by the content of *str*, which is another string object. Lengths may differ.**
>
> After the call to this member function, the value of this object is the value *str* had before the call, and the value of *str* is the value this object had before the call.
>
> Notice that a non-member function exists with the same name, [swap](http://www.cplusplus.com/string:swap), overloading that algorithm with an optimization that behaves like this member function.

#### **[`stoi()`](http://www.cplusplus.com/reference/string/stoi/)** (static) (C++ 11)

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



#### **[`to_string()`](http://www.cplusplus.com/reference/string/to_string/)** (static) (C++ 11)

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

### 2.4 非成员函数

#### **[`getline(string)`](http://www.cplusplus.com/reference/string/string/getline/)**

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