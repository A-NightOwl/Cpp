**本章重点**
1. 栈和队列
2. 双端队列
3. 优先级队列


# 1.栈和队列
## 1.1 .栈和队列的介绍
**stack**

>1. stack是一种**容器适配器**，专门用在**具有后进先出操作的上下文环境中**，其**删除只能从容器的一端进行元素的插入与提取操作。**
>2. stack是作为容器适配器被实现的，容器适配器即是对特定类封装作为其底层的容器，并提供一组特定的成员函数来访问其元素，将特定类作为其底层的，元素特定容器的尾部(即栈顶)被压入和弹出。
>3. stack的底层容器可以是任何标准的容器类模板或者一些其他特定的容器类，这些容器类应该支持以下操作：
>- empty：判空操作
>- back：获取尾部元素操作
>- push_back：尾部插入元素操作
>- pop_back：尾部删除元素操作
>
>4. 标准容器vector、deque、list均符合这些需求，默认情况下，如果没有为stack指定特定的底层容器，默认情况下使用deque。
>
>![请添加图片描述](https://i-blog.csdnimg.cn/direct/b58f1a89c39d491db6b7174e6f511478.png)

**queue**

>1. 队列是一种**容器适配器**，**专门用于在FIFO上下文(先进先出)中操作**，其中从容器一端插入元素，另一端提取元素。
>2. 队列作为容器适配器实现，容器适配器即将特定容器类封装作为其底层容器类，queue提供一组特定的成员函数来访问其元素。元素从队尾入队列，从队头出队列。
>3. 底层容器可以是标准容器类模板之一，也可以是其他专门设计的容器类。该底层容器应至少支持以下操作:
>- empty：检测队列是否为空
>- size：返回队列中有效元素的个数
>- front：返回队头元素的引用
>- back：返回队尾元素的引用
>- push_back：在队列尾部入队列
>- pop_front：在队列头部出队列
>
>4. 标准容器类deque和list满足了这些要求。默认情况下，如果没有为queue实例化指定容器类，则使用标准容器deque。
> 
>![请添加图片描述](https://i-blog.csdnimg.cn/direct/4fa363ab6db94c2a9e59d69126c810a1.png)

## 1.2 栈和队列练习

### 1.2.1 [最小栈](https://leetcode.cn/problems/min-stack/description/)
思路：

用两个栈来实现，一个栈存放正常的元素，一个栈存放之前每次入栈时栈内的最小元素


![请添加图片描述](https://i-blog.csdnimg.cn/direct/9c96496a43584d6e972fe07c7e9a90a1.png)


插入的时候，如果minst为空或者要插入的元素数据 小于等于 minst的top元素，则minst插入

删除的时候，如果minst的top元素等于st的top元素，则对minst进行pop

代码如下：
```C++
class MinStack {
public:
    MinStack() 
    {}
    
    void push(int val) 
    {
        _st.push(val);
        if(_mst.empty() || val<=_mst.top())
        {
            _mst.push(val);
        }
    }
    
    void pop() 
    {
        if(_st.top()==_mst.top())
        {
            _mst.pop();
        }
        _st.pop();
    }
    
    int top() 
    {
        return _st.top();
    }
    
    int getMin() 
    {
        return _mst.top();
    }
private:
    stack<int> _st;
    stack<int> _mst;
};
```

### 1.2.2 [栈的压入、弹出序列](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?tpId=13&&tqId=11174&rp=1&ru=/activity/oj&qru=/ta/coding-interviews/question-ranking)

思路：

**模拟入栈出栈的过程，**

1. 先进行入栈操作，如果和出栈序列的第popi个元素不同，就继续入

2. 如果相同，进行出栈操作，

3. 判断是最后popi是否与出栈序列的大小一致（由于每次出栈，popi都会++，如果能出完，则此时popi会等于popV.size()）

**代码实现如下：**
```C++
class Solution {
public:
    bool IsPopOrder(vector<int>& pushV, vector<int>& popV) 
    {
        stack<int> st;
        int pushi=0,popi=0;
      
        while(pushi < pushV.size())
        {
            st.push(pushV[pushi++]);
 
            while(!st.empty() && st.top() == popV[popi])
            {
                st.pop();
                popi++;
            }
        }
 
        return popi==popV.size();
    }
}
```

### 1.2.3 [逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)
中缀表达式: 2 + 1 * 3  
后缀表达式: 2 1 3 * +  
>中缀->后缀:
>1. 操作数输出(储存在容器中)
>2. 操作符入栈
>    - 如果栈为空 或 当前操作符比栈顶优先级高,继续入栈
>    - 栈不为空 且 当前操作符比栈顶优先级低或相等,输出操作符
>3. 表达式结束后,依次出栈里的操作符
> 
>例:
>中缀表达式: 2 + 1 - 3 * 4   
>后缀表达式: 2 1 + 3 4 * - 

思路:
1. 操作数入栈
2. 操作符,取栈顶元素两个进行运算,运算结果继续入栈

```c++
class Solution 
{
public:
    int evalRPN(vector<string>& tokens) 
    {
        stack<int> s;
        for (size_t i = 0; i < tokens.size(); ++i) 
        {
            string& str = tokens[i];
            // str为数字
            if (!("+" == str || "-" == str || "*" == str || "/" == str)) 
            {
                s.push(atoi(str.c_str()));
            } 
            else 
            {
                // str为操作符
                int right = s.top();
                s.pop();
                int left = s.top();
                s.pop();
                switch (str[0]) 
                {
                case '+':
                    s.push(left + right);
                    break;
                case '-':
                    s.push(left - right);
                    break;
                case '*':
                    s.push(left * right);
                    break;
                case '/':
                    // 题目说明了不存在除数为0的情况
                    s.push(left / right);
                    break;
                }
            }
        }
        return s.top();
    }
};
```
### 1.2.4 [用两个栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/description/)
自行实现

```c++
class MyQueue {
public:
    MyQueue() 
    { }
    
    void push(int x) 
    {
        pushST.push(x);
    }
    
    int pop() 
    {
        if(popST.empty())
        {
            while(!pushST.empty())
            {
                popST.push(pushST.top());
                pushST.pop();
            }
        }
        int tmp=popST.top();
        popST.pop();
        return tmp;
    }
    
    int peek() 
    {
        if(popST.empty())
        {
            while(!pushST.empty())
            {
                popST.push(pushST.top());
                pushST.pop();
            }
        }
        return popST.top();
    }
    
    bool empty() 
    {
        return popST.empty()&&pushST.empty();
    }
private:
    stack<int> pushST;
    stack<int> popST;
};
```
### 1.2.5 [二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/description/)
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        vector<vector<int>> vv;
        queue<TreeNode*> q;
        int levelSize = 0;

        if(root)
        {
            q.push(root);
            levelSize=1;
        }

        
        while(!q.empty())
        {
            //一层一层出
            vector<int> v;
            for(int i=0;i<levelSize;i++)
            {
                TreeNode* front=q.front();
                q.pop();

                v.push_back(front->val);
                
                if(front->left)
                    q.push(front->left);
                if(front->right)
                    q.push(front->right);
            }
            vv.push_back(v);
            levelSize = q.size();
        }
        return vv;
    }
};
```
### 1.2.6 [二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/description/)
```c++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) 
    {
        vector<vector<int>> vv;
        queue<TreeNode*> q;
        int levelSize = 0;

        if(root)
        {
            q.push(root);
            levelSize=1;
        }

        
        while(!q.empty())
        {
            //一层一层出
            vector<int> v;
            for(int i=0;i<levelSize;i++)
            {
                TreeNode* front=q.front();
                q.pop();

                v.push_back(front->val);
                
                if(front->left)
                    q.push(front->left);
                if(front->right)
                    q.push(front->right);
            }
            vv.push_back(v);
            levelSize = q.size();
        }
        reverse(vv.begin(),vv.end());
        return vv;
    }
};
```
## 1.3 栈和队列的模拟实现

**stack**
```c++
#pragma once
#include<vector>
#include<list>
#include<deque>
using namespace std;

