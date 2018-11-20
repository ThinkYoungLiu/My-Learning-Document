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

线程间通信之条件变量

```c++
#include<iostream>
#include<thread>
#include<mutex>
#include<condition_variable>
using namespace std;

// 使用条件变量的三剑客：
/*
1.互斥元
2.条件变量
3.条件值done
*/
mutex mA;
condition_variable cv;
bool done = false;

void f()
{
	unique_lock<mutex> _1(mA);
	// 条件变量的wait所必须是unique_lock而不是lock_guard，因为wait会在内部调用unique_lock.unlock先解锁，当被唤醒后，条件满足时，会unique_lock.lock
	// 条件为：当done为true时，收到notify的线程会被唤醒，否则即使收到notify，也不会被唤醒
	cv.wait(_1, [] {return done; });
	cout << "has done" << endl;

	// 需要手动释放锁
	_1.unlock();
}

void f2()
{
	// 这里使用lock_guard在mA上加锁即可
	lock_guard<mutex> _1(mA);
	cout << "f2" << endl;
	std::this_thread::sleep_for(1s);

	//必须将条件done设置为true，否则线程t1不会被唤醒
	done = true;

	//通知一个线程，让收到的线程检查其条件，收到通知的线程发现条件满足，则该线程会被唤醒
	cv.notify_one();
}


int main(int argc, int * argv[])
{
	thread t1(f);

	thread t2(f2);

	t1.join();
	t2.join();

	cout << "main" << endl;
	system("pause");
}
```

条件变量的本质在于，只有当条件被满足时，线程才会被唤醒，而不是收到notify了，该等待线程就会被唤醒。 

条件变量实际上实现了线程间的数据共享操作。线程A在修改完某些数据后，通过条件变量，来通知线程B来获取最新的数据修改值。 

