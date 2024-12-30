# Trajectory-Prediction-Intention-Prediction
Trajectory Prediction&amp; Intention Prediction for autonomous driving, some basic knowledge

Python八股文
1.上下文管理器：一种资源管理的机制，常用于需要进入和退出某种运行环境的场景。典型应用包括文件操作、数据库连接、线程锁。上下文管理器确保资源在使用后正确地释放，避免资源泄露或未关闭问题
上下文管理器使用 with 语句来操作。它通过两个特殊方法实现：
__enter__()：在进入上下文时执行的代码，通常用于资源初始化。
__exit__(exc_type, exc_value, traceback)：在退出上下文时执行的代码，通常用于资源清理。
exc_type：异常的类型。
exc_value：异常的具体值。
traceback：异常的追溯信息。

1）文件操作例子：
with open("example.txt", "w") as file:
    file.write("Hello, Python!")
等价于手动实现：
file = open("example.txt", "w")
try:
    file.write("Hello, Python!")
finally:
    file.close()  # 确保文件被正确关闭

在 with 语句中，open 函数返回一个上下文管理器：
__enter__()：打开文件。
__exit__()：确保无论是否发生异常，文件都能被正确关闭。

2）锁（线程安全）
在多线程编程中，使用上下文管理器可以简化锁的管理
from threading import Lock

lock = Lock()
with lock:
    # 临界区代码
    print("This section is thread-safe.")
等价于
lock.acquire()
try:
    print("This section is thread-safe.")
finally:
    lock.release()
    
3）自定义上下文管理器
class LogManager:
    def __enter__(self):
        print("Starting log...")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Ending log...")
        if exc_type:
            print(f"An error occurred: {exc_value}")

使用自定义上下文管理器
with LogManager() as log:
    print("Inside the context.")
    # raise ValueError("Something went wrong!")  # 测试异常处理

上下文管理器的机制：依赖于 __enter__ 和 __exit__ 两个魔法方法，它们共同实现了上下文环境的管理。上下文管理器的核心目的是提供一种机制，用于在进入和退出某一代码块时执行特定的资源管理逻辑，例如资源分配、初始化、清理或释放。

C++八股文

1.lambda表达式结构：匿名函数的一种
[捕获列表](参数列表) { 执行体 }
#include <iostream>
int main() {
    int x = 10, y = 20;
    auto lambda = [x, y](int a) { return x + y + a; };
    std::cout << lambda(5) << std::endl; // 输出 35
    return 0;
}
1）按值捕获（=）
#include <iostream>
int main() {
    int x = 10, y = 20;
    auto lambda = [=]() { return x + y; }; // 全部按值捕获
    x = 15;  // 修改外部变量
    std::cout << lambda() << std::endl; // 输出 30，与 x 修改无关
    return 0;
2）引用捕获（&）
#include <iostream>
int main() {
    int x = 10, y = 20;
    auto lambda = [&]() { return x + y; }; // 全部按引用捕获
    x = 15;  // 修改外部变量
    std::cout << lambda() << std::endl; // 输出 35，与 x 修改相关
    return 0;
}
3）混合捕获（=，&var和&，var）
#include <iostream>
int main() {
    int x = 10, y = 20;
    auto lambda = [=, &y]() { return x + y; }; // x 按值，y 按引用
    x = 15;
    y = 25;  // 修改 y
    std::cout << lambda() << std::endl; // 输出 35，x 不受修改影响，但 y 修改有效
    return 0;
}
[=]：所有变量按值捕获。
[&]：所有变量按引用捕获。
[=, &var]：默认按值捕获，但 var 按引用捕获。
[&, var]：默认按引用捕获，但 var 按值捕获。