namespace ymz
{
	//容器适配器
	template<class T,class Container = deque<T>>
	class stack
	{
	public:
		void push(const T& x)
		{
			_con.push_back(x);
		}

		void pop()
		{
			_con.pop_back();
		}

		T& top()
		{
			return _con.back();
		}

		size_t size()
		{
			return _con.size();
		}

		bool empty()
		{
			return _con.enpty();
		}
	private:
		Container _con;
	};
}
```
**queue**
```c++
#pragma once
#include<vector>
#include<list>
#include<deque>
using namespace std;

namespace ymz
{
	//容器适配器
	template<class T, class Container = deque<T>>
	class list
	{
	public:
		void push(const T& x)
		{
			_con.push_back(x);
		}

		void pop()
		{
			_con.pop_front();
		}

		T& front()
		{
			return _con.front();
		}
		T& back()
		{
			return _con.back();
		}

		size_t size()
		{
			return _con.size();
		}

		bool empty()
		{
			return _con.enpty();
		}
	private:
		Container _con;
	};
}
```

# 2.双端队列deque的简单介绍
## 2.1 deque的原理介绍
deque(双端队列)：是一种双开口的"连续"空间的数据结构，双开口的含义是：可以在头尾两端进行插入和删除操作，且时间复杂度为O(1)，与vector比较，头插效率高，不需要搬移元素；与list比较，空间利用率比较高。

**deque并不是真正连续的空间，而是由一段段连续的小空间拼接而成的，实际deque类似于一个动态的二维数组**，

**双端队列底层是一段假象的连续空间，实际是分段连续的，为了维护其“整体连续”以及随机访问的假象，落在了deque的迭代器身上**，因此deque的迭代器设计就比较复杂



![请添加图片描述](https://i-blog.csdnimg.cn/direct/460fe0e88f7949d6903d7d3146016028.png)

## 2.2 deque的缺陷
**与vector比较**，deque的优势是：头部插入和删除时，**不需要搬移元素，效率特别高**，而且在**扩容时，也不需要搬移大量的元素**，因此其效率是必vector高的。

**与list比较**，其底层是连续空间，**空间利用率比较高**，不需要存储额外字段。

但是，**deque有一个致命缺陷：不适合遍历**，因为在遍历时，deque的迭代器要频繁的去检测其是否移动到某段小空间的边界，导致效率低下，而序列式场景中，可能需要经常遍历，因此在实际中，需要线性结构时，大多数情况下优先考虑vector和list，deque的应用并不多，而目前看到的一个应用就是，STL用其作为stack和queue的底层数据结构。
## 2.3 为什么选择deque作为stack和queue的底层默认容器

stack是一种后进先出的特殊线性数据结构，因此只要具有push_back()和pop_back()操作的线性结构，都可以作为stack的底层容器，比如vector和list都可以；queue是先进先出的特殊线性数据结构，只要具有push_back和pop_front操作的线性结构，都可以作为queue的底层容器，比如list。但是STL中对stack和queue默认选择deque作为其底层容器，主要是因为：
1. stack和queue不需要遍历(因此stack和queue没有迭代器)，只需要在固定的一端或者两端进行操作。
2. 在stack中元素增长时，deque比vector的效率高(扩容时不需要搬移大量数据)；queue中的元素增长时，deque不仅效率高，而且内存使用率高。

结合了deque的优点，而完美的避开了其缺陷。
 
# 3.优先级队列

[优先级队列](https://legacy.cplusplus.com/reference/queue/priority_queue/?kw=priority_queue)

## 3.1 优先级队列的介绍
>1. 优先队列**是一种容器适配器**，根据严格的弱排序标准，**它的第一个元素总是它所包含的元素中最大的**。
>2. 此上下文**类似于堆**，在堆中可以随时插入元素，**并且只能检索最大堆元素**(优先队列中位于顶部的元素)。
>3. 优先队列被实现为容器适配器，容器适配器即将特定容器类封装作为其底层容器类，queue提供一组特定的成员函数来访问其元素。元素从特定容器的“尾部”弹出，其称为优先队列的顶部。
>4. 底层容器可以是任何标准容器类模板，也可以是其他特定设计的容器类。容器应该可以通过随机访问迭代器访问，并支持以下操作：
>- empty()：检测容器是否为空
>- size()：返回容器中有效元素个数
>- front()：返回容器中第一个元素的引用
>- push_back()：在容器尾部插入元素
>- pop_back()：删除容器尾部元素
>5. 标准容器类vector和deque满足这些需求。**默认情况下，如果没有为特定的priority_queue类实例化指定容器类，则使用vector。**
>6. 需要支持随机访问迭代器，以便始终在内部保持堆结构。容器适配器通过在需要时自动调用算法函数make_heap、push_heap和pop_heap来自动完成此操作。
## 3.2 priority_queue的函数接口

![请添加图片描述](https://i-blog.csdnimg.cn/direct/a2304bd17f684d74a85ff214f1b9f88e.png)  
**Test1**
```c++
#include <iostream>
#include <queue>
using namespace std;
 
