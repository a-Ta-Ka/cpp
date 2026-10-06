# 第十章

## 10.1 基本概念

STL（Standard Template Library，标准模板库)是惠普实验室开发的一系列软件的统称。现然主要出现在C++中，但在被引入C++之前该技术就已经存在了很长的一段时间。

STL的从广义上讲分为三类：algorithm（算法）、container（容器）和iterator（迭代器），容器和算法通过迭代器可以进行无缝地连接。几乎所有的代码都采 用了模板类和模板函数的方式，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。在C++标准中，STL被组织为下面的13个头文 件：

`
``<algorithm>、<deque>、<functional>、<iterator>、<vector>、<list>、<map>、<memory>、<numeric>、<queue>、<set>、<stack> 和<utility>``
`

使用STL的好处

1）STL是C++的一部分，因此不用额外安装什么，它被内建在你的编译器之内。

2）STL的一个重要特点是数据结构和算法的分离。尽管这是个简单的概念，但是这种分离确实使得STL变得非常通用。

例如，在STL的vector容器中，可以放入元素、基础数据类型变量、元素的地址；

STL的sort()函数可以用来操作vector,list等容器。

3）程序员可以不用思考STL具体的实现过程，只要能够熟练使用STL就OK了。这样他们就可以把精力放在程序开发的别的方面。

4）STL具有高可重用性，高性能，高移植性，跨平台的优点。

* 高可重用性：STL中几乎所有的代码都采用了模板类和模版函数的方式实现，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。关于模板的知识，已经给大家介绍了。
* 高性能：如map可以高效地从十万条记录里面查找出指定的记录，因为map是采用红黑树的变体实现的。(红黑树是平横二叉树的一种)
* 高移植性：如在项目A上用STL编写的模块，可以直接移植到项目B上。
* 跨平台：如用windows的Visual Studio编写的代码可以在Mac OS的XCode上直接编译。

## 10.2 容器

### 10.2.1 容器的分类

**1、序列式容器（Sequence containers）**

* 每个元素都有固定位置－－取决于插入时机和地点，和元素值无关。
* vector、deque、list、stack、queue

**2、关联式容器（Associated containers）**

* 元素位置取决于特定的排序准则，和插入顺序无关
* set、multiset、map、multimap

### 10.2.2 vector 容器

1、 vector容器简介

* vector是将元素置于一个动态数组中加以管理的容器。
* vector可以随机存取元素（支持索引值直接存取， 用[]操作符或at()方法，这个等下会详讲）。
* vector尾部添加或移除元素非常快速。但是在中部或头部插入元素或移除元素比较费时

2、vector对象的默认构造

**vector采用模板类实现**，vector对象的默认构造形式

```cpp
vector<T> vecT; 

vector<int> vecInt;        	//一个存放int的vector容器。
vector<float> vecFloat;     	//一个存放float的vector容器。
vector<string> vecString;   	//一个存放string的vector容器。
...				    //尖括号内还可以设置指针类型或自定义类型。
class CA{};
vector<CA*> vecpCA;	  	//用于存放CA对象的指针的vector容器。
vector<CA> vecCA;     	//用于存放CA对象的vector容器。由于容器元素的存放是按值复制的方式进行的，
                        //所以此时CA必须提供CA的拷贝构造函数，以保证CA对象间拷贝正常。
```

3、vector对象的带参数构造

理论知识

* vector(beg,end);    //构造函数将[beg, end)区间中的元素拷贝给本身。注意该区间是左闭右开的区间。
* vector(n,elem);   //构造函数将n个elem拷贝给本身。
* vector(const vector &vec);  //拷贝构造函数

```cpp
int  iArray[] = {0,1,2,3,4};
vector<int> vecIntA(iArray, iArray+5);
// //用构造函数初始化容器vecIntB 
vector<int> vecIntB(vecIntA.begin() , vecIntA.end());  
vector<int> vecIntB(vecIntA.begin() , vecIntA.begin()+3);  
vector<int> vecIntC(3,9); //此代码运行后，容器vecIntB就存放3个元素，每个元素的值是9。
vector<int> vecIntD(vecIntA);
```

4、vector的赋值

理论知识

* vector.assign(beg,end);    //将[beg, end)区间中的数据拷贝赋值给本身。注意该区间是左闭右开的区间。
* vector.assign(n,elem);  //将n个elem拷贝赋值给本身。
* vector& operator=(const vector  &vec);	//重载等号操作符
* vector.swap(vec);  // 将vec与本身的元素互换。

```cpp
vector<int> vecIntA, vecIntB, vecIntC, vecIntD;
int  iArray[] = {0,1,2,3,4};
vecIntA.assign(iArray,iArray+5);
//用其它容器的迭代器作参数。
vecIntB.assign(vecIntA.begin(),  vecIntA.end());  
vecIntC.assign(3,9);
vector<int> vecIntD;
vecIntD = vecIntA;
vecIntA.swap(vecIntD);
```

5、vector的大小

理论知识

* vector.size();	   //返回容器中元素的个数
* vector.empty();	   //判断容器是否为空
* vector.resize(num);   //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
* vector.resize(num, elem);  //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。

> 例如   vecInt是vector  声明的容器，现已包含1,2,3元素。
> int iSize = vecInt.size();		//iSize == 3;
> bool bEmpty = vecInt.empty();	// bEmpty == false;
> 执行vecInt.resize(5);  //此时里面包含1,2,3,0,0元素。
> 再执行vecInt.resize(8,3);  //此时里面包含1,2,3,0,0,3,3,3元素。
> 再执行vecInt.resize(2);  //此时里面包含1,2元素。

6、vector末尾的添加移除操作

* vector vecInt;
* vecInt.push_back(1);  //在容器尾部加入一个元素
* vecInt.push_back(3);
* vecInt.push_back(5);
* vecInt.push_back(7);
* vecInt.push_back(9);
* vecInt.pop_back();
* vecInt.pop_back();

7、vector的数据存取

理论知识：

* vec.at(idx);  //返回索引idx所指的数据，如果idx越界，抛出out_of_range异常。
* vec[idx];  	//返回索引idx所指的数据，越界时，运行直接报错

```cpp
vector<int> vecInt;    //假设包含1 ,3 ,5 ,7 ,9
vecInt.at(2) == vecInt[2]	;		//5
vecInt.at(2) = 8;  或  vecInt[2] = 8;
vecInt 就包含 1, 3, 8, 7, 9值

int iF = vector.front();	//iF==1
int iB = vector.back();	//iB==9
vector.front() = 11;	//vecInt包含{11,3,8,7,9}
vector.back() = 19;	//vecInt包含{11,3,8,7,19}
```

