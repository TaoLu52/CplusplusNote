----
### 第一节，第二节
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
		double real () const {return re;}
		double imag () const {return im;}
	private:
		double re,im;
		friend complex& __doapl (complex*, const complex&);
};

{
	complex c1(2,1);
	complex c2;
}
```
