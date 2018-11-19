# 文件IO
应用层API->内核层API->驱动层API

守护进程：缓输出

|          FILE          |
| :--------------------: |
|         iNode          |
|         f-pos          |
| buffer (8192Byte in C) |


何时刷新buffer？

1.满了

2.fflush()

3.文件正常关闭

PCB 定义在：sched.h task_struct

记录当前进程的所有状态