8、vector的插入

理论知识

* vector.insert(pos,elem);   //在pos位置插入一个elem元素的拷贝，返回新数据的位置。
* vector.insert(pos,n,elem);   //在pos位置插入n个elem数据，无返回值。
* vector.insert(pos,beg,end);   //在pos位置插入[beg,end)区间的数据，无返回值

简单案例

```cpp
vector<int> vecA;
vector<int> vecB;
 
vecA.push_back(1);
vecA.push_back(3);
vecA.push_back(5);
vecA.push_back(7);
vecA.push_back(9);
 
vecB.push_back(2);
vecB.push_back(4);
vecB.push_back(6);
vecB.push_back(8);
vecA.insert(vecA.begin(), 11); //{11, 1, 3, 5, 7, 9}
vecA.insert(vecA.begin()+1,2,33); //{11,33,33,1,3,5,7,9}
vecA.insert(vecA.begin() , vecB.begin() , vecB.end()); //{2,4,6,8,11,33,33,1,3,5,7,9}
```

9、vector的删除

理论知识

* vector.clear(); //移除容器的所有数据
* vec.erase(beg,end);  //删除[beg,end)区间的数据，返回下一个数据的位置。
* vec.erase(pos);    //删除pos位置的数据，返回下一个数据的位置。

简单案例:

```cpp
删除区间内的元素
vecInt是用vector<int>声明的容器，现已包含按顺序的1,3,5,6,9元素。
vecInt.erase(vecInt.begin(),vecInt.begin()+3);
//删除vecInt的所有元素
vecInt.clear(); //容器为空
```

10、迭代器

1）迭代器的基本概念

* 什么是迭代器：

  * 迭代器是一种检查容器内元素并且遍历容器内元素的数据类型。
* 迭代器的作用：

  * 迭代器提供对一个容器中的对象的访问方法，并且定义了容器中对象的范围。
* 为什么需要迭代器：

  * STL提供了多种容器，每种容器的实现原理各不相同，如果没有迭代器我们需要记住每一种容器中对象的访问方法，很显然这样会变得非常麻烦。
  * STL提供的许多容器中都实现了一个迭代器用于对容器中对象的访问，虽然每个容器中的迭代器的实现方式不一样，但是对于用户来说操作方法是一致的，也就说通过迭代器统一了对所有容器的访问方式。例如：访问当前元素的下一个元素我们可以通过迭代器自增进行访问。
  * 迭代器是为了提高编程效率而开发的。
* 迭代器的本质：

  * 迭代器是容器类中专门实现的一个访问容器中数据的内嵌类（类中类）
  * 为了统一每个容器中对于迭代器的操作，在容器类中会使用typedef将迭代器类进行别名定义，别名为：**iterator**
  * 迭代器类对容器中元素的访问方式：指针
  * 迭代器类的具体实现：为了隐藏每个容器中迭代器的具体实现，也为了统一用户对于每个容器中迭代器的访问方式，用户可以把迭代器当成一个指针对容器中的元素进行访问。但是因为迭代器不是指针，因此在迭代器类中我们需要对 * 、->、前置++/--、后置++/--等操作符进行重载。
    ```cpp
    T &operator*() const {}
    node<T>*operator->() const {}
    list_iterator &operator++() {}
    list_iterator operator++(int) {}
    bool operator==(const list_iterator &t) const {}
    bool operator!=(const list_iterator &t) const {}
    ```

2）vector容器的迭代器

每种容器类型都定义了自己的迭代器类型，如vector：

```cpp
vector<int>::iterator iter; //变量名为iter。
```

3）vector容器迭代器类中的成员函数

vector容器的迭代器属于“**随机访问迭代器**”：**迭代器一次可以移动多个位置**

4）begin和end操作

每种容器都定义了一队命名为begin和end的函数，用于返回迭代器。如果容器中有元素的话，由begin返回的元素指向第一个元素。

```cpp
vector<int>::iterator iter=v.begin(); //若v不为空，iter指向v[0]。
```

由end返回的迭代器指向最后一个元素的下一个, 若v为空，begin和end返回的相同。

* **++iter;//使迭代器自增指向下一个元素**
* ==和!=操作符来比较两个迭代器，若两个迭代器指向同一个元素，则它们相等，否则不想等。

迭代器使用举例：

```cpp
for(vector<int>::iterator iter=v.begin();iter!=v.end();iter++)
    *iter=0; //将vector中的元素全部赋值为0;
```

5）迭代器的算术操作

* iter+n; //迭代器iter加上n，指在当前迭代器所在的位置i（如在vector第一个元素位置）之前加上n个元素后的位置。
* iter-n;  //迭代器iter减去n，指在当前迭代器的所在位置之后减n个元素的位置

5）迭代器失效

* 插入元素导致迭代器失效

代码：

```cpp
int main()
{
	vector<int> v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    v.push_back(4);//v有三个元素 1，2，3,4

    vector<int>::iterator it1 = v.begin() + 3;
    v.insert(it1, 8);//插入一个8
    cout << *it1 << endl;//输出it位置的元素
    return 0;
}
```

运行上面这段代码，我们会发现输出的结果并不是8，甚至有可能会导致程序崩溃。这是为什么呢？

**因为在insert时，vector可能需要进行扩容，而扩容的本质是new一块新的空间，再将数据迁移过去。而我们知道，迭代器的内部是通过指针访问容器中的元素的，而插入后，若vector扩容，则原有的数据被释放，指向原有数据的迭代器就成了野指针，所以迭代器失效了。**

而解决的办法很简单，insert函数提供了返回值，这个返回值是指向插入之后的val的迭代器。我们只需要保存返回的迭代器，并使用这个新的迭代器即可。

```cpp
int main()
{
	vector<int> v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    v.push_back(4);//v有三个元素 1，2，3,4

    vector<int>::iterator it1 = v.begin() + 3;

    it1 = v.insert(it1, 8);//插入一个8
    cout << *it1 << endl;//输出it位置的元素
    return 0;
}
```

* 删除元素导致迭代器失效

代码：

```cpp
vector<int> cont ={1,2,3,3,3,4,5,5,5,6};
for (iter = cont.begin(); iter != cont.end();iter++)
{
       if (*iter == 3)
           cont.erase(iter);
}
```

对于序列式容器(如vector,deque)，序列式容器就是数组式容器，删除当前的iterator会使后面所有元素的iterator都失效。这是因为vetor,deque使用了连续分配的内存，删除一个元素导致后面所有的元素会向前移动一个位置。所以不能使用erase(iter++)的方式，还好erase方法可以返回下一个有效的iterator。

