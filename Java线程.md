
* [概述](#概述)
    * [线程与进程的区别](#线程与进程的区别)
    * [资源共享，健壮性](#资源共享健壮性)
    * [程序的并发执行](#程序的并发执行)
    * [进程切换线程切换](#进程切换线程切换)
* [java中的线程的使用](#java中的线程的使用)
    * [创建线程](#创建线程)
    * [使用Runnable创建线程](#使用runnable创建线程)
    * [jconsole的使用](#jconsole的使用)
    * [java Thread的几个常见属性](#java-thread的几个常见属性)
    * [线程是否存活](#线程是否存活)
    * [线程优先级](#线程优先级)
    * [前台线程和后台线程](#前台线程和后台线程)
    * [try和catch的用法](#try和catch的用法)
    * [runnable和thread的区别](#runnable和thread的区别)




### 概述

##### 线程与进程的区别

* 进程：任务管理器中的每一个应用程序，操作系统资源分配的基本单位
* 线程：是进程中的一个执行单元，操作系统调度的基本单位

##### 资源共享，健壮性

* 一个进程可以包含多个线程
* 一个进程中的所有线程共享该进程中的资源（I/O、CPU、内存）
* 多进程比多线程更健壮，一个进程崩溃后，在保护模式下不对其他进程产生影响，但一个线程崩溃整个进程挂掉

##### 程序的并发执行

* 进程：是为了实现程序的并发执行
* 线程：是为了减少程序在并发执行所付出的时空开销，使操作系统具有更好的并发性

##### 进程切换线程切换

* 线程切换快，消耗资源少
* 进程切换慢，消耗资源多
* 频繁切换时，使用线程

### java中的线程的使用

##### 创建线程

```java
public class Main {
    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            while (true) {
                System.out.println("t1");
            }
        }, "线程1");//线程的名字

        Thread t2 = new Thread(() -> {
            while (true) {
                System.out.println("t2");
            }
        }, "线程2");//线程的名字

        t1.start();
        t2.start();

        while (true) {
            //给主线程加了一个死循环，防止主线程退出
            //这样就可以在jconsole中查看到Main线程了
            System.out.println("main");
        }
    }
}
```
##### 使用Runnable创建线程
```java

//通过实现Runnable接口，创建线程
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getId() + "Value" + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t1 = new Thread(myRunnable);
        t1.start();
    }
}
```



##### jconsole的使用

> 目录：D:\SoftWare\Java\bin\jconsole.exe

> ![jconsole_connectThread1.jpg](jconsole_connectThread1.jpg)
> ![jconsole_connectThread2.jpg](jconsole_connectThread2.jpg)
> ![jconsole_connectThread3.jpg](jconsole_connectThread3.jpg)

##### java Thread的几个常见属性

| 属性     | 说明                          | 获取方法             |
|--------|-----------------------------|------------------|
| ID     | ID是线程的唯一标识，不同线程ID不会重复       | getId()          |
| 名称     | 线程名称在使用各种调试工具时可以用到          | getName()        |
| 状态     | 当前线程所处的一个情况                 | getState()       |
| 优先级    | 优先级高的线程更容易被调度到              | getPriority()    |
| 是否后台线程 | JVM会在一个进程的所有非后台线程结束后，才会结束运行 | isDaemon()       |
| 是否存活   | run方法是否运行结束了                | isActive()       |
| 是否被中断  | 线程中断问题                      |  isInterrupted() |



##### 线程是否存活
```java
public class Main {
    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            while (true) {
//                System.out.println("t1");
            }
        }, "线程1");

        Thread t2 = new Thread(() -> {
            while (true) {
//                System.out.println("t2");
            }
        }, "线程2");
        System.out.println("是否存活 " + t1.isAlive());
        t1.start();
        System.out.println("是否存活 " + t1.isAlive());


        t2.start();
        while (true) {
//            System.out.println("main");
        }
    }
}
```


##### 线程优先级
> 优先级高的线程，更容易被调度到，但并不绝对。
> 因为优先级只是"建议"性质的，不是强制性质的。
> 优先级高，意味着“建议”操作系统优先调度某线程。
> 但实际到底要不要优先调度，还是取决于操作系统。
```java


```

##### 前台线程和后台线程
> 线程分为前台线程和后台线程。
> JVM会在所有前台线程结束后，才会结束运行。
> 创建的线程默认是前台线程，main线程也是一个前台线程。
> Daemon表示一个后台进程(亦称守护进程)。
> isDaemon方法，可以判断，线程是前台进程还是后台进程。
> True是后台进程，False是前台进程。

##### try和catch的用法
```java
public class Main {

    public static void main(String[] args) {
        try {
            //try内部写的是你要执行的命令(该命令可能会发生异常)
            // 有可能引发异常的代码,10除以0
            int result = divide(10, 0);
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            //e是ArithmeticException实例化出来的一个对象
            //Arithmetic Exception 是Java编程语言中的一个异常类，用于表示算数运算错误
            //该异常通常在数学运算中出现问题时被抛出
            // 处理异常的代码
            System.err.println("Error: " + e.getMessage());
            System.out.println("Cause：" + e.getCause());
        }

        System.out.println("Program continues after exception handling.");
    }

    public static int divide(int numerator, int denominator) {
        return numerator / denominator; // 引发 ArithmeticException 异常
    }
}
```

##### 使用sleep休眠线程
```java
//使用runnable创建一个线程

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("开始休眠");
        try {
            long baseTime = System.currentTimeMillis();
            //线程休眠
            Thread.sleep(3000);
            long curTime = System.currentTimeMillis();
            long time = curTime - baseTime;
            System.out.println(time);
        } catch (InterruptedException e) {
            e.printStackTrace();
            //printStackTrace 打印异常堆栈跟踪信息
            throw new RuntimeException(e);
        }

    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t1 = new Thread(myRunnable);
        t1.start();
    }
}
```

##### runnable和thread的区别
> 使用“Runnable”接口的方式更加灵活，能够更好地支持代码组织和资源共享。
> 因此，在创建多线程应用程序时，推荐优先选择“Runnable”接口。
1. 继承关系
   * Thread是一个具体的类
   * Runnable是一个函数式接口，可以通过实现该接口的类来创建线程
2. 单继承VS多实现
   * 由于Java不支持多继承，如果选择继承Thread类，就不能再继承其他类。
   * 通过实现“Runnable”接口，你的类还可以继承其他的类，实现更灵活的代码组织。
3. 资源共享
   * 使用Thread类创建的线程，由于是直接继承自“Thread”，线程之间共享相同的实例变量，可能会引发一些并发访问的问题。
   * 使用“Runnable”接口创建的线程可以更容易地实现资源共享，因为你可以创建多个线程共享同一个“Runnable”实例。
4. 线程池
   * 通常，使用实现“Runnable”接口的方式，更适合在线程池中使用。线程池可以更好地管理和控制线程的生命周期。
