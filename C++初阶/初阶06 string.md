**本章重点**
1. 为什么要学习string类
2. 标准库中的string类
3. string类的模拟实现
4. 扩展阅读

# 1.为什么要学习string类
**C语言中的字符串**

>C语言中，字符串是以'\0'结尾的一些字符的集合，为了操作方便，C标准库中提供了一些str系列的库函数，但是这些库函数与字符串是分离开的，不太符合OOP的思想，而且底层空间需要用户自己管理，稍不留神可能还会越界访问。

>在OJ中，有关字符串的题目基本以string类的形式出现，而且在常规工作中，为了简单、方便、快捷，基本都使用string类，很少有人去使用C库中的字符串操作函数。

我们下面的学习，都会以文档内容为标准

**string类 就类比数据结构的  字符类型的顺序表  来学习能更好的理解**

# 2.标准库中的string类
1. 字符串是表示字符序列的类
   
2. 标准的字符串类提供了对此类对象的支持，其接口类似于标准字符容器的接口，但添加了专门用于操作单字节字符字符串的设计特性。
3. string类是使用char(即作为它的字符类型，使用它的默认char_traits和分配器类型(关于模板的更多信息，请参阅basic_string)。
4. string类是basic_string模板类的一个实例，它使用char来实例化basic_string模板类，并用char_traits和allocator作为basic_string的默认参数(根于更多的模板信息请参考basic_string)。
5. 注意，这个类独立于所使用的编码来处理字节:如果用来处理多字节或变长字符(如UTF-8)的序列，这个类的所有成员(如长度或大小)以及它的迭代器，将仍然按照字节(而不是实际编码的字符)来操作。

**基本使用**  

库里面的类——那就要包含头文件 `#include<string>`

要使用库里的类：

1. 展开命名空间 `using namespace::std;`

2. 或者指定作用域 `std::string A;`

## 2.1 常见构造
![06.01.png]( http://154.37.212.149:9999/pic/home/bl/img/U1/C++初阶/06.01.png)

> 1. **无参构造**  
> 构造函数的string类对象，即空字符串
> ```C++
> string();
>  
> int main()
> {
> 	string s1;
> 	
> 	return 0;
> }
> ```

> 2. 带参构造  
> 用常量字符串来构造string对象
> ```C++
> string (const char* s);
>  
> int main()
> {
> 	string s2("hello world");
>  
> 	return 0;
> }
> ```

> 3. 拷贝构造  
> 拿一个已经存在的对象去初始化另一个未存在的对象（加引用是为了减少拷贝）
> ```C++
> string(const string& str);
>  
> int main()
> {
> 	string s2("hello world");
>  
> 	string s3(s2);
>  
> 	return 0;
> }
> ```

> 4. 用n字符的初始化
> string 类对象内会有n个字符#
> ```C++
> string(size_t n, char c);
>  
> int main()
> {
> 	// 用5个#去初始化s4对象
> 	string s4(5, '#'); //#####
>  
> 	return 0;
> }
> ```

> 5. 用字符串的前n个字符去初始化
> ```C++
> string(const char* s, size_t n);
>  
> int main()
> {
> 	// 用字符串的前5个字符去初始化s5对象
> 	string s5("hello world", 5);
>  
> 	cout << s5 << endl; //hello
>  
> 	return 0;
> }
> ```
> string存放的是前5个字符



> 6. 从一个 string 对象的 pos 为值开始，拿 len 个字符去初始化   
> string(const string& str, size_t pos, size_t len = npos);
> ```C++
> int main()
> {
> 	string s6("STL_is_so_hard");
>  
> 	// 从s6对象的第0个位置开始（包括第0个位置的字符），往后数7个字符，去初始化s7对象
> 	string s7(s6, 0, 7);
> 	string s8(s6, 0);
> 	cout << s7 << endl;//STL_is_
> 	string s8(s6, 0);  //STL_is_so_hard
> 	return 0;
> }
> ```
> 
> 可以看到，len 里面给了个缺省值 nops，那么这个 nops 是多少？
> 
> 查看文档可以看到：  
> npos 其实是 string 类里面的一个静态成员变量，给的值是 -1
> 
> 但是 size_t 是用无符号来修饰的，也就是说无符号的 -1，它的补码全是1，也就是整形的最大值，所以他的意思就是：从 pos 位置开始，往后取 42 亿多个字符。
> 
> 也就是代表着如果不给 len 值，那么就是有多少取多少

## 2.2 容量接口
函数名称|	功能说明
|---|---|
[size](https://cplusplus.com/reference/string/string/size/)（重点）|	返回字符串有效字符长度
[length](https://cplusplus.com/reference/string/string/length/)|	返回字符串有效字符长度
[capacity](https://cplusplus.com/reference/string/string/capacity/)	|返回空间总大小
[empty](https://cplusplus.com/reference/string/string/empty/)(重点)	|检测字符串释放为空串，是返回true，否则返回false
[clear](https://cplusplus.com/reference/string/string/clear/)（重点）|	清空有效字符
[reserve](https://cplusplus.com/reference/string/string/reserve/)（重点）|	为字符串预留空间**
[resize](https://cplusplus.com/reference/string/string/resize/)（重点）|	将有效字符的个数该成n个，多出的空间用字符c填充

> ### 2.2.1 **size** 与 **length**
>
> 功能: 返回有效字符串(有效字符就是求除了\0的字符)有效长度
> 
> 函数
> ```c++
> size_t size() const;
> size_t length() const;
> ```
> 举例:
> ```c++
> int main()
> {
> 	string s1("hello world");
>  
> 	cout << s1.size() << endl;
> 	cout << s2.length() << endl;
> 	return 0;
> }
> ```
> size 与 length 方法底层实现原理完全相同，引入 size 的原因是为了与其他容器的接口保持一致，所以一般情况下基本都是用 size

>### 2.2.2 **max_size**
>
>功能：字符串能够达到的最大长度。
>   
>函数：
>```C++
>size_t max_size() const;
>```
>
>举例：
>```C++
> #include <iostream>
> #include <string>
> int main()
> {
> 	string A;
> 	cout<<A.max_size()<<endl;
> 	return 0;
> }
>```
>这个实际上就是告诉你，字符串最大能开多长，但是这个函数在实际中并没有什么太大的用处。大家可以试试运行下看看最大能开多大

> ### 2.2.3 **empty**
> 
> 功能：检测string是否为空，为空返回真，其余情况返回假。
> 
> 函数：
> ```C++
> bool empty() const;
> //注：加了const,所以使用empty是不会对string类进行修改的。
> ```
> 
> 案例：
> ```C++
> #include<iostream>
> #include<string>
> using namespace::std;
> int main()
> {
> 	string A("\0");
> 	if (A.empty())
> 	{
> 		printf("YES\n");
> 	}
> 	else
> 	{
> 		printf("NO\n");
> 	}
> 	return 0;
> }
> ```

> ### 2.2.4 **clear**
> 
> 功能：减少string中的字符，使之成为空字符串，也就是字符串的长度为0。
> 
> 函数：
> ```C++
> void clear();
> ```
> 
> 
> 案例：
> ```C++
> #include<iostream>
> #include<string>
> using namespace::std;
> int main()
> {
> 	string A("hello world");
> 	A.clear();
> 	if (A.empty())
> 	{
> 		printf("YES\n");
> 	}
> 	else
> 	{
> 		printf("NO\n");
> 	}
> 	return 0;
> }
> ```

> ### 2.2.5 **capacity**
>    
> 功能： 返回分配的存储空间容量大小
>
> 函数:
> ```C++
> size_t capacity() const;
>```
> 举例：
> ```C++
> #include<iostream>
> #include<string>
> using namespace::std;
> int main()
> {
> 	string A("hello");
> 	cout << A.capacity() << endl;
> 	return 0;
> }
> ```
>![06.02.png##w800##]( http://154.37.212.149:9999/pic/home/bl/img/U1/C++初阶/06.02.png)
>第一次扩容是2倍扩容（算上\0），之后的扩容接近1.5倍扩容。（VS2019）  
>而在Linux平台上进行测试：扩容是一直以两倍进行扩容的,
>
> 这是因为他们版本不同。VS是PJ版，它是微软进行维护的；Linux下的g++是开源社区维护的。也正是STL并没有明确的扩容机制，所以每个社区或平台上的是不一样的。
> 
>另外**扩容也是有代价的**，需要开新的空间，那么有什么方法能够减少扩容呢？这就需要我们的 **reserve 函数**


> ### 2.2.6 **reserve**
>    
> 功能：为字符串预留n空间，当n大于capacity时进行扩容，其余情况按照原先的capacity。  
> 结果：使string的capacity大于等于n。
> 
> 当n小于capacity时，这是一个不具约束力的请求，因此容器的执行操作是依据实际编译器进行的优化并且留下一个capacity大于等于n的结果。
> 一般来说，编译器觉得字符串的空间远小于capacity时可能会进行缩容。
> 
> 函数：
> ```C++
> void reserve (size_t n = 0);
> ```
> 案例:
> ```C++
> #include <iostream>
> #include <string>
> int main()
> {
> 	string A("hello world");
> 	A.reserve(20);
> 	cout <<A<< endl;
> 	return 0;
> }
> ```

> ### 2.2.7 **resize**
> 
> 功能：开空间 + 赋值
> 
> 当n小于size时，会将多于n的字符全部删除，对于少的则不做处理。  
> 当n大于size时，如果没有第二个参数，会将大于size的部分填充为\0,如果有第二个参数，则会将大于size的部分填充为第二个参数的值。
> 
> 函数：
> ```C++
> void resize (size_t n);
> void resize (size_t n, char c);
> //这两个是函数重载
> ```
> 
>> 例1：小于size
>> ```C++
>> #include <iostream>
>> #include <string>
>> int main()
>> {
>> 	string A("hello world");
>> 	A.resize(1,'x');//这里的x其实没啥用
>> 	cout << A << endl;
>> 	return 0;
>> }
>> ```
>> 结果：
>>```c++
>>h
>>```
>
>> 例2：大于size
>> ```C++
>> #include <iostream>
>> #include <string>
>> int main()
>> {
>> 	string A("hello world");
>> 	A.resize(20,'x');
>> 	cout << A << endl;
>> 	return 0;
>> }
>> ```
>> 结果:
>>```c++
>>hello worldxxxxxxxxx
>>```


## 2.3 访问,修改及遍历操作

>### 2.3.1 **+=**
>   
>函数：
>```C++
>string& operator+= (const string& str);
>string& operator+= (const char* s);
>string& operator+= (char c);
>```
>功能：+=一个字符串/字符/string类
>
>例：
>```C++
> #include <iostream>
> #include <string>
> 
> int main()
> {
> 	string A;
> 	A += "hello";
> 	A += ' ';
> 	string B("world");
> 	A += B;
> 	cout << A << endl;
> 	return 0;
> }
>```

>### 2.3.2 **[]** 与 **at**
>   
>> **[]**
>> 
>> 函数：
>> ```C++
>>  char& operator[] (size_t pos);
>>  const char& operator[] (size_t pos) const;
>> ```
>> 功能：访问字符串下标位置的字符。
>>
>> 例：
>> ```C++
>> #include <iostream>
>> #include <string>
>> int main()
>> {
>> 	string A("hello");
>> 	char a = A[0];
>> 	cout << a << endl;
>> 	return 0;
>> }
>> ```
>>说明：[ ]越界会进行报错（assert）而at越界则会进行抛异常。
>
>> at  
>> at 和 operator[ ]  很相似，区别是：当他们出错时编译器会有不同的警告
>> ```C++
>> int main()
>> {
>> 	string s1 = "hello world";
>>  
>> 	s1[100];
>> 	s1.at(100);
>>  
>> 	return 0;
>> }
>> ```
>> operator[ ]：就是 assert（） 那种直接中断的报错警告
>> ![06.03.png##w600##]( http://154.37.212.149:9999/pic/home/bl/img/U1/C++初阶/06.03.png)
>> at ：at是抛异常的警告
>> ![06.04.png##w600##]( http://154.37.212.149:9999/pic/home/bl/img/U1/C++初阶/06.04.png)
>> 
>> 抛异常是能被 捕获 到的：
>> ```C++
>> int main()
>> {
>> 	string s1 = "hello world";
>>  
>> 	try
>> 	{
>> 		s1.at(100);
>> 	}
>> 	catch(const exception& a)
>> 	{
>> 		cout << a.what() << endl;
>> 	}
>>  
>> 	return 0;
>> }
>> ```
>> 实际过程中很少用at




>### 2.3.3 **push_back**
>   
>函数
>```C++
>void push_back (char c);
>```
>功能：再string末尾后追加一个字符
>
>例:
>```C++
> #include <iostream>
> #include <string>
> int main()
> {
> 	string A("hello");
> 	A.push_back('x');
> 	cout << A << endl;
> 	return 0;
> }
>```

>### 2.3.4 **append**
>
> 函数：
> ```C++
> string& append (const string& str);
> 
> string& append (const string& str, size_t subpos, size_t sublen);
> 
> string& append (const char* s);
> 	
> string& append (const char* s, size_t n);
> 
> string& append (size_t n, char c);
> ```
> **功能**：在指定string后面追加 n个字符/(前n个字符的)字符串/string(从subpos位置开始的,sublen长度)。
> 
> 例：
> ```c++
> #include <iostream>
> #include <string>
> int main()
> {
> 	string A("hello world");
> 	string B(" hello");
> 	A.append(B,0,6);
> 	cout << A << endl;
> 	A.append(B);
> 	cout << A << endl;
> 	A.append(" hello", 0, 6);
> 	cout << A << endl;
> 	A.append(6, 'x');
> 	cout << A << endl;
> 	return 0;
> }
> ```

>### 2.3.5 **范围for**
>  
>范围for 的底层实现就是通过迭代器实现的。
>
>如果我们是通过范围 for 来修改对象的元素，那么接收元素的变量 e 的类型必须是引用类型，否则 e 只是对象元素的拷贝，对 e 的修改不会影响到对象的元素。
>```C++
> void TestString() 
> {
> 	string s1("hello world!");
>  
> 	//使用范围for访问对象元素
> 	for (auto e : s1) 
> 	{
> 		cout << e;
> 	}
> 	cout << endl;
>  
> 	//使用范围for修改对象元素
> 	for (auto& e : s1)
> 	{
> 		e = 'x';
> 	}
> 	cout << s1 << endl;
> 
> }
>```

>### 2.3.6 **insert**
>函数:
>
>```c++
>insert (size_t pos, const string& str);	
>insert (size_t pos, const string& str, size_t subpos, size_t sublen);
>insert (size_t pos, const char* s);
>insert (size_t pos, const char* s, size_t n);
>string& insert (size_t pos, size_t n, char c);
>iterator insert (iterator p, char c);
>```
> 可以看到insert有很多结构，但并不是每个都会用到，这里我们选几个用的最多的来演示
>
>> 1）在 pos 位置插入一个常量字符串
>> ```C++
>> //string& insert(size_t pos, const string& str);
>>  
>> int main()
>> {
>> 	string s("hello");
>>  
>> 	// 在第3个位置插入字符串xxx
>> 	s.insert(3, "xxx");  
>>  
>> 	cout << s << endl;  // helxxxlo 
>>  
>> 	return 0;
>> }
>> ```
> 
>> 2）在 pos 位置插入 n 个字符串 c
>> 
>>  
>> ```C++
>> //string& insert(size_t pos, size_t n, char c);
>>  
>> int main()
>> {
>> 	string s("hello");
>>  
>> 	// 在第3个位置插入5个字符y
>> 	s.insert(3, 5, 'y');
>>  
>> 	cout << s << endl; //helyyyyylo
>>  
>> 	return 0;
>> }
>> ```
> 
>> 3）使用迭代器来进行插入
>> 
>>  
>> ```C++
>> //iterator insert(iterator p, char c);
>>  
>> int main()
>> {
>> 	string s("hello");
>>  
>> 	// 从begin开始的5个位置插入字符y
>> 	s.insert(s.begin() + 5, 'y');
>>  
>> 	cout << s << endl; //helloy
>>  
>> 	return 0;
>> }
>> ```
>> begin()，是s对象的第一个位置的指针

>### 2.3.7 **erase**
>功能:
>
>函数:
>
>
>```c++
>string& erase (size_t pos = 0, size_t len = npos);
>iterator erase (iterator p);
>iterator erase (iterator first, iterator last);
>```
>> 1）从 pos 位置开始，删除 len 个字符
>> 
>> ```C++
>> //string& erase(size_t pos = 0, size_t len = npos);
>>  
>> int main()
>> {
>> 	string s1("helloworld");
>>  
>> 	从s1下标为5的位置开始，往后删除2个字符
>> 	s1.erase(5, 2); // 删除 w 和 o
>>  
>> 	cout << s1 << endl; //hellorld
>>  
>>  
>> 	return 0;
>> }
>> ```
>注意：len 参数的缺省值是 npos，不给值会将后面的全删掉，删到' /0 ' 
>
>2）从迭代器的位置删除字符
>
> 
>>```C++
>> //iterator erase(iterator p);
>>  
>> int main()
>> {
>> 	string s1("helloworld");
>>  
>> 	//删除s1字符串第一个位置的元素
>> 	s1.erase(s1.begin());
>>  
>> 	cout << s1 << endl; //elloworld
>>  
>> 	return 0;
>> }
>>```
>
>> 3）从迭代器开始的位置，一直删除到迭代器结束的位置
>> 
>>  
>> ```C++
>> //iterator erase(iterator first, iterator last);
>>  
>> int main()
>> {
>> 	string s1("helloworld");
>>  
>> 	//从begin+5的位置开始，一直删除到end的位置
>> 	s1.erase(s1.begin() + 5, s1.end());
>>  
>> 	cout << s1 << endl; // hello
>>  
>> 	return 0;
>> }
>> ```

>### 2.3.8 **replace**
>功能:替换字符串的一部分
>
>函数:replace的函数有很多,但只要求掌握这两个
>```c++
>string& replace (size_t pos,  size_t len,  const char* s);
>string& replace (size_t pos,  size_t len,  size_t n, char c);
>```
>
>>  1）从 pos 位置开始的第 len 个字符替换为一个字符串
>>  
>> ```C++
>> string& replace(size_t pos, size_t len, const char* s);
>>  
>> int main()
>> {
>> 	string s1("helloworld");
>>  
>> 	//将第6个位置开始的4个字符替换为字符串xxxyyy
>> 	s1.replace(6, 4, "xxxyyy");
>>  
>> 	cout << s1 << endl; //hellowxxxyyy
>>  
>> 	return 0;
>> }
>> ```
>
>>  2）从 pos 位置开始的第 len 个字符替换为 n 个字符 c
>> ```C++
>> string& replace(size_t pos, size_t len, size_t n, char c);
>>  
>> int main()
>> {
>> 	string s1("helloworld");
>>  
>> 	//将第5个位置开始的1个字符替换为3个字符x
>> 	s1.replace(5, 1, 3, 'x');
>>  
>> 	cout << s1 << endl; //helloxxxorld
>>  
>> 	return 0;
>> 
>> }
>> ```

>### 2.3.9 **c_str**
>功能：返回一个字符串，类型是const char *。
>
>函数：
>```C++
>const char* c_str() const;
>```
>
>例：
>```C++
>#include <iostream>
>#include <string>
>int main()
>{
>	string A("hello");
>	const char* a = A.c_str();
>	cout << a << endl;
>	return 0;
>}
>```

>### 2.3.10 **find**
>
>功能：在string类的指定pos下标位置开始，**向后查找**目标【string/字符串(指定查找前n个)/字符】——的字符c位置，并返回其位置。
注意：查找的是（字符串，string里面的）字符。
>
> 函数：
> ```C++
> size_t find (const string& str, size_t pos = 0) const;
> size_t find (const char* s, size_t pos = 0) const;
> size_t find (const char* s, size_t pos, size_t n) const;
> size_t find (char c, size_t pos = 0) const;
> //pos是要查找string的下标，n是要在指定字符串中查找的长度。
> //size_t，如果成功返回一个匹配成功的下标，如果失败返回-1(转换为无符号整形)。
> ```
> 
> 例：
> ```C++
> #include <iostream>
> #include <string>
> int main()
> {
> 	string A("hello world");
> 	string B("world");
> 	cout << A.find(B, 0)<<endl;
> 	cout << A.find("world", 0) <<endl;//在A中找'w'
> 	cout << A.find("world", 0, 5) <<endl;//在A中找"world"
> 	cout << A.find('w', 5) <<endl;//从A第5个位置中找'w'
> 	return 0;
> 	//都是6
> }
> ```

>### 2.3.11 **rfind**
>功能：在string类的指定pos下标位置开始，**向前查找**目标【string/字符串(指定查找前n个)/字符】——的字符c位置，并返回其位置。
>
>函数：
>
>```C++
>size_t rfind (const string& str, size_t pos = npos) const;
>size_t rfind (const char* s, size_t pos = npos) const;
>size_t rfind (const char* s, size_t pos, size_t n) const;
>size_t rfind (char c, size_t pos = npos) const;
>```
>原理：同find
>
>
>例：
> ```C++
> #include <iostream>
> #include <string>
> int main()
> {
> 	string A("hello world");
> 	string B("world");
> 	cout << A.rfind(B, 5)<<endl;
> 	cout << A.rfind("world", 10) <<endl;
> 	cout << A.rfind("world", 10, 5) <<endl;
> 	cout << A.rfind('w', 10) <<endl;
> 	return 0;
> }
> ```

>### 2.3.12 **find_first_of**
> 功能：在string类中的pos位置开始查找，与字符串的前n个位置相关的任何一个字符。
> 
> 函数:
> 
> ```C++
> size_t find_first_of (const string& str, size_t pos = 0) const;
> size_t find_first_of (const char* s, size_t pos = 0) const;
> size_t find_first_of (const char* s, size_t pos, size_t n) const;
> size_t find_first_of (char c, size_t pos = 0) const;
> ```
> 
> 例：
> ```C++
> #include <iostream>       // std::cout
> #include <string>         // std::string
> #include <cstddef>        // std::size_t
> 
> int main ()
> {
>   std::string str ("Please, replace the vowels in this sentence by asterisks.");
>   std::size_t found = str.find_first_of("aeiou");
>   while (found!=std::string::npos)
>   {
>     str[found]='*';
>     found=str.find_first_of("aeiou",found+1);
>   }
> 
>   std::cout << str << '\n';
> 
>   return 0;
> }
> ```
>类似的还有find_last_of从后往前  
>find_first_not_of 找不是指定字符串中的  
>find_last_of

## 2.4 迭代器——iterator

1. **基本概念**
>迭代器的作用是用来访问容器（用来保存元素的数据结构）中的元素，所以使用迭代器，我们就可以访问容器中里面的元素。

2. **begin与end**
函数：
```C++
      reverse_iterator rbegin();
const_reverse_iterator rbegin() const;
```
功能：获取string的开头位置。

函数：
```C++
 iterator end();
 const_iterator end() const;
```
功能：获取string最后一个有效字符的位置的下一个位置通常为\0。

例：
```C++
#include <iostream>
#include <string>
int main()
{
	string A("hello world");
	string::iterator it = A.begin();
	//当A是const修饰的类时，我们就要用string::const_iterator迭代器
	while (it != A.end())
	{
		cout << *it << " " ;
		it++;
	}
	return 0;
}
```

3. **rbegin和rend**
函数：
```C++
      reverse_iterator rbegin();
const_reverse_iterator rbegin() const;
```
功能：获取最后一个有效字符的位置的。
函数：
```C++
      reverse_iterator rend();
const_reverse_iterator rend() const;
```
功能：获取第一个字符的位置的前一个位置。

例：
```C++
#include <iostream>
#include <string>
int main()
{
	string A("hello world");
	string::reverse_iterator it = A.rbegin();
	while (it != A.rend())
	{
		cout << *it << " " ;
		it++;
	}
	return 0;
}
```
说明：这里的rbegin + 1是倒着加1

**作用**  
迭代器是所有容器通用的  
且能适应所有算法


## 2.5 非成员函数
1. **getline**

函数:
```C++
istream& getline (istream& is, string& str, char delim);
istream& getline (istream& is, string& str);
```
功能:
通过流，对字符串进行输入/输出，输入/输出遇到delim终止字符结束，如果不写，默认是\n。这就解决了我们输入遇到空格的问题。

对于scanf读取一行可以

```C++
scanf("%[^\n]",str);//意为读取到\n结束。
```

对于gets

```C++
gets(str);
```

2. **stoi——字符串转化系列** 

**说明**：这个系列就介绍这一个，其它的可自行了解。

**功能**：
将string 的数字字符以base进制（默认为10进制）转换为整形。

**函数**：
```C++
int stoi (const string&  str, size_t* idx = 0, int base = 10);
int stoi (const wstring& str, size_t* idx = 0, int base = 10);
```
>**参数说明：**
>
>str：要转换的字符串。
>
>idx（可选）：一个指针，用于存储转换结束的位置（即第一个非数字字符的位置）。如果不需要，可以忽略或传递 nullptr。
>
>base（可选）：进制基数，默认为 10（十进制）。可以是 2 到 36 之间的值。
>
>**返回值：**
>返回转换后的整数（int 类型）。

例：

```C++
#include <iostream>   // std::cout
#include <string>     // std::string, std::stoi

int main()
{
	std::string str_dec = "2001, A Space Odyssey";
	std::string str_hex = "40c3";
	std::string str_bin = "-10010110001";
	std::string str_auto = "0x7f";

	std::string::size_type sz;   // alias of size_t

	int i_dec = std::stoi(str_dec, &sz);
	int i_hex = std::stoi(str_hex, nullptr, 16);
	//不使用第二个参数传入空指针即可。
	int i_bin = std::stoi(str_bin, nullptr, 2);
	int i_auto = std::stoi(str_auto, nullptr, 0);
	std::cout << str_dec << ": " << i_dec << "\n";
	std::cout << str_hex << ": " << i_hex << '\n';
	std::cout << str_bin << ": " << i_bin << '\n';
	std::cout << str_auto << ": " << i_auto << '\n';
	return 0;
}
```

# 3.string的模拟实现(源码)

```c++
namespace ymz
{
	class string
	{
	public:
		typedef char* iterator;
		typedef const char* const_iterator;
		iterator begin()
		{
			return _str;
		}
		iterator end()
		{
			return _str + _size;
		}
		const_iterator begin()const
		{
			return _str;
		}
		const_iterator end()const
		{
			return _str + _size;
		}

		string(const char* str="")
			: _size(strlen(str))
			, _capacity(_size)
			, _str(new char[_capacity + 1])
		{ 
			memcpy(_str, str, _size + 1);
		}
		/*string(const string& s)
		{
			_str = new char[s._capacity + 1];
			memcpy(_str, s._str, s._size + 1);
			_size = s._size;
			_capacity = s._capacity;
		}*/
		//拷贝构造要初始化
		//理由(问AI的不确定是否准确,但拷贝构造最好初始化):
		//显式调用拷贝构造函数（如 ymz::string s2(s1);）：
		//编译器会确保所有成员变量被正确初始化，即使拷贝构造函数没有显式初始化它们。
		//被动调用拷贝构造函数（如 s2 = s1; ）：
		// 临时对象的初始化可能会被优化或省略，其成员变量可能会保留未定义的值。
		string(const string& s)
			: _size(0)
			, _capacity(0)
			, _str(nullptr)
		{
			string tmp(s._str);
			//这(初始化使用strlen)可能导致拷贝的不准确
			swap(tmp);
		}
		//传统写法
		/*string& operator=(const string& s)
		{
			if (this != &s)
			{
				char* tmp = new char[s._capacity + 1];
				memcpy(tmp, s._str, s._size + 1);
				delete[] _str;
				_str = tmp;
				_capacity = s._capacity;
				_size = s._size;
			}
			
			return *this;
		}*/
		//现代写法
		void swap(string& s)
		{
			std::swap(_str, s._str);
			std::swap(_size, s._size);
			std::swap(_capacity, s._capacity);
		}
		string& operator=(string tmp)
		{
			//this->swap(tmp);
			swap(tmp);

			return *this;
		}

		~string()
		{
			delete[] _str;
			_str = nullptr;
			_size = _capacity = 0;
		}

		const char* c_str() const
		{
			return _str;
		}
		size_t size()const
		{
			return _size;
		}
		char& operator[](size_t pos)
		{
			assert(pos<_size);

			return _str[pos];
		}

		const char& operator[](size_t pos) const
		{
			assert(pos < _size);
			
			return _str[pos];
		}

		string& operator+=(char ch)
		{
			push_back(ch);
			return *this;
		}

		string& operator+=(const char* str)
		{
			append(str);
			return *this;
		}

		bool operator<(const string& s)const
		{
			//int ret = memcmp(_str, s._str, _size < s._size ? _size : s._size);
			//return ret == 0 ? _size < s._size : ret < 0;
			size_t i1=0;
			size_t i2=0;
			while (i1 < _size && i2 < s._size)
			{
				if (_str[i1] < s._str[i2])
				{
					return true;
				}
				else if (_str[i1] > s._str[i2])
				{
					return false;
				}
				else
				{
					++i1;
					++i2;
				}
			}
			return _size < s._size;
		}
		bool operator==(const string& s)const
		{
			return _size == s._size
				&& memcmp(_str, s._str, _size) == 0;
		}
		bool operator<=(const string& s)const
		{
			return *this < s || *this == s;
		}
		bool operator>(const string& s)const
		{
			return !(*this <= s);
		}
		bool operator>=(const string& s)const
		{
			return !(*this < s);
		}
		bool operator!=(const string& s)const
		{
			return !(*this == s);
		}

		void reserve(size_t n)
		{
			if (n > _capacity)
			{
				char* tmp = new char[n + 1];
				memcpy(tmp, _str, _size + 1);
				delete[] _str;
				_str = tmp;
				_capacity = n;
			}
		}

		void push_back(char ch)
		{
			if (_size == _capacity)
			{
				//2倍扩容
				reserve(_capacity == 0 ? 4 : _capacity * 2);
			}
			_str[_size] = ch;
			++_size;
			_str[_size] = '\0';
		}

		void append(const char* str)
		{
			size_t len = strlen(str);
			if (_size + len > _capacity)
			{
				//至少扩容到_size_t + len
				reserve(_size + len);
			}
			//strcpy(_str + _size, str);
			memcpy(_str + _size, str, len + 1);
			_size += len;
		}

		void insert(size_t pos,size_t n,char ch)
		{
			assert(pos <= _size);

			if (_size + n > _capacity)
			{
				reserve(_size + n);
			}

			//挪动数据
			size_t end = _size;
			//注意整形提升
			while (end >= pos && end != npos)
			{
				_str[end + n] = _str[end];
				end--;
			}

			for (int i = 0; i < n; i++)
			{
				_str[pos + i] = ch;
			}

			_size += n;
		}

		void insert(size_t pos, const char* str)
		{
			assert(pos <= _size);

			size_t len = strlen(str);

			if (_size + len > _capacity)
			{
				reserve(_size + len);
			}

			//挪动数据
			size_t end = _size;
			//注意整形提升
			while (end >= pos && end != npos)
			{
				_str[end + len] = _str[end];
				end--;
			}

			for (int i = 0; i < len; i++)
			{
				_str[pos + i] = str[i];
			}

			_size += len;
		}

		void erase(size_t pos,size_t len=npos)
		{
			assert(pos <= _size);

			if (len == npos || pos + len >= _size)
			{
				_str[pos] = '\0';
				_size = pos;
			}
			else
			{
				size_t end = pos + len;
				while (end<=_size)
				{
					_str[pos++] = _str[end++];
				}
				_size -= len;
			}
		}

		size_t find(char ch,size_t pos=0)
		{
			assert(pos <= _size);

			for (size_t i = pos; i < _size; i++)
			{
				if (_str[i] == ch)
					return i;
			}
			return npos;
		}

		size_t find(const char* str, size_t pos = 0)
		{
			assert(pos <= _size);

			const char* ptr = strstr(_str + pos, str);
			if (ptr)
			{
				return ptr - _str;
			}
			else
			{
				return npos;
			}
		}

		string substr(size_t pos = 0, size_t len = npos)
		{
			size_t n = len;
			if (len == npos || pos + len > _size)
			{
				n = _size - pos;
			}
			string tmp;
			tmp.reserve(n);
			for (size_t i = pos; i < pos + n; i++)
			{
				tmp += _str[i];
			}
			return tmp;
		}

		void resize(size_t n, char ch = '\0')
		{
			if (n < _size)
			{
				_size = n;
				_str[_size] = '\0';
			}
			else
			{
				reserve(n);
				
				for (size_t i = _size; i < n; i++)
				{
					_str[i] = ch;
				}
			}
		}

		void clear()
		{
			_size = 0;
			_str[_size] = '\0';
		}

	private:
		size_t _size;
		size_t _capacity;
		char* _str; 
	public:
		//const static size_t npos = -1;
		static size_t npos;
	};
	size_t string::npos = -1;

	ostream& operator<<(ostream& out, const string& s)
	{
		for (auto ch : s)
		{
			out << ch;
		}
		return out;
	}

	istream& operator>>(istream& in, string& s)
	{
		/*s.clear();
		char ch = in.get();
		while (ch != ' ' && ch != '\n')
		{
			s += ch;
			ch = in.get();
		}
		return in;*/
		s.clear();
		char ch = in.get();
		while (ch == ' ' || ch == '\n')
		{
			ch= in.get();
		}
		char buff[128];
		int i = 0;

		while (ch != ' ' && ch != '\n')
		{
			buff[i++] = ch;
			if (i == 127)
			{
				buff[i] = '\0';
				s += buff;
				i = 0;
			}
			ch = in.get();
		}
		if (i != 0)
		{
			buff[i] = '\0';
			s += buff;
		}
		return in;
	}
};
```

# 4.扩展:写时拷贝(延时拷贝)
对于浅拷贝的两个问题:
1. 析构两次
2. 一个对象改变会影响另一个

解决方法:
1. 引入引用计数
2. 在对一个对象修改时,如果其引用计数不为1,此时再进行深拷贝,再修改

资料:  
[延时拷贝](http://coolshell.cn/articles/12199.html)
[延时拷贝缺陷](http://coolshell.cn/articles/1443.html)