解决办法：

```cpp
vector<int> cont ={1,2,3,3,3,4,5,5,5,6};
for (iter = cont.begin(); iter != cont.end();)
{
       if (*iter == 3)
           iter = cont.erase(iter);
       else
           iter++;  
}
```

### 10.2.3 deque容器

deque简介：

* deque是“double-ended queue”的缩写，和vector一样都是STL的容器，deque是双端数组，而vector是单端的。
* deque在接口上和vector非常相似，在许多操作的地方可以直接替换。
* deque可以随机存取元素（支持索引值直接存取， 用[]操作符或at()方法，
* deque头部和尾部添加或移除元素都非常快速。但是在中部安插元素或移除元素比较费时。

deque与vector在操作上几乎一样，deque多两个函数：

* deque.push_front(elem); //在容器头部插入一个数据
* deque.pop_front(); //删除容器第一个数据

### 10.2.4 list容器

1、list简介

* list是一个双向链表容器，可高效地进行插入删除元素。
* list不可以随机存取元素，所以不支持at.(pos)函数与[]操作符。It++(ok)    it+5(err)

2、list对象的默认构造

list采用模板类实现,对象的默认构造形式：``list<T> lst ``如：

```cpp
list<int> lstInt;            //定义一个存放int的list容器。
list<float> lstFloat;     //定义一个存放float的list容器。
list<string> lstString;     //定义一个存放string的list容器。
```

3、list头尾的添加移除操作

* list.push_back(elem);    //在容器尾部加入一个元素
* list.pop_back();              //删除容器中最后一个元素
* list.push_front(elem);     //在容器开头插入一个元素
* list.pop_front();              //从容器开头移除第一个元素

```cpp
list<int> lstInt;
lstInt.push_back(1);
lstInt.push_back(3);
lstInt.push_back(5);
lstInt.push_back(7);
lstInt.push_back(9);
lstInt.pop_front();
lstInt.pop_front();
lstInt.push_front(11);
lstInt.push_front(13);
lstInt.pop_back();
lstInt.pop_back();
```

4、list的数据存取

* list.front();   //返回第一个元素。
* list.back();  //返回最后一个元素。

```cpp
list<int> lstInt;
lstInt.push_back(1);
lstInt.push_back(3);
lstInt.push_back(5);
lstInt.push_back(7);
lstInt.push_back(9);
 
int iFront = lstInt.front(); //1
int iBack = lstInt.back(); //9
lstInt.front() = 11; //11
lstInt.back() = 19; //19
```

5、list与迭代器

list 容器的迭代器是“双向迭代器”：双向迭代器从两个方向读写容器。除了提供前向迭代器的全部操作之外，双向迭代器还提供前置和后置的自减运算

* list.begin();                     //返回容器中第一个元素的迭代器。
* list.end();                       //返回容器中最后一个元素之后的迭代器。
* list.rbegin();         //返回容器中倒数第一个元素的迭代器。
* list.rend();         //返回容器中倒数最后一个元素的后面的迭代器。

```cpp
list<int> lstInt;
lstInt.push_back(1);
lstInt.push_back(3);
lstInt.push_back(5);
lstInt.push_back(7);
lstInt.push_back(9);
 
for (list<int>::iterator it=lstInt.begin(); it!=lstInt.end(); ++it)
{
    cout << *it;
    cout << " ";
}
 
for (list<int>::reverse_iterator rit=lstInt.rbegin(); rit!=lstInt.rend(); ++rit)
{
    cout << *rit;
    cout << " ";
}
```

6、list对象的带参数构造

* list(n,elem);   //构造函数将n个elem拷贝给本身。
* list(beg,end);   //构造函数将[beg,end]区间中的元素拷贝给本身
* list(const list &lst);  //拷贝构造函数。

```cpp
list<int> lstIntA;
lstIntA.push_back(1);
lstIntA.push_back(3);
lstIntA.push_back(5);
lstIntA.push_back(7);
lstIntA.push_back(9);
 
list<int> lstIntB(lstIntA.begin(),lstIntA.end()); //1 3 5 7 9
list<int> lstIntC(5,8); //8 8 8 8 8
list<int> lstIntD(lstIntA); //1 3 5 7 9
```

7、list的赋值

* list.assign(beg,end);    //将[beg, end)区间中的数据拷贝赋值给本身。注意该区间是左闭右开的区间。
* list.assign(n,elem);  //将n个elem拷贝赋值给本身。
* list& operator=(const list &lst); //重载等号操作符
* list.swap(lst);  // 将lst与本身的元素互换。

```cpp
list<int> lstIntA,lstIntB,lstIntC,lstIntD;
lstIntA.push_back(1);
lstIntA.push_back(3);
lstIntA.push_back(5);
lstIntA.push_back(7);
lstIntA.push_back(9);
 
lstIntB.assign(lstIntA.begin(),lstIntA.end()); //1 3 5 7 9
lstIntC.assign(5,8); //8 8 8 8 8
lstIntD = lstIntA; //1 3 5 7 9
lstIntC.swap(lstIntD); //互换
```

8、list的大小

* list.size();    //返回容器中元素的个数
* list.empty();    //判断容器是否为空
* list.resize(num);   //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
* list.resize(num, elem);  //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。

```cpp
list<int> lstIntA;
lstIntA.push_back(1);
lstIntA.push_back(3);
lstIntA.push_back(5);
 
if (!lstIntA.empty())
{
    int iSize = lstIntA.size(); //3
    lstIntA.resize(5); //1 3 5 0 0
    lstIntA.resize(7,1); //1 3 5 0 0 1 1
    lstIntA.resize(2); //1 3
}
```

9、list的插入

* list.insert(pos,elem);   //在pos位置插入一个elem元素的拷贝，返回新数据的位置。
* list.insert(pos,n,elem);   //在pos位置插入n个elem数据，无返回值。
* list.insert(pos,beg,end);   //在pos位置插入[beg,end)区间的数据，无返回值。

  ```cpp
  list<int> lstA;
  list<int> lstB;
   
  lstA.push_back(1);
  lstA.push_back(3);
  lstA.push_back(5);
  lstA.push_back(7);
  lstA.push_back(9);
   
  lstB.push_back(2);
  lstB.push_back(4);
  lstB.push_back(6);
  lstB.push_back(8);
   
  lstA.insert(lstA.begin(), 11); //{11, 1, 3, 5, 7, 9}
  lstA.insert(++lstA.begin(),2,33); //{11,33,33,1,3,5,7,9}
  lstA.insert(lstA.begin() , lstB.begin() , lstB.end() ); //{2,4,6,8,11,33,33,1,3,5,7,9}
  ```

