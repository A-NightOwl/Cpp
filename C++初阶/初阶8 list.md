**本章重点**
1. list的介绍
2. list的基本使用
3. list的模拟实现


# 1.list的介绍
>1. list是可以在常数范围内在任意位置进行插入和删除的序列式容器，并且**该容器可以前后双向迭代**。
>   
>2. **list的底层是双向链表结构**，双向链表中每个元素存储在互不相关的独立节点中，在节点中通过指针指向其前一个元素和后一个元素。
>
>3. list与forward_list非常相似：最主要的不同在于**forward_list是单链表**，只能朝前迭代，已让其更简单高效。
>
>4. 与其他的序列式容器相比(array，vector，deque)，list通常在任意位置进行插入、移除元素的执行效率更好。
>
>5. 与其他序列式容器相比，**list和forward_list最大的缺陷是不支持任意位置的随机访问** 比如：要访问list的第6个元素，必须从已知的位置(比如头部或者尾部)迭代到该位置，在这段位置上迭代需要线性的时间开销；list还需要一些额外的空间，以保存每个节点的相关联信息(对于存储类型较小元素的大list来说这可能是一个重要的因素)

# 2.list的基本使用
## 2.1 插入结点元素:insert
```c++
	list<int> lt;
	lt.push_back(1);
	lt.push_back(2);
	lt.push_back(3);
	lt.push_back(4);
	lt.push_back(5);
	lt.push_back(6);

	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;
	
	list<int>::iterator it = lt.begin();
	for (size_t i = 0; i < 5; i++)
	{
		it++;
	}
	//在第五个位置的前面插入10
	//第五个位置——是下标为5，也就是第六个元素。
	lt.insert(it,10);


	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;
```
>说明：因为list不支持随机访问，因此**没有+，+=，[] 运算符重载**，但支持 **++，--**.
>
>这里的**迭代器，在插入之后，并没有失效**，因为结点的插入是**单个申请，单个释放**的。不存在 扩容的现象。

## 2.2 删除结点元素:erase
```c++
void test_list()
{
	list<int> lt;
	lt.push_back(2);
	lt.push_back(2);
	lt.push_back(3);
	lt.push_back(4);
	lt.push_back(90);
	lt.push_back(6);
	lt.push_back(7);


	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;

	//删除所有的偶数结点
	list<int>::iterator it = lt.begin();
	while (it != lt.end())
	{
		if (*it % 2 == 0)
		{
			it = lt.erase(it);
		}
		else
		{
			it++;
		}
	}

	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;

}
```
erase在释放之后，迭代器是肯定失效的，因为其所指向的空间都失效了。因此库里采用**返回值进行更新迭代器**。

## 2.3 链表元素排序:sort
在`<algorithm>`中`sort`只接受随机迭代器,但`list`的迭代器类型为双向迭代器,因此`<algorithm>`库中`sort(快排)`不能使用, 只能用自带的`sort(归并排序)`
>注:list的排序**很慢**,但需要将**大量数据排序(大概大于10万)**时,宁愿先**拷贝到vector排完再拷回来**.

## 2.4 合并两个有序链表:merge
前提**必须**是**有序**的

```c++
void test_list()
{
	list<int> lt;
	lt.push_back(2);
	lt.push_back(2);
	lt.push_back(3);
	lt.push_back(4);
	lt.push_back(5);
	lt.push_back(6);
	lt.push_back(7);


	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;

	list<int> lt1;
	lt1.push_back(1);
	lt1.push_back(3);
	lt1.push_back(5);
	lt1.push_back(7);
	lt1.push_back(9);

	for (auto e : lt1)
	{
		cout << e << " ";
	}
	cout << endl;

	//将lt1合并到lt上
	lt.merge(lt1);
	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;

}
```

## 2.5 对链表排序并去重:unique
unique去重的前提是**有序**。

```c++
void test_list()
{
	list<int> lt;
	lt.push_back(2);
	lt.push_back(1);
	lt.push_back(3);
	lt.push_back(4);
	lt.push_back(2);
	lt.push_back(6);
	lt.push_back(4);
	
	lt.sort();
	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;

	lt.unique();

	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;
}
```

## 2.6 删除指定元素:remove
相当于 `find`+`erase`

`remove` 会遍历整个`list`删除所有指定元素,当`list`中无指定元素时它会什么也不做

