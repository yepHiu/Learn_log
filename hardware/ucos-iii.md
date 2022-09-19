# UCOS-III使用笔记
(个人参考)

## 简介 
- 本文档用于记录本人UCOS-III使用过程中遇到的问题

### 关于延时问题
- 注意在UCOS的使用过程中，系统的延时是不可以出现在系统任务里


### 任务初始化
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

### 延时使用问题
- 在UCOS-III中延时的使用不可以再像未搭载实时操作系统的情况下随意的使用延时函数，例如下面的使用systick的延时将不可以使用
    ```c

    ```

### 中断配置问题