10、list的删除

* list.clear(); //移除容器的所有数据
* list.erase(beg,end);  //删除[beg,end)区间的数据，返回下一个数据的位置。
* list.erase(pos);    //删除pos位置的数据，返回下一个数据的位置。
* lst.remove(elem);   //删除容器中所有与elem值匹配的元素。

```cpp
list<int>::iterator itBegin=lstInt.begin();
++itBegin;
list<int>::iterator itEnd=lstInt.begin();
++itEnd;
++itEnd;
++itEnd;
lstInt.erase(itBegin,itEnd);
//此时容器lstInt包含按顺序的1,6,9三个元素。

lstA.push_back(3);
lstA.push_back(3);
lstA.remove(3); //将list中所有的3删除
lstA.clear();//容器为空
```

11、list的反序排列

* lst.reverse();     //反转链表，比如lst包含1,3,5元素，运行此方法后，lst就包含5,3,1元素。

```cpp
list<int> lstA;
lstA.push_back(1);
lstA.push_back(3);
lstA.push_back(5);
lstA.push_back(7);
lstA.push_back(9);
 
lstA.reverse(); //9 7 5 3 1
```

12、list迭代器失效

* 删除结点导致迭代器失效

```cpp
for(list<int>::iterator it=lstInt.begin(); it!=lstInt.end(); )    //小括号里不需写  ++it
{
   if(*it == 3)
   {
       lstInt.erase(it);  //以迭代器为参数，删除元素3，并把数据删除后的下一个元素位置返回给迭代器。
   }
}
```

### 10.2.5 stack容器

1、Stack简介

* stack是堆栈容器，是一种“先进后出”的容器。
* stack是简单地装饰deque容器而成为另外的一种容器。

2、stack对象的默认构造

> stack采用模板类实现， stack对象的默认构造形式：`stack<T> s;`

```cpp
stack <int> stkInt;            //一个存放int的stack容器。
stack <float> stkFloat;     //一个存放float的stack容器。
stack <string> stkString;     //一个存放string的stack容器。
...   
//尖括号内还可以设置指针类型或自定义类型。
```

3、stack的push()与pop()方法

```cpp
stack.push(elem);   //往栈头添加元素
stack.pop();   //从栈头移除第一个元素
 
stack<int> stkInt;  
stkInt.push(1);stkInt.push(3);stkInt.pop();   
stkInt.push(5);stkInt.push(7);  
stkInt.push(9);stkInt.pop();   
stkInt.pop();  
//此时stkInt存放的元素是1,5  
```

4、 stack对象的拷贝构造与赋值

* stack(const stack &stk);      //拷贝构造函数
* stack& operator=(const stack &stk); //重载等号操作符

```cpp
stack<int> stkIntA;
stkIntA.push(1);
stkIntA.push(3);
stkIntA.push(5);
stkIntA.push(7);
stkIntA.push(9);
 
stack<int> stkIntB(stkIntA); //拷贝构造
stack<int> stkIntC;
stkIntC = stkIntA; //赋值
```

5、 stack的数据存取

```cpp
stack.top();   //返回最后一个压入栈元素
stack<int> stkIntA;
stkIntA.push(1);
stkIntA.push(3);
stkIntA.push(5);
stkIntA.push(7);
stkIntA.push(9);
 
int iTop = stkIntA.top(); //9
stkIntA.top() = 19; //19
```

6、stack的大小

* stack.empty();   //判断堆栈是否为空
* stack.size();      //返回堆栈的大小

```cpp
stack<int> stkIntA;
stkIntA.push(1);
stkIntA.push(3);
stkIntA.push(5);
stkIntA.push(7);
stkIntA.push(9);
 
if (!stkIntA.empty())
{
    int iSize = stkIntA.size(); //5
}
```

### 10.2.6 queue 容器

1、Queue简介

* queue是队列容器，是一种“先进先出”的容器。

2、queue对象的默认构造

queue采用模板类实现，queue对象的默认构造形式：`queue<T> q;`  如：

```cpp
queue<int> queInt;            //一个存放int的queue容器。
queue<float> queFloat;     //一个存放float的queue容器。
queue<string> queString;     //一个存放string的queue容器。
...     
//尖括号内还可以设置指针类型或自定义类型。
```

3、queue的push()与pop()方法

* queue.push(elem);   //往队尾添加元素
* queue.pop();   //从队头移除第一个元素

```cpp
queue<int> queInt;
queInt.push(1);queInt.push(3);
queInt.push(5);queInt.push(7);
queInt.push(9);queInt.pop();
queInt.pop();
此时queInt存放的元素是5,7,9
```

4、queue对象的拷贝构造与赋值

* queue(const queue &que);      //拷贝构造函数
* queue& operator=(const queue &que); //重载等号操作符

```cpp
queue<int> queIntA;
queIntA.push(1);
queIntA.push(3);
queIntA.push(5);
queIntA.push(7);
queIntA.push(9);
 
queue<int> queIntB(queIntA); //拷贝构造
queue<int> queIntC;
queIntC = queIntA; //赋值
```

5、queue的数据存取

* queue.back();   //返回最后一个元素
* queue.front();   //返回第一个元素

```cpp
queue<int> queIntA;
queIntA.push(1);
queIntA.push(3);
queIntA.push(5);
queIntA.push(7);
queIntA.push(9);
 
int iFront = queIntA.front(); //1
int iBack = queIntA.back(); //9
 
queIntA.front() = 11; //11
queIntA.back() = 19; //19
```

6、queue的大小

* queue.empty();   //判断队列是否为空
* queue.size();      //返回队列的大小

```cpp
queue<int> queIntA; 
queIntA.push(1);   
queIntA.push(3);  
queIntA.push(5);
queIntA.push(7);
queIntA.push(9);
 
if (!queIntA.empty())
{
    int iSize = queIntA.size(); //5
}
```

### 10.2.7 Set和multiset容器

1、set/multiset的简介

* set是一个集合容器，其中所包含的**元素是唯一的**，集合中的**元素按一定的顺序排列**。元素插入过程是按排序规则插入，所以不能指定插入位置。
* **set采用红黑树变体的数据结构实现**，红黑树属于平衡二叉树。在插入操作和删除操作上比vector快。
* set不可以直接存取元素。（不可以使用at.(pos)与[]操作符）。
* multiset与set的区别：set支持唯一键值，每个元素值只能出现一次；而multiset中同一值可以出现多次。
* 不可以直接修改set或multiset容器中的元素值，因为该类容器是自动排序的。如果希望修改一个元素值，必须先删除原有的元素，再插入新的元素。
* ```
  #include <set>
  ```