void trxt_priority_queue()
{
    // 创建优先级队列
    // 默认是一个大堆，大的优先级高
	priority_queue<int> pq;
 
    // 插入元素
	pq.push(1);
	pq.push(0);
	pq.push(6);
	pq.push(9);
	pq.push(2);
	pq.push(3);
	pq.push(3);
 
	cout << "size：" << pq.size() << endl;	// 查看该队列的元素大小
	cout << "top：" << pq.top() << endl;	// 查看该队列的首元素
 
	while (!pq.empty())// 判空
	{
		cout << pq.top() << " ";
		pq.pop(); // 删除元素
	}
 
	cout << endl;
 }
int main()
{
	trxt_priority_queue();
	return 0;
}
```
**Test2**
```c++
void min()
{
	//用仿函数，也叫函数对象
	//变小堆，使其小的优先级高
	priority_queue<int,vector<int>,greater<int>> pq;
	pq.push(1);
	pq.push(0);
	pq.push(6);
	pq.push(9);
	pq.push(2);
	pq.push(3);
	pq.push(3);
 
	cout << "size：" << pq.size() << endl;	// 查看该队列的元素大小
	cout << "top：" << pq.top() << endl;	// 查看该队列的首元素
 
	while (!pq.empty())
	{
		cout << pq.top() << " ";
		pq.pop();
	}
 
	cout << endl;
}
 
