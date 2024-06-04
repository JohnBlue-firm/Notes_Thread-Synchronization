
# Threads
- Threads are ***individual sequences of execution within a process***. They allow programs to perform multiple tasks concurrently.
- In simpler terms, a thread is like a lightweight process. A single process can have multiple threads, and these threads can run concurrently, sharing the same memory space.

# Synchronization
- Synchronization is ***the coordination of multiple threads*** to ensure that they execute in a desired order and that they don't interfere with each other's operations.
- Common synchronization mechanisms include locks ***mutexes, semaphore, monitor, and barrier***. These mechanisms help control access to shared resources, ensuring that only one thread can access a resource at a time or enforcing certain ordering constraints.

# Library for implementations
- Kthread: the kernel thread that handle the user thread and directly execute with the hardware resources.
- Kernel library: the library for developing kernel including kthread. usually, the library header look like <linux/xxx.h>.
- Pthread: it is an interface specification that define api for engineer to develop user thread. The library is <pthread.h>.
extra hint: Pthread有spinlock, 但是不用擔心ISR的問題, 這由kernel處理.
extra hint: The Linux kernel does not implement conditional variables.

# Extra back ground hints
- Kernel is operation system. It's a group of program that provides essential services and functions to manage hardware resources and facilitate communication between software and hardware components. \
  Appilcation(User) <-> Kernel <-> Hardware Resource(CPU, Memory, ...)
- Program: the code
- Proccess: the entity of the code
- User mode VS Kernel mode: CPU will check which mode the section in the program belong, and decide whether the section have sufficient authencation to execute (the hardware).
- User space VS Kernel space: the memory or code section that can be operated in User mode or Kernel mode.

