# 07 类
类的基本思想是**数据抽象**和**封装**。数据抽象是一种依赖于**接口**和**实现**分类的编程技术。
封装实现了类的接口和实现的分离，封装后的类隐藏了它的实现细节，也就是说，类的用户只能使用接口而无法访问实现部分。 
## 1. 定义抽象数据类型
### 1.1 设计Sales_data类
Sales_data类的接口应该包含以下操作
- 一个isbn成员函数，用于返回对象的ISBN编号
- 一个combine成员函数，用于将一个Sales_data对象加到另一个对象上
- 一个名为add的函数，执行两个Sales_data对象的加法
- 一个read函数，将数据从istream读入到Sales_data对象中
- 一个print函数，将Sales_data对象的值输出到ostream
Sales_data的具体使用方法
```
Sales_data total;
if (read(cin,total)){
    Sales_data trans;
    while (read(cin,trans)){
        if (total.isbn()==trans.isbn())
          total.combine(trans);
        else{
        print(cout,total)<<endl;
        total=trans;
        }
    }
    print (cout<< total)<<endl;
    else{
    cerr<<"No data?!"<<endl;
    }
}
```
### 1.2定义改进的Sales_data类
改进的sales_data类应该如下：
```
struct Sales_data{
    std::string isbn() const {return bookNo;} //函数后面加const，代表该函数不允许修改类的成员
    Sales_data& combine (const Sales_data&);
    double avg_price() const;
    std::string bookNo;
    unsigned units_sold=0;
    double revenue=0;
};
Sales_data add(const Sales_data&, const Sales_data&)
std::istream &print(std::ostream&, const Sales_data&)
std::ostream &read(std::istream&,Sales_data&);
```
### 1.3 定义类相关的非成员函数
### 1.4 构造函数
我们将定义4个不同的构造函数
- 一个istream&，从中读取一条信息。
- 一个const stirng&，表示ISBN；一个unsigned，表示售出的图书数量；以及一个double，表示图书的售出价格。
- 一个const string&，表示ISBN;编译器将赋予其他成员默认值。
- 一个空参数列表（如默认构造函数）。
 ```
struct Sales_data{
  //新增构造函数
  Sales_data()=default;
  Sales_data(const std::string &s):bookNo(s){}
  Sale_data(const std::string &s,unsigned n,double p):bookNo(s),units_sold(n),revenue(p*n){}
  Sales_data(std::istream &)
  //之前已有的其他成员
  ......
}
```
这里出现了新的部分，即冒号以及冒号和花括号之间的代码。花括号代表函数体，新出现的部分称为**构造函数初始值列表**。构造函数初始值是成员名字的一个列表，每个名字后面紧跟括号括起来的成员初始值。不同成员之间用逗号隔开。
对于前三类，花括号里没有内容（没有函数体）。因为这些构造函数唯一目的就是为数据成员赋初值，一旦没有其他任务需要执行，函数体也就空了。
对于第四个构造函数，我们在类外做如下定义：
```
Sales_data::Sales_data (std::istream &is)
{
  read(is,*this);
}
```
### 1.5拷贝、赋值和析构
除了定义类的对象的初始化之外，类还需要控制拷贝，赋值和销毁对象时发生的行为。
## 2. 访问控制与封装
每个访问符制定了接下来的成员的访问级别， 其有效范围直到出现下一个访问说明符或者到达类的结尾为止。
- public 说明符之后的成员在整个程序内可被访问。
- private 说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码访问。
- friend 类可以允许其他类或者函数访问它的非公有成员，方法是另其他类或者函数成为它的友元（friend）。如果类想把一个函数作为他的友元，只需要增加以friend关键字开头的函数声明语句即可。友元不是类的成员。
```
class Sales_data{
//为Sales_data的非成员函数所做的友元声明
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::istream &read(std::istream&,Sales_data&);
friend std::ostream &print(std::ostream&, const Sales_data&);
//其他成员及访问说明符与之前一致。
public:
  Sale_data()=default;
  Sale_data(const std::string &s,unsigned n, double p):bookNo(s),units_sold(n),revenue(p*n){}
  Sales_data(const std::string &s):bookNo(s){}
  Sales_data(std::istream&);
  std::string isbn() const {return bookNo;}
  Sales_data &combine(const Sales_data &);
private:
  std::string bookNo;
  unsigned units_sold=0;
  double revenue=0.0;
}
//Sales_data接口的非成员组成部分的声明
Sales_data add(const Sales_data&, const Sales_data&);
std::istream &read(std::istream&,Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
```
## 3. 类的其他特性
### 3.1 类成员再谈
mutable 和 * this

## 4. 类的作用域
## 5. 构造函数再探
## 6. 类的静态成员