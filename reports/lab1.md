# chapter3练习
## 编程作业
1. 在TCB中添加字段syscall_times和start_time字段记录task开始时间和系统调用次数
2. 为TaskManager添加了函数用于设置task开始时间和更新task系统调用次数
3. 在适当位置调用2中函数更新对应值
4. 在TaskManager添加函数返回当前进程的开始时间和系统调用次数
5. 调用4中接口实现sys_task_info函数，完成实验。
## 简答作业
### 1
RustSBI version 0.3.0-alpha.4, adapting to RISC-V SBI v1.0.0  
程序输出如下：
```
[kernel] Loading app_0
[kernel] PageFault in application, kernel killed it.
[kernel] Loading app_1
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] Loading app_2
[kernel] IllegalInstruction in application, kernel killed it.
```
### 2
1.  内核栈栈顶   
    应用场景：
    1. 从栈上的trap上下文恢复
    2. 在内核栈上压入一个 Trap 上下文
2. sstatus和spec，sscratch
    * sstatus的`ssp`等字段给出 Trap 发生之前 CPU 处在哪个特权级（S/U）等信息，需要正确恢复到用户态
    * sepc给出当Trap 是一个异常的时候，记录 Trap 发生之前执行的最后一条指令的地址,他决定了进入用户态后开始执行的指令地址
    * sscratch保存的是用户栈指针，确保进入用户态后栈也被正确恢复
3. x2是sp后面会单独处理，x4没有被使用
4. sp->user stack; sscratch->kernel stack 
5. sret  
因为sstatus的被恢复了，sret检查相应字段后系统将会进入用户态，pc被设置为sepc，进入用户态继续执行程序
6. sp->kernel stack; sscratch->user stack
7. 在执行__alltraps代码之前cpu硬件完成。

## 荣誉准则
荣誉准则


1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容:  
和gpt交流了实验框架代码中的函数和结构体字段的解释说明和rust的语法问题。

2. 此外，我也参考了以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容:  
无，代码均独立完成，没有使用ai参与编码。

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。