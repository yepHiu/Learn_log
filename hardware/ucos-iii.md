# UCOS-III使用笔记
(个人参考)

## 文档简介 
- 本文档用于记录本人UCOS-III使用过程中遇到的问题

## UCOS-III简介
- UCOS-III是一个抢占式内核，意味着uCOS-III总是执行醉重要的就绪任务
















































### 任务管理
#### 启动UCOS-III
- 调用```OSInit() ```初始化UCOSIII
- 在调用OSTaskCreate()函数创建任务的时候一定要调用``OS_CRITICAL_ENTER()`` 任务创建完成后要调用`OS_CRITICAL_EXIT()`退出临界区
- 最后调用`OSStart()`启动UCOS-III
#### 任务状态
- UCOS不适用于多核CPU,每次只能有一个任务进入运行状态  
- 在UCOS中任务有多种运行状态
  - `休眠态`:只是以任务函数的形式存在但是不接受UCOS的管理也就是说这这个状态下他只是一串代码
  - `就绪态`:任务就绪,等待获取CPU使用权
  - `运行态`:顾名思义
  - `等待态`:任务需要等待某一个事件才能进行,此时任务暂时让出CPU使用权
  - `中断服务态`:正在执行的任务被终端打断,CPU转而执行中断服务程序,此时任务被挂起

#### 同优先级上有多个就绪的任务
- 
  


#### 任务创建声明
- 这部分是针对UCOS任务创建的指南
    ```c
    //任务声明
    #define START_TASK_PRIO 3 //任务优先级
    #define START_STK_SIZE 1024 //任务堆栈大小
    OS_TCB StartTaskTCB; //声明任务控制块
    CPU_STK START_TASK_STK[START_STK_SIZE];//声明任务
    void start_task(void *p_arg);//指向任务函数

    //这里是自己声明的任务函数
    void start_task(void *p_arg)
    {
        //函数
    }
  ```
#### 任务创建问题
- 在UCOS-III中需要通过调用函数OSTaskCreate()创建任务
  ```c
    //任务创建
    OSTaskCreate(
      (OS_TCB *)&TEST_TaskTCB,      //任务控制块
      (CPU_CHAR *)"test_task",      //任务名字
      (OS_TASK_PTR)test_task,       //任务函数
      (void *)0,                    //传递给任务的参数
      (OS_PRIO)TEST_TASK_PRIO,      //任务优先级别
      (CPU_STK *)&TEST_TASK_STK[0], //栈基地址
      (CPU_STK_SIZE)TEST_STK_SIZE / 10,
      (CPU_STK_SIZE)TEST_STK_SIZE,
      (OS_MSG_QTY)0,
      (OS_TICK)0,
      (void *)0,
      (OS_OPT)OS_OPT_TASK_STK_CHK | OS_OPT_TASK_STK_CLR,
      (OS_ERR *)&err);
    ```
#### 任务函数讲解
- 任务函数的特点： 
  - ```无限循环``` 
  - ```不允许有返回值```
  - ``任务可以访问全局的变量``
  - ``可以访问一个或者多个IO设备``
- 当任务在运行时候，任务会占用实际的CPU


#### 任务API函数
- `OSTaskcreate()`任务创建函数





### 关于临界段问题
#### 临界段介绍：
- 临界段是一段不可以分割的代码，在临界段中需要关闭中断去保护临界段代码，(**注意在临界段中是不允许有系统延时函数的**)
  ```c
  //UCOS-III进入和退出临界段代码
  OS_CRITICAL_ENTER()//进入临界段
  OS_CRITICAL_EXIT()//退出临界段
  OS_CRITICAL_EXIT_NO_SCHED
  ```



### 延时使用问题
- 在UCOS-III中延时的使用不可以再像未搭载实时操作系统的情况下随意的使用延时函数
- 在任务中的延时函数
  ```c
    
  ```

### 中断配置问题
