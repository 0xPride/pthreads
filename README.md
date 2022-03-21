# Posix Threads

A thread is a mechanism that allow an application to perform multiple tasks concurrently.
A single process can contain multiple threads. All of the threads are independently executing
the same program and they all share the same global memory including the initialized data,
uninitialized data and heap memory segments

![image](img/four-threads-executing-in-a-process.png)

## Threads and Errno

in the traditional UNIX API, errno is a global variable. However this doesn't suffice for threaded programs,
if a thread made a function call that returned an error in global errno variable, then this would confuse other
threads that might also be making function calls to check errno, therefore in threaded programs each thread has
its own errno value.

## Return value

All pthreads function return 0 on success or positive integer on failure. the failure value is the same
value that can be placed in errno

## Compiling Pthreads prograns

On linux, programs that use pthread api must compile with -pthread option the effect if this option 
include the following:

- the _REENTRANT preprocessor macro is defined this cause the  declaration of a few reentrant function to be 
exposed.

- the program is linked with the *libpthread* library (-lpthread)

## Thread creation

```c
#include <pthread.h>

int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
 void *(*start)(void *), void *arg);
```

When a program is started the resulting process consists of a single thread called the initial or main thread,
the pthread_function create new thread. The new thread begin execution by calling the function identified by start
with the argument arg ( start(arg) ). the thread that calls pthread_create continue execution.
The arg is declared as void * meaning that we can pass a pointer to any type of object to the start function.