2、set/multiset对象的默认构造

```cpp
set<int> setInt;            //一个存放int的set容器。
set<float> setFloat;     //一个存放float的set容器。
set<string> setString;     //一个存放string的set容器。
multiset<int> mulsetInt;            //一个存放int的multi set容器。
multi set<float> multisetFloat;     //一个存放float的multi set容器。
multi set<string> multisetString;     //一个存放string的multi set容器。
```

3、set对象的拷贝构造与赋值

* set(const set &st);      //拷贝构造函数
* set& operator=(const set &st); //重载等号操作符
* set.swap(st); //交换两个集合容器

```cpp
set<int> setIntA;
setIntA.insert(3);
setIntA.insert(1);
setIntA.insert(7);
setIntA.insert(5);
setIntA.insert(9);
 
set<int> setIntB(setIntA);  //1 3 5 7 9
set<int> setIntC;
setIntC = setIntA; //1 3 5 7 9
 
setIntC.insert(6);
setIntC.swap(setIntA);   //交换
```

4、set的大小

* set.size(); //返回容器中元素的数目
* set.empty();//判断容器是否为空

```cpp
set<int> setIntA;
setIntA.insert(3);
setIntA.insert(1);
setIntA.insert(7);
setIntA.insert(5);
setIntA.insert(9);
 
if (!setIntA.empty())
{
    int iSize = setIntA.size(); //5
}
```

5、set的插入与迭代器

* set.insert(elem);     //在容器中插入元素。
* set.begin();  //返回容器中第一个数据的迭代器。
* set.end();  //返回容器中最后一个数据之后的迭代器。
* set.rbegin();  //返回容器中倒数第一个元素的迭代器。
* set.rend();   //返回容器中倒数最后一个元素的后面的迭代器。

```cpp
set<int> setInt;
setInt.insert(3); 
setInt.insert(1);
setInt.insert(5);
setInt.insert(2);
for(set<int>::iterator it=setInt.begin(); it!=setInt.end(); ++it)
{
      int iItem = *it;
      cout << iItem;    //或直接使用cout << *it
}
//这样子便顺序输出  1 2 3 5。 
```

5、set的删除

* set.clear(); //清除所有元素
* set.erase(pos); //删除pos迭代器所指的元素，返回下一个元素的迭代器。
* set.erase(beg,end);     //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
* set.erase(elem);     //删除容器中值为elem的元素。

```cpp
删除区间内的元素
setInt是用set<int>声明的容器，现已包含按顺序的1,3,5,6,9,11元素。
set<int>::iterator itBegin=setInt.begin();
++itBegin;
set<int>::iterator itEnd=setInt.begin();
++itEnd;
++itEnd;
++itEnd;
setInt.erase(itBegin,itEnd);
//此时容器setInt包含按顺序的1,6,9,11四个元素。
 
删除容器中第一个元素
setInt.erase(setInt.begin()); //6,9,11
 
删除容器中值为9的元素
set.erase(9);    
 
删除setInt的所有元素
setInt.clear(); //容器为空
```

6、set集合的元素排序

* 要解决如上两个问题，需要了解容器的函数对象，也叫伪函数，英文名叫functor。
* 下面将讲解什么是functor，functor的用法。

```cpp
set<int,greater<int>> setIntB;   
setIntB.insert(3);
setIntB.insert(1);
setIntB.insert(5);
setIntB.insert(2);
此时容器setIntB就包含了按顺序的5,3,2,1元素
```

函数对象functor的用法

* 尽管函数指针被广泛用于实现函数回调，但C++还提供了一个重要的实现回调函数的方法，那就是函数对象。
* functor，翻译成函数对象，伪函数，算符，是重载了“()”操作符的普通类对象。从语法上讲，它与普通函数行为类似。
* greater<>与less<>就是函数对象。

下面举出greater的简易实现原理:

```cpp
class greater
{
    bool operator() (const int& iLeft, const int& iRight)
    {
           return (iLeft>iRight);    //如果是实现less<int>的话，这边是写return (iLeft<iRight);
    }
}
```

**容器就是调用函数对象的operator()方法去比较两个值的大小。**

思考：学生包含学号，姓名属性，现要求任意插入几个学生对象到set容器中，使得容器中的学生按学号的升序排序。

```cpp
//学生类
class CStudent
{
    public:
    CStudent(int iID, string strName)
    {
          m_iID = iID;
          m_strName = strName;
     }
     int m_iID; //学号
     string m_strName; //姓名
}
//本类不写拷贝构造函数。但大家仍要有考虑拷贝构造函数的习惯。
//函数对象
class StuFunctor
{
    bool operator()  (const CStudent &stu1, const CStudent &stu2)
    {
        return (stu1.m_iID<stu2.m_iID);
    }
}
 
//main函数
int main()
{
   set<CStudent, StuFunctor> setStu;
   setStu.insert(CStudent(3,"小张"));
   setStu.insert(CStudent(1,"小李"));
   setStu.insert(CStudent(5,"小王"));
   setStu.insert(CStudent(2,"小刘"));
   //此时容器setStu包含了四个学生对象，分别是按姓名顺序的“小李”，“小刘”，“小张”，“小王” 
}
```

7、set的查找

* set.find(elem);   //查找elem元素，返回指向elem元素的迭代器。
* set.count(elem);   //返回容器中值为elem的元素个数。对set来说，要么是0，要么是1。对multiset来说，值可能大于1。
* set.lower_bound(elem);  //返回第一个 >=elem元素的迭代器。
* set.upper_bound(elem);    //  返回第一个>elem元素的迭代器。

```cpp
set<int> setInt;
setInt.insert(3);
setInt.insert(1);
setInt.insert(7);
setInt.insert(5);
setInt.insert(9);
 
set<int>::iterator itA = setInt.find(5);
int iA = *itA; //iA == 5
int iCount = setInt.count(5); //iCount == 1
 
set<int>::iterator itB = setInt.lower_bound(5);
set<int>::iterator itC = setInt.upper_bound(5);
int iB = *itB; //iB == 5
int iC = *itC; //iC == 7
```

* set.equal_range(elem); //返回容器中与elem相等的上下限的两个迭代器。上限是闭区间，下限是开区间，如[beg,end)。
  * 函数返回两个迭代器，而这两个迭代器被封装在pair中。
  * ```cpp
    pair< set<int>::iterator, set<int>::iterator > pairIt = setInt.equal_range(5);  //pair是什么？
    
  
    pair译为对组，可以将两个值视为一个单元。
    pair<T1,T2>存放的两个值的类型，可以不一样，如T1为int，T2为float。T1,T2也可以是自定义类型。
    pair.first是pair里面的第一个值，是T1类型。
    pair.second是pair里面的第二个值，是T2类型。
    ```

