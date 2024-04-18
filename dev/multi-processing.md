并发(concurrency): 单 CPU 上下文切换
并行(parallelism): 在多核 CPU 上同时运行多个线程

# Thread
```python
import threading

# lock
lock = threading.Lock()
lock.acquire()

# semaphore
semaphore = threading.Semaphore(2)
semaphore.acquire()

# event
event = threading.Event()
event.wait()
event.set() 

# condition
condition = threading.Condition()
condition.acquire()
condition.wait()
condition.notify()

# barrier
barrier = threading.Barrier(2)
barrier.wait()
```

# Process
创建子进程的计算成本非常高，因此，当想要创建大量进程时，创建进程池会更有效

`Process` 提供了类比 `fork` 的功能
```python
import multiprocessing
p = multiprocessing.Pool(4)
p.map(f, [1, 2, 3])
```

进程间通信
```python
import multiprocessing

# Queue
queue = multiprocessing.Queue()

# Pipe
parent_conn, child_conn = multiprocessing.Pipe()

# Shared Memory
shared_memory = multiprocessing.Value('i', 0)
shared_memory = multiprocessing.Array('i', 10)
shared_memory = multiprocessing.sharememeory.SharedMemory(name='name', create=True, size=1024)

# Manager
manager = multiprocessing.Manager()
manager.dict()
manager.Namespace()
manager = multiprocessing.managers.SharedMemoryManager()
```

## Distributed Process
```python
class QueueManager(BaseManager):
    pass

QueueManager.register('get_queue', callable=lambda: queue)
manager = QueueManager(address=('', 5000), authkey=b'abc')
```

### RPC
### Redis
1. pickle function with argument, send them into queue.
2. workers listening to the queue will execute.
```python
import redis
from rq import Queue
```

# Subprocess
`Subprocess` 提供了类比 `exec` 的功能
```python
subprocess.run(['ls', '-l'])
subprocess.Popen(['ls', '-l'], stdout=subprocess.PIPE, stdin=p.stdout, stderr=subprocess.PIPE)
```
通过设置 PIPE 可以实现运行时通信

# Asyncio
Async/Await 定义了协程的语法(python), 定义了 Promise 的语法(js).

他解决了同一线程内的并发问题，不需要新建子线程去调度

`await` 声明了当前语句可以被挂起

[ref](https://blog.csdn.net/rhx_qiuzhi/article/details/124332114)

# 协程
协程的本质是单线程（单核）切换/保存状态（上下文），不涉及多核并行，仅并发

> 所以，理论上可以通过函数内反复调用 api，实现类似协程的控制流来回跳转。语法糖改变函数的调用顺序本质上也是协程了

### 生成器
```python
def gen():
    for i in range(10):
        yield i

for i in gen():
    print(i)
```

这样，for 每循环一轮，gen 函数会接着上一个 yield 的地方继续执行