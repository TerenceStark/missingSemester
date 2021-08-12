# HW7Solution

## Author:金之航

## StuID:2183411101



## Problem

### 1.Sorts.py

```shell
python -m cProfile -s time sorts.py #按照执行时间排序
python -m cProfile -s time sorts.py | grep sorts.py
```

![image-20210812174943504](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210812174943504.png)

```shell
pip3 install line_profile #安装line_profiler
kernprof -l -v sorts.py #为需要分析的函数添加装饰器 @profile，并执行

pip3 install memory_profiler #安装memory_profiler
python3 -m memory_profiler sorts.py #为需要分析的函数添加装饰器 @profile，并执行
```

![image-20210812184739325](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210812184739325.png)

![image-20210812184951326](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210812184951326.png)

插入排序的内存效率略好于快速排序，因为快速排序需要一些额外的空间来保存结果，而插入排序则是原地操作，但是耗时更高。



### 2.fibonacci

```shell
pip3 install pycallgraph
pip3 install graphviz
pycallgraph graphviz -- ./fibonacci.py
```

![1.png](https://missing-semester-cn.github.io/missing-notes-and-solutions/2020/solutions/images/7/5.png)



### 3.kill http.server

```shell
python -m http.server 4444
lsof | grep LISTEN
```

![image-20210812185535348](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210812185535348.png)

![image-20210812185526011](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210812185526011.png)



### 4.stress -c 3  

```shell
stress -c 3  
taskset --cpu-list 0,2 stress -c 3
man taskset #taskset 命令可以将任务绑定到指定CPU核心
```