### 10.2.8 map和multimap容器

1、map/multimap的简介

* map是标准的关联式容器，一个map是一个键值对序列，即(key,value)对。它提供基于key的快速检索能力。
* map中key值是唯一的。集合中的元素按一定的顺序排列。元素插入过程是按排序规则插入，所以不能指定插入位置。
* map的具体实现采用红黑树变体的平衡二叉树的数据结构。在插入操作和删除操作上比vector快。
* map可以直接存取key所对应的value，支持[]操作符，`如map[key]=value`(将key键所对应的值修改为value)
* multimap与map的区别：map支持唯一键值，每个键只能出现一次；而multimap中相同键可以出现多次。multimap不支持[]操作符。

2、map/multimap对象的默认构造

```cpp
map/multimap采用模板类实现，对象的默认构造形式：
map<T1,T2> mapTT;
multimap<T1,T2>  multimapTT;  
如：
map<int, char> mapA;
map<string,float> mapB;
//其中T1,T2还可以用各种指针类型或自定义类型
```

3、map的插入与迭代器

```cpp
map.insert(...);    //往容器插入元素，返回pair
```

在map中插入元素的三种方式：

假设  map mapStu;

一、通过pair的方式插入对象

```cpp
mapStu.insert(pair(3,"小张") );
```

二、通过value_type的方式插入对象

```cpp
mapStu.insert(  map<int,string>::value_type(1,"小李")  );
```

三、通过数组的方式插入值

```cpp
mapStu[3] = "小刘";

mapStu[5] = "小王";
```

前两种方法，采用的是insert()方法，该方法返回值为pair

第三种方法非常直观，但存在一个性能的问题。插入3时，先在mapStu中查找主键为3的项，若没发现，则将一个键为3，值为“小刘”的键值对插入到map中。若发现已存在3这个键，则修改这个键对应的value为“小刘”。

如果键存在则修改，如果不存在则插入

string strName = mapStu[2];   //取操作或插入操作

只有当mapStu存在2这个键时才是正确的取操作，否则会自动插入一个实例，键为2，值为初始化值。

```cpp
map<int, string> mapA;
pair<map<int,string>::iterator, bool> pairResult = 
                        mapA.insert(pair<int,string>(3,"小张")); //插入方式一
 
int iFirstFirst = (pairResult.first)->first; //iFirst == 3;
string strFirstSecond = (pairResult.first)->second; //strFirstSecond为"小张"
bool bSecond = pairResult.second; //bSecond == true;
mapA.insert(map<int,string>::value_type(1,"小李")); //插入方式二
mapA[3] = "小刘"; //修改value
mapA[5] = "小王"; //插入方式三
 
string str1 = mapA[2]; //执行插入 string() 操作，返回的str1的字符串内容为空。
string str2 = mapA[3]; //取得value，str2为"小刘"
```

使用迭代器遍历：

```cpp
for (map<int,string>::iterator it=mapA.begin(); it!=mapA.end(); ++it)
{
    pair<int, string> pr = *it;
    int iKey = pr.first;
    string strValue = pr.second;
}
```

4、map容器或者键所对应的值

方法一：使用[]

方法二：使用find()函数：成功返回对应的迭代器，失败返回end()的返回值

```cpp
map<int, string>::iterator it = mapS.find(3);
```

方法三：使用at()函数，如果键值对不存在会抛出“out_of_range 异常”

5、map对象的拷贝构造与赋值

* map(const map &mp);      //拷贝构造函数
* map& operator=(const map &mp); //重载等号操作符
* map.swap(mp); //交换两个集合容器

```cpp
map<int, string> mapA;
mapA.insert(pair<int,string>(3,"小张"));
mapA.insert(pair<int,string>(1,"小杨"));
mapA.insert(pair<int,string>(7,"小赵"));
mapA.insert(pair<int,string>(5,"小王"));
 
map<int ,string> mapB(mapA); //拷贝构造
map<int, string> mapC;
mapC = mapA; //赋值
 
mapC[3] = "老张";
mapC.swap(mapA); //交换
```

6、map的大小

* map.size(); //返回容器中元素的数目
* map.empty();//判断容器是否为空

```cpp
map<int, string> mapA;
mapA.insert(pair<int,string>(3,"小张"));
mapA.insert(pair<int,string>(1,"小杨"));
mapA.insert(pair<int,string>(7,"小赵"));
mapA.insert(pair<int,string>(5,"小王"));
 
if (mapA.empty())
{
    int iSize = mapA.size(); //iSize == 4
}
```

7、map的删除

* map.clear(); //删除所有元素
* map.erase(pos); //删除pos迭代器所指的元素，返回下一个元素的迭代器。
* map.erase(beg,end);     //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
* map.erase(keyElem);     //删除容器中key为keyElem的对组。

```cpp
map<int, string> mapA;
mapA.insert(pair<int,string>(3,"小张"));
mapA.insert(pair<int,string>(1,"小杨"));
mapA.insert(pair<int,string>(7,"小赵"));
mapA.insert(pair<int,string>(5,"小王"));
```

删除区间内的元素：

```cpp
map<int,string>::iterator itBegin=mapA.begin();
++ itBegin;
++ itBegin;
map<int,string>::iterator itEnd=mapA.end();
mapA.erase(itBegin,itEnd); //此时容器mapA包含按顺序的{1,"小杨"}{3,"小张"}两个元素。
 
mapA.insert(pair<int,string>(7,"小赵"));
mapA.insert(pair<int,string>(5,"小王"));
```

删除容器中指定 的元素：

```cpp
mapA.erase(5);    
```

删除容器中指定位置的元素：

```cpp
mapA.erase(mapA.begin());
```

8、map的查找

* map.find(key);   查找键key是否存在，若存在，返回该键的元素的迭代器；若不存在，返回map.end();
* map.count(keyElem);   //返回容器中key为keyElem的对组个数。
* map.lower_bound(elem);  //返回第一个>=elem元素的迭代器。
* map.upper_bound(elem);    //  返回第一个>elem元素的迭代器。
* map.equal_range(elem); //返回容器中与elem相等的上下限的两个迭代器。上限是闭区间，下限是开区间，如[beg,end)。

```cpp
map<int,string>::iterator it=mapStu.find(3);
if(it == mapStu.end())
{
//没找到
}
else
{
    //找到了
        pair<int, string> pairStu = *it;
       int iID = pairStu.first; //或   int  iID = it->first;
        string strName = pairStu.second; //或   string strName = it->second;
}
```