## 2.7 将链表的某部分转移到另一链表:splic
接口：
```C++
void splice (iterator position, list& x);
//将x链表转移到另一条链表的pos位置之前
void splice (iterator position, list& x, iterator i);
//将x链表的某个位置的结点移到另一条链表的pos之前
void splice (iterator position, list& x, iterator first, iterator last);
//将x链表的[first,last)的区间移到另一条链表的pos位置之前。
```
都是转移到指定位置**之前**  
注意：迭代器/迭代器区间不能重合！
```c++
void test_list()
{
list<int> lt;
	lt.push_back(2);
	lt.push_back(2);
	lt.push_back(3);
	lt.push_back(4);
	lt.push_back(5);
	lt.push_back(6);
	lt.push_back(7);


	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;

	list<int> lt1;
	lt1.push_back(1);
	lt1.push_back(3);
	lt1.push_back(5);
	lt1.push_back(7);
	lt1.push_back(9);

	for (auto e : lt1)
	{
		cout << e << " ";
	}
	cout << endl;

	//将lt1转移到到lt的前面
	lt.splice(lt.begin(),lt1);
	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;

	//将lt的后半部分转移到lt1上
	list<int>::iterator it = lt.begin();
	for (size_t i = 0; i < 5; i++)
	{
		it++;
	}
	lt1.splice(lt1.begin(), lt ,it, lt.end());

	for (auto e : lt1)
	{
		cout << e << " ";
	}
	cout << endl;

	//将lt1的第一个结点移到lt的最前面
	lt.splice(lt.begin(), lt1, lt1.begin());

	for (auto e : lt)
	{
		cout << e << " ";
	}
	cout << endl;
}
```
# 3.list的模拟实现
```c++
#pragma once
#include<assert.h>
#include<iostream>
using namespace std;

namespace ymz
{
	template<class T>
	struct list_node
	{
		list_node<T>* _next;
		list_node<T>* _prev;
		T _val;

		list_node(const T& val = T())
			:_next(nullptr)
			, _prev(nullptr)
			, _val(val)
		{ }
	};

	template<class T,class Ref,class Ptr>
	struct _list_iterator
	{
		typedef list_node<T> Node;
		typedef _list_iterator<T, Ref, Ptr> self;
		Node* _node;

		_list_iterator(Node* node)
			:_node(node)
		{ }

		Ref operator*()
		{
			return _node->_val;
		}

		Ptr operator->()
		{
			return &_node->_val;
		}
		//在实际使用过程中,如:it->_a1(_a1是结构体A的成员,it是迭代器)
		//语法是不正确的,应为it->->_a1,第1个返回T*(即A*),第2个取_a1
		//但运算符重载要求可读性,编译器进行特殊处理自动省略一个 ->

		self& operator++()
		{
			_node = _node->_next;
			return *this;
		}
		self& operator++(int)
		{
			_list_iterator<T> tmp(*this);
			_node = _node->_next;
			return tmp;
		}
		self& operator--()
		{
			_node = _node->_prev;
			return *this;
		}
		self& operator--(int)
		{
			_list_iterator<T> tmp(*this);
			_node = _node->_prev;
			return tmp;
		}

		//加const,如:it.end()的返回值是临时拷贝具有常性
		bool operator!=(const self& it) const
		{
			return _node != it._node;
		}

		bool operator==(const self& it) const
		{
			return _node == it._node;
		}
	};

	template<class T>
	class list
	{
		typedef list_node<T> Node;

	public:
		//typedef _list_iterator<T> iterator;
		//typedef const _list_iterator<T> const_iterator;
		//vector能用是因为它的iterator就是指针,iterator移动不会解引用(内容不改变,仅指向改变)
		//list不能用是因为它的iterator是结构体,iterator移动 本质是结构体内部成员的改变

		typedef _list_iterator<T,T&,T*> iterator;
		typedef _list_iterator<T,const T&,const T*> const_iterator;

		iterator begin()
		{
			//return iterator(_head->_next);
			//单参数的构造函数（或转换运算符）默认支持隐式类型转换
			return _head->_next;
		}
		iterator end()
		{
			return _head;
		}

		void empty_init()
		{
			_head = new Node;
			_head->_prev = _head;
			_head->_next = _head;
			_size = 0;
		}

		list()
		{
			empty_init();
		}
		list(const list<T>& lt)
		{
			empty_init();

			for (auto& e : lt)
			{
				push_back(e);
			}
		}
		void swap(list<T>& lt)
		{
			std::swap(_head, lt._head);
			std::swap(_size, lt._size);
		}
		//list& operator=(list& lt) 也可以
		list<T>& operator=(list<T>& lt)
		{
			swap(lt);
			return *this;
		}

		~list()
		{
			clear();

			delete _head;
			_head = nullptr;
		}

		void clear()
		{
			iterator it = begin();
			while (it != end())
			{
				it = erase(it);
			}
		}

		void push_back(const T& x)
		{
			insert(end(), x);
		}
		void push_front(const T& x)
		{
			insert(begin(), x);
		}
		void pop_back()
		{
			erase(--end());
		}
		void pop_front()
		{
			erase(begin());
		}

		size_t size()
		{
			/*size_t sz = 0;
			iterator it = begin();
			while (it != end()
			{
				++sz;
				++it;
			}
			return sz;*/
			return _size;
		}

		iterator insert(iterator pos, const T& x)
		{
			Node* cur = pos._node;
			Node* prev = cur->_prev;
			Node* newnode = new Node(x);

			prev->_next = newnode;
			newnode->_next = cur;

			cur->_prev = newnode;
			newnode->_prev = prev;
			++_size;
			return newnode;
		}

		iterator erase(iterator pos)
		{
			assert(pos != end());

			Node* cur = pos._node;
			Node* prev = cur->_prev;
			Node* next = cur->_next;
			prev->_next = next;
			next->_prev = prev;
			delete cur;
			--_size;
			return next;
		}
	private:
		Node* _head;
		size_t _size;
	};
}

```


