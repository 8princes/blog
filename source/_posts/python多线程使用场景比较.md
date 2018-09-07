---
title: python多线程使用场景比较
date: 2018-09-06 20:57:55
tags: 
- python

---

## python中的多线程

> 多线程即在一个进程中启动多个线程执行任务。一般来说使用多线程可以达到并行的目的。

<!-- more -->

> 但是，由于***python***中使用了全局解释锁**GIL**（global interpreter lock）的概念，在用cpu执行计算任务的时候，**GIL**锁不会被释放，***python***多线程其实还是使用的单核在进行cpu计算。一个cpu时间片只会分给一个线程，因此，cpu密集型的情况下，多线程并不会加快计算速度。

> 而***python***在io阻塞的情况下，会释放**GIL**锁，其他线程会在当前线程等待返回值（阻塞）的情况下继续执行发送请求（output），第三个线程又会在第二个线程等待返回值（阻塞）的情况下发送请求（output），即在同一时间片段，会有一个线程在等待数据，也会有一个线程在发数据。这就减少了io传输的时间。


## CPU密集型任务

### 定义一个CPU计算密集型任务

~~~python
def task_cpu(task_id):
    print("CPU task[%s] start" % task_id)
    for i in range(100):
        count = 0
        for i in range(10000):
            count += pow(3*2, 3*2)
    print("CPU task[%s] end" % task_id)
    return task_id
~~~

### 顺序执行10次

~~~python
def sync_compute():
    for i in range(10):
        task_cpu(i)
s=time.time()
sync_compute()
print(time.time()-s)
~~~

总耗时：***4.718312740325928***
### 多线程执行

~~~ python
def threads_compute():
    threads = []
    for i in range(10):
        threads.append(threading.Thread(target=task_cpu,args=(i,)))
    for t in threads:
        t.start()
    for t in threads:
        t.join()
# %timeit async_request()
import time
s=time.time()
threads_compute()
print(time.time()-s)
~~~

总耗时：***4.946189880371094***

多线程执行CPU密集型任务甚至比顺序执行还费时了一点点。这是由于线程切换时间导致的。可见，***python***多线程不适合执行CPU密集型任务。

## IO密集型任务

### 定义一个IO计算密集型任务

~~~python
import requests,time
def task_io(t_id):
    print('io task[{}] start'.format(t_id))
    requests.get('http://0.0.0.0:8000/example/test')
    print('io task[{}]  end'.format(t_id))
~~~

这里用django框架简单定义了一个http接口，接到请求后等待2秒。代码如下：

~~~python
class TestView(APIView):
    def get(self, request):
        time.sleep(2)
        return json_resp(code=200)
~~~

### 顺序执行10次

~~~python
import requests
def sync_io():
    for i in range(10):
        task_io(i)
        
# %timeit sync_request()
import time
s=time.time()
sync_io()
print(time.time()-s)
~~~

总耗时：***20.233391761779785***

### 多线程执行

~~~ python
import threading

def threads_request():
    threads = []
    for i in range(10):
        threads.append(threading.Thread(target=task_io,args=(i,)))
    for t in threads:
        t.start()
    for t in threads:
        t.join()
# %timeit async_request()
import time
s=time.time()
threads_request()
print(time.time()-s)
~~~
总耗时：***2.036729097366333***

相比较下，多线程执行这个IO密集型任务速度提高了近10倍。

#### 结果证明，python多线程更适合IO密集型任务。


