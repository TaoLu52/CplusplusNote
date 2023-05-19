----
##  第一节，第二节
1.  防卫式编程： 头文件中使用如下方式可以防止重复包含。
```c++
	#ifndef __XXX__
	#define __XXX__
	//content
	#endif
```
2.  头文件的一般布局
```c++
#ifndef __XXX__
#define __XXX__

//section 0: 前置声明

//section 1: 类声明

//section 2: 类定义

#endif
```
3. 模板初级使用(class template)
``` C++
template <typename T> //定义类型为T的模板
class complex
{
	public:
		complex(T r=0, T i =0)
		: re(r), im(i)
		{}
	private:
		T re,im;
}
complex<double> c1(2.5,2.0);
complex<int>    c2(1,2);
```
4. 类（class）的声明
```c++
class complex   //class head
{
	public:     //class body
		complex (double r=0, double i =0)
		:re (r),im (i)
		{ }
		complex& operator += (const complex&);
		double real () const {return re;}//inline
		double imag () const {return im;}//inline
	private:
		double re,im;
		friend complex& __doapl (complex*, const complex&);
};

{
	complex c1(2,1);
	complex c2;
}
```
## 第三节

5. 第4点的例子里，在类内定义的函数是inline函数
6. 类的访问级别：
	1. public :
	2. private: 数据一般放private
7. 构造函数（constructor）
	1. 函数名与类名相同。
	2. 
	```C++
		complex (double r=0, double i =0)// r=0,i=0默认实参
		:re (r),im (i)//initialization list 初始化列
		{ }
	```
	相当于
	```c++
	complex (double r=0, double i =0)
	{
		re=r;
		im=i;
	}
	```
	但是上面的方式中运行更快（构造函数特有的）
	3. 构造函数可以没有返回值
	4. 创建对象的例子
	```C++
	complex c1(2,1);
	complex c2;
	complex* p = new complex(4);
	```
	5. 构造函数可以有多种（重载（overloading））
	```c++
	class complex   //class head
	{
		public:     //class body
			complex (double r=0, double i =0)
			:re (r),im (i)
			{ }
			//complex() : re(0),im(0) {}//这两个不能同时存在，由于上一个ctor已经有默认值的情况了
			complex& operator += (const complex&);
			double real () const {return re;}//inline
			double imag () const {return im;}//inline
		private:
			double re,im;
			friend complex& __doapl (complex*, const complex&);
	};
```
	6.  ctor放在private区的情况：
			单例模式（singleton）
		```c++
		class A{
			public:
				static A& getInstance();
				setup(){...}
			private:
				A();
				A(const A& rhs);
				...
		}
		A& A::getInstance()
		{
			static A a;
			return a;
		}
		A::getInstance().setup();
		```
8.  const member functions ( 常量成员函数)
	```c++
	class complex   //class head
	{
		public:     //class body
			complex (double r=0, double i =0)
			:re (r),im (i)
			{ }
			//complex() : re(0),im(0) {}//这两个不能同时存在，由于上一个ctor已经有默认值的情况了
			complex& operator += (const complex&);
			double real () const {return re;}//需要加const 
			double imag () const {return im;}//需要加const
		private:
			double re,im;
			friend complex& __doapl (complex*, const complex&);
	};
```
不会改变数据的函数需要加const。
		如果不加在下面的例子中：
```c++
	const complex c1(2,1);
	cout<<c1.real();//编译器会报错
	cout<<c1.imag();//编译器会报错
```
9. 传参（传值与传引用）
