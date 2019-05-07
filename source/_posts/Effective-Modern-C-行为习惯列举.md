title: Effective Modern C++ 行为习惯列举
author: Jiang Tao
tags:
  - c++
categories:
  - c++
date: 2019-05-07 16:57:00
---
# Effective Modern C++ 行为习惯列举

> 面向已经掌握了C++11之前的同学, 本文只列举了基本的几条.
>
> ref: Effective Modern C++


1.用**auto** 代替显示声明.

✖ 错误示范:

```c++
typename std::iterator_traits<It>::value_type currValue = *b;
```
<!-- more -->
✔ 正确示范:

```c++
auto currValue = *b;
```


2.使用**nullptr** 替代**NULL** 和**0**.

✖ 错误示范:

```c++
if (result == 0){
  ...
}
```

```c++
if (result == NULL){
	...
}
```

✔ 正确示范:

```c++
if (result == nullptr){
  ...
}
```



3.使用**using** 替代**typedef**.

✖ 错误示范:

```c++
typedef std::unique_ptr<std::unordered_map<std::string, std::string>> UPtrMapSS;
```

✔ 正确示范:

```c++
using UPtrMapSS = std::unique_ptr<std::unordered_map<std::string, std::string>>;
```



4.有范围的**enums** 替代无范围的**enums**.

✖ 错误示范:

```c++
enum Color { black, white, red };
auto white = false; //white 已经被声明了, error!
```

✔ 正确示范:

```c++
enum class Color { black, white, red };
auto white = false; // 一切正常
Color c = Color::white; //规范的声明方式
auto c = Color::white; //规范的声明方式
```

5.禁用函数时, 用**delete** 替代**private**.

✖ 错误示范:

```c++
class basic_ios : public ios_base { 
public:
	...
private:
	basic_ios(const basic_ios& ); 
	basic_ios& operator=(const basic_ios&); 
  ...
};
```

✔ 正确示范:

```c++
class basic_ios : public ios_base { 
public:
	basic_ios(const basic_ios& ) = delete; 
	basic_ios& operator=(const basic_ios&) = delete; 
  ...
};
```

6.使用**override** 关键字标注 override函数.

```c++
class Base { 
public:
	virtual void mf1() const; 
  virtual void mf2(int x); 
  virtual void mf3() &; 
  void mf4() const; 
};

class Derived: public Base { 
public:
	virtual void mf1() override; 
	virtual void mf2(unsigned int x) override; 
	virtual void mf3() && override; virtual void mf4() const override;
};
```

7.使用const_iterators 替代iterators.

✖ 错误示范:

```c++
std::vector<int> values;
…
auto it = std::find(values.begin(),values.end(), 1983); //使用begin()和end()
values.insert(it, 1998);
```

✔ 正确示范:

```c++
std::vector<int> values; 
…
auto it = std::find(values.cbegin(),values.cend(), 1983);//使用cbegin()和cend()
values.insert(it, 1998);
```



8.如果函数不会抛出异常, 使用**noexcept**进行声明.

✖ C++98:

```c++
int f(int x) throw();
```

✔ C++11:

```c++
int f(int x) noexcept;
```



9.使用智能指针 ``std::unique_ptr``, ``std::shared_ptr``,  ``std::weak_ptr``替代传统指针 (``std::auto`` 淘汰了别用了).

10.能用**constexpr**就用**constexpr**.

11.让*常成员函数*线程安全: 使用``std::mutex`` 或``std::atomic`` 等.

12.善用右值[*Rvalue*], 语义转移[*Move Semantics*], 完美转发[*Perfect Forwarding*]

13.善用Lambda 表达式

14.善用并发编程API

15.容器中使用``emplace_back()``,  替代``push_back()``