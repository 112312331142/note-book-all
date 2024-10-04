# C++STL容器

c++容器总共分为两大类，一类是序列式容器，包括vector(连续存储),deque(双向队列),list(双向链表),stack(栈),queue(队列)，另一类是关联式容器，包括set,multiset,map,multimap.

### 1.vector容器

vector是一种动态数组，可以随机存储元素。

```c++
//vector对象默认构造
vector <int> VecInt;
class CA{};
vector<CA*> vecpCA;//用于存放CA对象指针的vector容器
vector<CA> vecCA;//用用于存放CA对象的vector的容器
    
//vector对象带参构造
int arr[]={1,2,3,4,5};
vector<int> v1(arr,arr+5);
vector<int> v2(3,10);
vector<int> v3;
vector<int> v4=v1;

//vector 赋值
vector assign(beg,end);
vector assign(n,elem)；
vector.swap(vec);

//vector比大小
vector.size();
vector.empty();
vector.resize(num);

//vector访问方式
//1.下标法访问
vector[num];

//vector末尾的添加移除操作
vector.push_back();
vector.pop_back();
vector.insert(pos,elem);
vector.insert(pos,n,elem);
vector.insert(pos,beg,end);
```

迭代器是一种检查容器内元素并且遍历容器内元素的数据类型，每个容器中都实现了一个迭代器用于对容器中对象的访问，虽然每个容器中的迭代器实现方式不一样，但是对于用户来说操作方法是一致的，也就是说通过迭代器统一了对所有容器的访问方式。

```c++
//简单的运用迭代器遍历
vector<int> cont = { 1,2,3,3,3,3,4,5,6 };
vector<int>::iterator it;
for (it = cont.begin(); it != cont.end(); it++)
	cout << *it << " ";//1 2 3 3 3 3 4 5 6 
cout << endl;

```

### 2. deque容器

deque是一个双端数组，与vector操作几乎一样，比deque多两个函数。

```c+
deque.push_front(elem);
deque.pop_front;
```

### 3.list容器

list是一个双向链表容器，可高效地进行插入删除元素，不可以随机存储元素，不支持at与[]操作。

```c++
it++;//(ok)
it+5;//(error),一次只能走一步
list.front;

//list容器的迭代器是双向迭代器，从两个方向读写容器，除了提供前向迭代器的全部操作，双向迭代器还提供前置和后置的自减运算
list.begin();
list.end();
list.rbegin();
list.rend();

//list对象的带参构造
list(n.elem);
list(beg,end);
list(const list&lst);

//list的赋值
list.assign(beg,end);
list&operator=(const list&lst);
list.swap(lst);

//list的插入不会导致迭代器失效，而vector与deque会导致其失效

//list的删除
list.clear();
list.erase(beg,end);
list.erase(pos);
list.remove(elem);

//删除会导致迭代器失效
```



### 4.stack容器

stack相当于数据结构的栈。

```c+
stack.push(elem);
stack.pop();
s.top();

//stack对象的拷贝构造与赋值
s2(s1);
s3=s1;

//stack的大小
stack.empty();
stack.size();
```



### 5.queue容器

先进先出容器

```c++
queue.push(elem);//队尾添加元素
queue.pop();//队首移除元素

//queue容器没有提供迭代器
queue.front();
queue.back();
```

---

### 6.set容器

* set是一个集合容器，所包含元素是唯一的，集合中的元素按一定的顺序排列，元素插入过程是按排序规则插入，不能指定插入位置。

* set采用红黑树变体的数据结构实现，属于平衡二叉树，再插入和删除上比vector快。

* set不可以直接存储元素（不可以使用at.(pos)与[]操作符）。

* set支持唯一键值，每元素值只能出现一次，而muliset可出现多次。

* 不可以直接修改set和multiset容器中的元素值，因为该容器是自动排序的，如果修改一个元素值，必须删除原有的元素，再插入新的元素。

* set又去重功能，默认是升序排序。

  ```c++
  //set拷贝构造
  s2(s1);
  s2=s1;
  s2.swap(s1);

  //set容器的删除
  set.clear();
  set.erase(pos);
  set.erase(beg,end);
  set.erase(elem);
  //不能使用反向迭代器删除

  //set元素的排序
  set<int,less<int>> setIntA;
  set<int,greater<int>> setIntB;
  ```

  函数对象functor的用法：

  尽管函数指针被广泛用于实现函数回调，但C++还提供了一个重要的实现函数回调的方法，那就是函数对象。functor是重载了”（）“操作符的普通类函数，从语法上讲，它与普通函数行为类似。

  greater<>于less<>就是函数对象。容器就是调用函数对象的operator（）方法去比较两个值的大小。

```c++
//set容器的查找
set.find(num);
set.count(elem);
set.lower_bound(elem);//返回第一个>=elem元素的迭代器
set.upper_bound(elem);//返回第一个<elem元素的迭代器

//set容器
set.equal_range();
//返回容器中与elem相等的上下限的两个迭代器。上限是闭区间，下限是开区间。
//函数返回两个迭代器，而这两个迭代器被封装到pair中
```

pair译为对组，可以将两个值视为一个单元。

pair<T1,T2>存放的两个值的类型可以不一样。

pair.first是pair里面的第一个值，是T1类型，pair.second是pair第二个值，是T2类型

### 7.map容器

map/multimap采用模板类实现对象的默认构造类型。

```c++
map<T1,T2> mapTT;
multimap<T1,T2> multimapTT;
//T1，T2可以用各种指针类型和自定义类型

//map容器的输入
map.insert(...);//容器中插入元素，返回pair
```

* 通过pair的方式插入对象：mapStu.insert(pair(3,"小张"))。
* 通过value-type的方式插入对象：3是键，”小张“是值

```c++
mapStu.insert(map<int,string>::value-type(1,"小李"))；
```

* 通过数组的方式插入值：mapStu[3]="小刘"。

上述第三种方式非常直观，插入3时，现在mapStu中查找主键为3的项，没发现，则插入到map中，若发现3这个键，则修改对应value为小刘。

注：

```c++
string Name=mapStu[2];
```

只有当mapStu存在2这个键时，才是正确的取操作，否则会自动插入一个实例，键为2，值为初始值。

map容器获取对象键对应的值：

* 使用[];
* 使用find函数，成功返回对应的迭代器，失败返回end的返回值；
* 使用at()函数，如果键值对不存在，会抛出"out_of_range"异常。

























