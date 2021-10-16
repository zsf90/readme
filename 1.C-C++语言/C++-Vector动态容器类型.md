向量（Vector）是一个封装了动态大小数组的顺序容器（Sequence Container）。跟任意其它类型容器一样，它能够存放各种类型的对象。可以简单的认为，向量是一个能够存放任意类型的动态数组。

## 容器特性

### 1.顺序序列

顺序容器中的元素按照严格的线性顺序排序。可以通过元素在序列中的位置访问对应的元素。

### 2.动态数组

支持对序列中的任意元素进行快速直接访问，甚至可以通过指针算述进行该操作。操供了在序列末尾相对快速地添加/删除元素的操作。

### 3.能够感知内存分配器的（Allocator-aware）

容器使用一个内存分配器对象来动态地处理它的存储需求。

## 基本函数

### 1.构造函数

* `vector()` 创建一个空 vector
* `vector(int nSize)` 创建一个 vector，元素数量为 nSize
* `vector(int nSize, const t& t)` 创建一个 vector，元素数量为 nSize，且值为 t）
* `vector(const vector&)` 复制构造函数
* `vector(begin, end)` 复制[begin, end] 区间内另一个数组的元素到vector中

### 2.添加数据函数

* `void push_back(const T& x)` 向量尾部增加一个元素 x
* `iterator insert(iterator it, const T& x)` 向量中迭代器指向元素前增加一个元素x
* `iterator insert(iterator it, int n, const T& x)` 向量中迭代器指向元素前增加n个相同的元素x
* `iterator insert(iterator it, const_iterator first, const_iterator last)` 向量中迭代器指向元素前插入另一个相同类型向量的[first, last]间的数据

### 3.删除函数

* iterator erase(iterator it) 删除向量中迭代器指向元素
* iterator erase(iterator first, iterator last) 删除向量中[first last] 中的元素
* void pop_back() 删除向量中最后一个元素
* void clear() 清空向量中所有元素

### 4.遍历函数

* `reference at(int pos)` 返回pos位置元素的引用
* `reference front()` 返回首元素的引用
* `reference_back()` 返回尾元素的引用
* `iterator begin()` 返回向量头指针，指向第一个元素
* `iterator end()` 返回向量尾指针，指向向量最后一个元素的下一个位置
* `reverse_iterator rbegin()` 反向迭代器，指向最后一个元素
* `reverse_iterator rend()` 反向迭代器，指向第一个元素之前的位置

### 5.判断函数

* `bool empty() const` 判断向量是否为空，若为空，则向量中无元素

### 6.大小函数

* `int size() const` 返回向量中元素的个数
* `int capacity() const` 返回当前向量所能容纳的最大元素
* `int max_size() const` 返回最大可允许的 vector 元素数量值

### 7.其他函数

* `void swap(vector&)` 交换两个同类型向量的数据
* `void assign(int n, const T& x)` 设置向量中前n个元素的值为x
* `void assign(const_iterator first, const_iterator last)` 向量中 [first, last] 中元素设置成当前向量元素

