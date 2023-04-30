# #  Thread Creation

```python
import threading

def worker():
    # Perform some work
    print("Worker thread executing...")

def main():
    # Create a new thread
    t = threading.Thread(target=worker)
    # Start the thread
    t.start()
    # Wait for the thread to complete
    t.join()

if __name__ == '__main__':
    main()

```


##  Pass Argument

```python
import threading

def worker(name):
    # Perform some work
    print(f"{name} thread executing...")

def main():
    # Create a new thread and pass an argument
    t = threading.Thread(target=worker, args=("Worker",))
    # Start the thread
    t.start()
    # Wait for the thread to complete
    t.join()

if __name__ == '__main__':
    main()
```


## Lock/ Synchronization

```python
import threading

counter = 0
lock = threading.Lock()

def worker():
    global counter
    # Acquire the lock
    lock.acquire()
    # Increment the counter
    counter += 1
    # Release the lock
    lock.release()

def main():
    # Create multiple worker threads
    threads = []
    for i in range(10):
        thread = threading.Thread(target=worker)
        thread.start()
        threads.append(thread)
    # Wait for all threads to complete
    for thread in threads:
        thread.join()
    # Print the final value of the counter
    print(f"Counter value: {counter}")

if __name__ == '__main__':
    main()
```









## Event/Synchronization

```python
import threading

event = threading.Event()

def worker():
    # Wait for the event to be set
    event.wait()
    # Perform some work
    print("Worker thread executing...")

def main():
    # Create a new thread
    t = threading.Thread(target=worker)
    # Start the thread
    t.start()
    # Set the event
    event.set()
    # Wait for the thread to complete
    t.join()

if __name__ == '__main__':
    main()
```








##  ThreadPool  Executor

```python
import concurrent.futures

def process_data(data):
    # Perform some time-consuming calculations on data
    ...

def main():
    data = load_large_dataset()
    with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
        # Assign each item in the dataset to a thread in the pool
        futures = [executor.submit(process_data, item) for item in data]
        # Wait for all tasks to complete
        concurrent.futures.wait(futures)

if __name__ == '__main__':
    main()

```





## Modified Above Code
```python
import threading

def process_data(data):
    # Perform some time-consuming calculations on data
    ...

def main():
    data = load_large_dataset()
    threads = []
    for item in data:
        # Create a new thread for each item in the dataset
        thread = threading.Thread(target=process_data, args=(item,))
        threads.append(thread)
        thread.start()
    # Wait for all threads to complete
    for thread in threads:
        thread.join()

if __name__ == '__main__':
    main()

```


## Queue / Thread Communication


```python

import threading
import queue

q = queue.Queue()

def producer():
    for i in range(10):
        # Produce an item and put it in the queue
        item = f"Item {i}"
        q.put(item)
        print(f"Produced {item}")
    # Put a sentinel value in the queue to signal the consumer to stop
    q.put(None)

def consumer():
    while True:
        # Get an item from the queue
        item = q.get()
        if item is None:
            break
        # Consume the item
        print(f"Consumed {item}")

def main():
    # Create and start the producer and consumer threads
    t1 = threading.Thread(target=producer)
    t2 = threading.Thread(target=consumer)
    t1.start()
    t2.start()
    # Wait for the producer and consumer threads to complete
    t1.join()
    t2.join()

if __name__ == '__main__':
    main()
```







## Thread Safety / Aromic Operation

```python
import threading

counter = 0

def worker():
    global counter
    # Perform an atomic increment operation
    with threading.Lock():
        counter += 1

def main():
    # Create multiple worker threads
    threads = []
    for i in range(10):
        thread = threading.Thread(target=worker)
        thread.start()
        threads.append(thread)
    # Wait for all threads to complete
    for thread in threads:
        thread.join()
    # Print the final value of the counter
    print(f"Counter value: {counter}")

if __name__ == '__main__':
    main()
```







##  Daemon Thread

```python
import threading
import time

def worker():
    while True:
        # Perform some work
        print("Worker thread executing...")
        time.sleep(1)

def main():
    # Create a daemon thread
    t = threading.Thread(target=worker, daemon=True)
    # Start the thread
    t.start()
    # Wait for user input to exit the program
    input("Press enter to exit...\n")

if __name__ == '__main__':
    main()
```





## Thread Local Data

```python
import threading

local_data = threading.local()

def worker(name):
    # Set some thread local data
    local_data.name = name
    # Access the thread local data
    print(f"{local_data.name} thread executing...")

def main():
    # Create multiple worker threads
    threads = []
    for i in range(3):
        thread = threading.Thread(target=worker, args=(f"Worker {i}",))
        thread.start()
        threads.append(thread)
    # Wait for all threads to complete
    for thread in threads:
        thread.join()

if __name__ == '__main__':
    main()

```






## RLocks/ Deadlock Prevent

```python

import threading

lock1 = threading.Lock()
lock2 = threading.Lock()

def worker1():
    # Acquire lock1
    lock1.acquire()
    print("Worker 1 acquired lock1")
    # Acquire lock2
    lock2.acquire()
    print("Worker 1 acquired lock2")
    # Release lock2
    lock2.release()
    print("Worker 1 released lock2")
    # Release lock1
    lock1.release()
    print("Worker 1 released lock1")

def worker2():
    # Acquire lock2
    lock2.acquire()
    print("Worker 2 acquired lock2")
    # Acquire lock1
    lock1.acquire()
    print("Worker 2 acquired lock1")
    # Release lock1
    lock1.release()
    print("Worker 2 released lock1")
    # Release lock2
    lock2.release()
    print("Worker 2 released lock2")

def main():
    # Create and start the worker threads
    t1 = threading.Thread(target=worker1)
    t2 = threading.Thread(target=worker2)
    t1.start()
    t2.start()
    # Wait for the threads to complete
    t1.join()
    t2.join()

if __name__ == '__main__':
    main()
```