int main()
{
	//trxt_priority_queue();
	min();
 
	return 0;
}
```

## 3.3 仿函数
简单理解就是在类里面使用了一个括号的重载运算符，使得类对象可以像函数一样被访问
```c++
//仿函数/函数对象
//这个类对象可以像函数一样使用
class Less
{
public:
	bool operator()(int x, int y)
	{
		return x < y;
	}
};
```
```c++
Less lessfunc;
cout << lessfunc(1, 2) << endl;
cout << lessfunc.operator(1, 2) << endl;
```
## 3.4 反向迭代器
头文件
```c++
template<class Iterator,class Ref,class Ptr>
struct ReverseIterator
{
	typedef ReverseIterator<Iterator, Ref, Ptr> Self
		Iterator _it;
	ReverseIterator(Iterator it)
		:_it(it)
	{
	}
	Ref operator*()
	{
		Iterator tmp = _it;
		return *(--tmp);
	}
	Ptr operator->()
	{
		return &(operator*());
	}
	Self& operator++()
	{
		--it;
		return *this;
	}
	Self& operator--()
	{
		++it;
		return *this;
	}
	bool operator!=(const Self& s)const
	{
		return _it != s._it;
	}
};
```
另一个

```c++
typedef ReverseIterator<iterator,T&,T*> reverse_iterator
typedef ReverseIterator<iterator,const T&,const T*> const reverse_iterator
reverse_iterator rbegin()
{
  return reverse_iterator(end());
}
reverse_iterator rend()
{
  return reverse_iterator(begin());
}

```

## 3.5 优先级队列的模拟实现
```c++
#pragma once
#include<vector>
using namespace std;

namespace ymz
{
	template<class T,class Container = vector<T>,class Compare = less<T>>
	class priority_queue
	{
	private:
		void AdjustDown(int parent)
		{
			Compare com;

			//找左右孩子中较大的
			int child = parent * 2 = 1;

			while (child < _con.size())
			{
				if (child + 1 < _con.size() && com(_con[child] , _con[child + 1]))
				{
					child++;
				}
				if (com(_con[parent], _con[child]))
				{
					swap(_con[child], _con[parent]);
					parent = child;
					child = parent * 2 + 1;
				}
				else
				{
					break;
				}
			}
		}

		void AdjustUp(int child)
		{
			int parent = (child - 1) / 2;
			while (child > 0)
			{
				if (com(_con[parent] , _con[child]))
				{
					swap(_con[child], _con[parent]);
					child = parent;
					parent = (child - 1) / 2;
				}
				else
				{
					break;
				}
			}
		}
	public:
		priority_queue()
		{ }
		template<class InputIterator>
		priority_queue(InputIterator first, InputIterator last)
		{
			while (first != last)
			{
				_con.push_bach(*first);
				++first;
			}
			//建堆
			for (int i = (_con.size() - 2) / 2; i >= 0; i--)
			{
				AdjustDown(i);
			}
		}

		void pop()
		{
			swap(_con[0], _con[_con.size() - 1]);
			_con.pop_back();
			AdjustDown(0);
		}

		void push(const T& x)
		{
			_con.push_back(x);
			AdjustUp(_con.size());
		}

		const T& top()
		{
			return _con[0];
		}

		bool empty()
		{
			return _con.empty();
		}

		size_t size()
		{
			return _con.size();
		}
	private:
		Container _con;
	};
}
```