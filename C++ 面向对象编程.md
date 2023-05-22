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
尽量传引用。
10. 返回值传递（传值与传应用）
```C++
complex&/*函数返回值*/ operator += (const complex&);
```
尽量传引用。如果返回值是临时变量（返回后对象不存在了），则不能用传引用。
11. 友元
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
	inline complex& __doapl (complex* ths, const complex& r)//友元函数
	{
		ths->re += r.re;
		ths->im += r.im;
		return *ths;
	}
	
```
相同class的各个objects互为友元
```C++
	{
		complex c1(2,1);
		complex c2(2);
		
		c2.func(c1);
	}
```

## 第五节

12. 操作符重载 与 this
C++中操作符其实是特殊的函数。
	1. 作为成员函数
	```
```C++
inline complex&
complex :: operator += (/*this,*/const complex &r)
{
	return __doapl(this,r);
}
```
传递者无需知道接收者是以reference形式接收
![[Pasted image 20230522182433.png]]
![[Pasted image 20230522184311.png]]
在上图2中体现了连续赋值时return的意义。
13. Class body之外的各种定义
![[Pasted image 20230522184630.png]]
![[Pasted image 20230522184741.png]]
14. 操作符重载（非成员函数）
![[Pasted image 20230522184908.png]]

![[Pasted image 20230522185102.png]]
![[Pasted image 20230522191015.png]]
上图中特殊的操作符因为需要与cout兼容，只能用全局写法
![[Pasted image 20230522191351.png]]
## 小结
	1. 数据放在Private
	2. 参数尽量传引用
	3. 返回值尽量传引用
	4. 类的body中可以加const的地方需要加
	5. 构造函数需要用initialization list
## 复习
---

## 三大函数：拷贝构造，拷贝复制，析构