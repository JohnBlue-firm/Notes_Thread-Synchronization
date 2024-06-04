
# Threads
- Threads are ***individual sequences of execution within a process***. They allow programs to perform multiple tasks concurrently.
- In simpler terms, a thread is like a lightweight process. A single process can have multiple threads, and these threads can run concurrently, sharing the same memory space.

# Synchronization
- Synchronization is ***the coordination of multiple threads*** to ensure that they execute in a desired order and that they don't interfere with each other's operations.
- Common synchronization mechanisms include locks ***mutexe, semaphore, monitor, and barrier***. These mechanisms help control access to shared resources, ensuring that only one thread can access a resource at a time or enforcing certain ordering constraints.

extra hint:
***semaphore*** is the basic and the most common implement of synchronization. It's like a flag with counter, and it will use wait() and signal() to perform blocking.
***semaphore*** doesn't has lock owner.

extra hint:
***spinlock*** will keep trying to get the lock (when counter is 1) without sleeping, and the modern linux will apply ticket policy to prevent starving.
***spinlock*** has lock own and the lock should be unlock by the same owner.
when using ***spinlock*** in to design of kernel, it is often cruicial to consider for preventing deadlock or not able to unlock: interrupt (better not to used in ISR) / sleep function (not to use sleep, spinlock aim to protect something that have to be done before sleep)
***For optimization***, we can use MCS spinlock for reducing context switch and still maintain ticket policy (lock only queued and spin on it own cacheline).
```
while (atomic_dec(&lock->counter != 0))	{
    while	(lock->counter <= 0) {
        // Pause the CPU until some coherence	
        // two loop two for consideration of cache line effect
    }	
}
```

extra hint:
***mutex*** is late introduced and could be the complicatest lock in Linux. It will use lock() and unlock() to perform blocking.
***mutex*** has lock own and the lock should be unlock by the same owner.

# Possible Issues
- Deadlock: two or more threads are using the same two or more resource, and will meet deadlock when neither of thread can get sufficient resources to work (and no thread releases the resouces).
```
fun1 ()	{
    lock(&A) {
        lock(&B) { }
    }	
}
fun2 ()	{
    lock(&B) {
        lock(&A) { }
    }	
}
```
- Livelock: two or more threads are using the same two or more resource, and will meet livelock when neither of thread can get sufficient resources to work (but all the threads are willing to release the resouces).
```
fun1 ()	{
    lock(&A)
    if (try_lock(&B) == true) {}
    else { release(&A) }
}
fun2 ()	{
    lock(&B)
    if (try_lock(&A) == true) {}
    else { release(&B) }
}
```
- Starving: some threads are starving.

----

# Library for implementations
- Kthread: the kernel thread that handle the user thread and directly execute with the hardware resources.
- Kernel library: the library for developing kernel including kthread. usually, the library header look like <linux/xxx.h>.
- Pthread: it is an interface specification that define api for engineer to develop user thread. The library is <pthread.h>.

extra hint:
In modern computer system, machine will provide atomic operation to support synchronization primitives such as spinlock.

extra hint:
Pthread also has spinlock, but no need to worry interrupt and ISR, since these are handle by the Kernel.

extra hint:
Pthread have implemented conditional variables, but Linux kernel doesn't have.

----

# Background hints
- Kernel is operation system. It's a group of program that provides essential services and functions to manage hardware resources and facilitate communication between software and hardware components. \
  Appilcation(User) <-> Kernel <-> Hardware Resource(CPU, Memory, ...)
- Program: the code
- Proccess: the entity of the code
- User mode VS Kernel mode: CPU will check which mode the section in the program belong, and decide whether the section have sufficient authencation to execute (the hardware).
- User space VS Kernel space: the memory or code section that can be operated in User mode or Kernel mode.

# Reference
The following are some materials for deep understanding of synchronization in (linuux) kernel

https://hackmd.io/@owlfox/SyVVY3EgI/https%3A%2F%2Fhackmd.io%2Fs%2FSJpp-bN0m#QueuedMCS-Spinlock

https://blog.louie.lu/2016/10/22/mutex-semaphore-the-difference-and-linux-kernel/

https://hackmd.io/@RinHizakura/rJhEpdyNw

https://www.cntofu.com/book/46/linux_device_driver_programming/tong_bu_yu_suo_ding.md#google_vignette