### 10.2.9 总结

## 10.3 算法

### 10.3.1 排序算法

#### 10.3.1.1 sort() 排序函数

* **sort() 函数是基于快速排序实现的**
* sort() 函数受到底层实现方式的限制，它仅适用于普通数组和部分类型的容器。换句话说，只有普通数组和具备以下条件的容器，才能使用 sort() 函数：
  * 容器支持的迭代器类型必须为随机访问迭代器。这意味着，**sort() 只对 vector、deque 这 2个容器提供支持**
  * 如果对容器中指定区域的元素做默认升序排序，则元素类型必须支持 < 小于运算符；同样，如果选用标准库提供的其它排序规则，元素类型也必须支持该规则底层实现所用的比较运算符
  * sort() 函数在实现排序时，需要交换容器中元素的存储位置。这种情况下，如果容器中存储的是自定义的类对象，则该类的内部必须拷贝构造函数和赋值运算符的重载。
  * **sort() 排序是不稳定的**

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::sort
#include <vector>       // std::vector
using namespace std;
//以普通函数的方式实现自定义排序规则
bool mycomp(int i, int j) {
    return (i < j);
}
//以函数对象的方式实现自定义排序规则
class mycomp2 {
public:
    bool operator() (int i, int j) {
        return (i < j);
    }
};
int main() {
	int a[] = { 32, 71, 12, 45, 26, 80, 53, 33 };
    vector<int> myvector(a, a+8);
    //调用第一种语法格式，对 32、71、12、45 进行排序
    sort(myvector.begin(), myvector.begin() + 4); //(12 32 45 71) 26 80 53 33
    //调用第二种语法格式，利用STL标准库提供的其它比较规则（比如 greater<T>）进行排序
    sort(myvector.begin(), myvector.begin() + 4, greater<int>()); //(71 45 32 12) 26 80 53 33
   
    //调用第二种语法格式，通过自定义比较规则进行排序
    sort(myvector.begin(), myvector.end(), mycomp2());//12 26 32 33 45 53 71 80
    //输出 myvector 容器中的元素
    for (vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it) {
        cout << *it << ' ';
    }
    return 0;
}
```

#### 10.3.1.2 stable_sort()排序算法

* **stable_sort() 函数是基于归并排序实现的**
* **stable_sort() 是稳定的排序算法**
* stable_sort()函数与sort()函数的使用方法相同。

#### 10.3.1.3 partial_sort()排序函数

1、引入

* **假设这样一种情境，有一个存有 100 万个元素的容器，但我们只想从中提取出值最小的 10 个元素，该如何实现呢？**

  * 通过前面的学习，可能会想到使用 sort() 或者 stable_sort() 排序函数，即通过对容器中存储的 100 万个元素进行排序，就可以成功筛选出最小的 10 个元素。但仅仅为了提取 10 个元素，却要先对 100 万个元素进行排序，可想而知这种实现方式的效率是非常低的。
  * 对于解决类似的问题，C++ STL 标准库提供了更高效的解决方案，使用 partial_sort()。
* partial sort 可直译为“部分排序”，该函数可以从指定区域中提取出部分数据，并对它们进行排序。

2、语法格式

```cpp
//按照默认的升序排序规则，对 [first, last) 范围的数据进行筛选并排序
void partial_sort (RandomAccessIterator first,
                   RandomAccessIterator middle,
                   RandomAccessIterator last);
//按照 comp 排序规则，对 [first, last) 范围的数据进行筛选并排序
void partial_sort (RandomAccessIterator first,
                   RandomAccessIterator middle,
                   RandomAccessIterator last,
                   Compare comp);
/*
其中，first、middle 和 last 都是随机访问迭代器，comp 参数用于自定义排序规则。
partial_sort() 函数会以交换元素存储位置的方式实现部分排序的。
具体来说，partial_sort() 会将 [first, last) 范围内最小（或最大）的 middle-first 个元素移动到 
[first, middle) 区域中，并对这部分元素做升序（或降序）排序。
*/
```

需要注意的是，partial_sort() 函数受到底层实现方式的限制，它仅适用于普通数组和部分类型的容器。换句话说，只有普通数组和具备以下条件的容器，才能使用 partial_sort() 函数：

* partial_sort() 函数只适用于 array、vector、deque 这 3 个容器。
* 当选用默认的升序排序规则时，容器中存储的元素类型必须支持 < 小于运算符；同样，如果选用标准库提供的其它排序规则，元素类型也必须支持该规则底层实现所用的比较运算符；
* partial_sort() 函数在实现过程中，需要交换某些元素的存储位置。因此，如果容器中存储的是自定义的类对象，则该类的内部必须提供移动构造函数和移动赋值运算符。

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::partial_sort
#include <vector>       // std::vector
using namespace std;
//以普通函数的方式自定义排序规则
bool mycomp1(int i, int j) {
    return (i > j);
}
//以函数对象的方式自定义排序规则
class mycomp2 {
public:
    bool operator() (int i, int j) {
        return (i > j);
    }
};
int main() {
    vector<int> myvector{ 3,2,5,4,1,6,9,7};
    //以默认的升序排序作为排序规则，将 myvector 中最小的 4 个元素移动到开头位置并排好序
    partial_sort(myvector.begin(), myvector.begin() + 4, myvector.end());
    cout << "第一次排序:\n";
    for (vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it)
        cout << *it << ' ';
    cout << "\n第二次排序:\n";
    // 以指定的 mycomp2 作为排序规则，将 myvector 中最大的 4 个元素移动到开头位置并排好序
    partial_sort(myvector.begin(), myvector.begin() + 4, myvector.end(), mycomp2());
    for (vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it)
        cout << *it << ' ';
    return 0;
}
```

#### 10.3.1.4 merge()函数

功能：将两个**已经排好序**的序列合并为一个有序的序列

默认排序规则：

```cpp
//以默认的升序排序作为排序规则
OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                      InputIterator2 first2, InputIterator2 last2,
                      OutputIterator result);
//以自定义的 comp 规则作为排序规则
OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                      InputIterator2 first2, InputIterator2 last2,
                      OutputIterator result, Compare comp);
/*
* firs1t为第一个容器的首迭代器，last1为第一个容器的末迭代器；
* first2为第二个容器的首迭代器，last2为容器的末迭代器；
* result为存放结果的容器，comapre为比较函数（可略写，默认为合并为一个升序序列）。
*/
```

**注意：使用的时候result，如果用的vector，必须先使用resize扩容**

#### 10.3.1.5 revrese()函数

函数参数:reverse(first，last)

功能：反转容器

注意：

