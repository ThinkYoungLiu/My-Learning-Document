# C++ 多线程系列#

## 1、利用detach来实现守护线程 ##

- 主线程结束，detach的线程即使是死循环，也依旧会被终止
- 使用detach来实现守护线程，主线程不能结束

## 2、线程启动函数的参数传递需类型匹配

- 给线程的执行函数传递参数时，一定要参数匹配，否则会有编译问题。不同的编译器处理可能不一样。 

## 3、获取当前线程id

- 线程id是用于标识当前线程的，所以需要获取当前线程id，以便程序处理一些问题。 std::thread::id tid = std::this_thread::get_id();

## 4、将类的成员函数作为线程启动函数

线程的启动函数：

1. 普通函数
2. 函数对象
3. class中的static函数
4. lambda表达式

## 5、不同场景下的多任务并发

```
#include<iostream>
#include<thread>
#include<mutex>
using namespace std;

mutex mA;
mutex mB;

void f0()
{
	cout << "enter f0" << endl;
	for (int i = 0; i <5; i++)
	{
		cout << "i=" << i << endl;
	}
}

void f1()
{
	cout << "enter f1" << endl;
	for (int i = 20; i < 25; i++)
	{
		cout << "i=" << i << endl;
	}
}

void f(int id)
{
	if (id == 0) {
		//分场景加锁保护，防止f0被多线程访问
		std::lock_guard<mutex> _1(mA);
		f0();
	}
	else {
		//分场景加锁保护
		std::lock_guard<mutex> _1(mB);
		f1();
	}
	cout << "f end" << endl;
}

int main(int argc, int * argv[])
{

	//两个线程启动同一个函数，f内部实现场景分离
	thread t1(f, 0);
	thread t2(f, 1);
	thread t3(f, 0);
	thread t4(f, 1);
	t1.join();
	t2.join();

	t3.join();
	t4.join();
	cout << "main" << endl;
	system("pause");
}
```