* string和vector和deque只能使用模板库算法里的反转函数
* list可以使用算法里的和list类的reverse
* stack和queue没有迭代器，自然不能使用算法里的reverse,其类也没有提供反转的成员函数
* set和map的元素是按照键值排序的，不能修改键值，不可反转.

### 10.3.2 查找算法

#### 10.3.2.1 adjacent_find()

功能：在iterator对标识元素范围内，查找一对相邻重复元素，找到则返回指向这对元素的第一个元素的迭代器。

```cpp
vector<int> vecInt;
vecInt.push_back(1);
vecInt.push_back(2);
vecInt.push_back(2);
vecInt.push_back(4);
vecInt.push_back(5);
vecInt.push_back(5);
 
vector<int>::iterator it = adjacent_find(vecInt.begin(), vecInt.end()); //*it == 2
```

#### 10.3.2.2 binary_search()

功能：二分查找法，在有序序列中查找value,找到则返回true。

```cpp
set<int> setInt;
setInt.insert(3);
setInt.insert(1);
setInt.insert(7);
setInt.insert(5);
setInt.insert(9);
 
bool bFind = binary_search(setInt.begin(),setInt.end(),5);
```

#### 10.3.2.3 count()

功能：利用等于操作符，把标志范围内的元素与输入值比较，返回相等的个数。

```cpp
vector<int> vecInt;
vecInt.push_back(1);
vecInt.push_back(2);
vecInt.push_back(2);
vecInt.push_back(4);
vecInt.push_back(2);
vecInt.push_back(5);
int iCount = count(vecInt.begin(),vecInt.end(),2); //iCount==3
```

#### 10.3.2.4 find()

功能：find() 函数用于在指定范围内查找和目标元素值相等的第一个元素。

函数的语法格式：

```cpp
InputIterator find (InputIterator first, InputIterator last, const T& val);
```

其中，first 和 last 为输入迭代器，[first, last) 用于指定该函数的查找范围；val 为要查找的目标元素。

正因为 first 和 last 的类型为输入迭代器，因此该函数适用于所有的序列式容器。

该函数会返回一个输入迭代器，当 find() 函数查找成功时，其指向的是在 [first, last) 区域内查找到的第一个目标元素；如果查找失败，则该迭代器的指向和 last 相同。

find() 函数的底层实现，其实就是用==运算符将 val 和 [first, last) 区域内的元素逐个进行比对。这也就意味着，[first, last) 区域内的元素必须支持==运算符。

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::find
#include <vector>       // std::vector
using namespace std;
int main() {
    //find() 函数作用于普通数组
    char stl[] ="http://c.biancheng.net/stl/";
    //调用 find() 查找第一个字符 'c'
    char * p = find(stl, stl + strlen(stl), 'c');
    //判断是否查找成功
    if (p != stl + strlen(stl)) {
        cout << p << endl;
    }
    //find() 函数作用于容器
    vector<int> myvector{ 10,20,30,40,50 };
    vector<int>::iterator it;
    it = find(myvector.begin(), myvector.end(), 30);
    if (it != myvector.end())
        cout << "查找成功：" << *it;
    else
        cout << "查找失败";
    return 0;
}
```

#### 10.3.2.5 find_if()

功能：和 find() 函数相同，find_if() 函数也用于在指定区域内执行查找操作。不同的是，前者需要明确指定要查找的元素的值，而后者则允许自定义查找规则。

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::find_if
#include <vector>       // std::vector
using namespace std;

//以函数对象的形式定义一个 find_if() 函数的查找规则
class mycomp2 {
public:
    bool operator()(const int& i) {
        return ((i % 2) == 1);
    }
};
int main() {
    vector<int> myvector{ 4,2,3,1,5 };
    //调用 find_if() 函数
    vector<int>::iterator it = find_if(myvector.begin(), myvector.end(), mycomp2());
    cout << "*it = " << *it;
    return 0;
}
```

#### 10.3.2.5 search()

功能：search()函数用于在序列 A 中查找序列 B 第一次出现的位置。

序列 B 在序列 A 中出现了 2 次，借助 search() 函数，我们可以找到序列 A 中第 1 个 {1,2,3}。

search() 函数提供有以下 2 种语法格式：

```cpp
//查找 [first1, last1) 范围内第一个 [first2, last2) 子序列
ForwardIterator search (ForwardIterator first1, ForwardIterator last1,
                        ForwardIterator first2, ForwardIterator last2);
//查找 [first1, last1) 范围内，和 [first2, last2) 序列满足 pred 规则的第一个子序列
ForwardIterator search (ForwardIterator first1, ForwardIterator last1,
                        ForwardIterator first2, ForwardIterator last2,
                        BinaryPredicate pred);
```

其中，各个参数的含义分别为：

* first1、last1：都为正向迭代器，其组合 [first1, last1) 用于指定查找范围（也就是上面例子中的序列 A）；
* first2、last2：都为正向迭代器，其组合 [first2, last2) 用于指定要查找的序列（也就是上面例子中的序列 B）；
* pred：用于自定义查找规则。该规则实际上是一个包含 2 个参数且返回值类型为 bool 的函数（第一个参数接收 [first1, last1) 范围内的元素，第二个参数接收 [first2, last2) 范围内的元素）。函数定义的形式可以是普通函数，也可以是函数对象。

search() 函数会返回一个正向迭代器，当函数查找成功时，该迭代器指向查找到的子序列中的第一个元素；反之，如果查找失败，则该迭代器的指向和 last1 迭代器相同。

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::search
#include <vector>       // std::vector
using namespace std;
//以普通函数的形式定义一个匹配规则
bool mycomp1(int i, int j) {
    return (i%j == 0);
}
//以函数对象的形式定义一个匹配规则
class mycomp2 {
public:
    bool operator()(const int& i, const int& j) {
        return (i%j == 0);
    }
};
int main() {
    vector<int> myvector{ 1,2,3,4,8,12,18,1,2,3 };
    int myarr[] = { 1,2,3 };
    //调用第一种语法格式
    vector<int>::iterator it = search(myvector.begin(), myvector.end(), 
    myarr, myarr + 3);
    if (it != myvector.end()) {
        cout << "第一个{1,2,3}的起始位置为：" << it - myvector.begin() << ",*it = " << *it << endl;
    }
    int myarr2[] = { 2,4,6 };
    //调用第二种语法格式
    it = search(myvector.begin(), myvector.end(), 
    myarr2, myarr2 + 3, mycomp2());
    if (it != myvector.end()) {
        cout << "第一个{2,3,4}的起始位置为：" << it - myvector.begin() << ",*it = " << *it;
    }
    return 0;
}
```
