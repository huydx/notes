Some notes (copy-paste included) from https://pages.cs.wisc.edu/~remzi/OSTEP/.

# Concurrency, OS perspective


## Multi threaded program

A multi-threaded program has more than
one point of execution (i.e., *multiple PCs*, each of which is being fetched
and executed from).

Each thread has its own private set of registers it
uses for computation; thus, if there are two threads that are running on
a single processor, when switching from running one (T1) to running the
other (T2), a context switch must take place. The context switch between
threads is quite similar to the context switch between processes, as the
register state of T1 must be saved and the register state of T2 restored
before running T2. With processes, we saved state to a process control
block (PCB); now, we’ll need one or more thread control blocks (TCBs)
to store the state of each thread of a process.


There is one major difference, though, in the context switch we 
perform between threads as compared to processes: the address space remains the same 
(i.e., there is no need to switch which page table we are using).
One other major difference between threads and processes concerns
the stack.

Multi-threaded address spaces, has multiple stack which is different to single threaded
program which has a single stack usually residing at the bottom of the address space.
Multi-threaded program has thread-local storage (stack of relevant thread)


Program Code
   ||
  Heap 
   ||
  Free
   ||
  Stack(2)
   ||
  Free
   ||
  Stack(1)


## Pthread primitive:

The name that the POSIX library uses for a lock is a mutex, as it is used
to provide mutual exclusion between threads

```
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
Pthread_mutex_lock(&lock);
// race code
Pthread_mutex_unlock(&lock);
```

## Few notes on concurrency bugs

Why do deadlock occur?
Main cause includes highly complex system, where locks are not easy to controlled to be grabbed 
in order. Another reason is due to nature of *encapsulation*, where programs were intentionally 
written in a way that hide inner complexity, includes lock mechanism.

Tricks
- Use address order (by comparing pointer)

```
if (m1 > m2) { // grab in high-to-low address order
  pthread_mutex_lock(m1);
  pthread_mutex_lock(m2);
} else {
  pthread_mutex_lock(m2);
  pthread_mutex_lock(m1);
}
// Code assumes that m1 != m2 (not the same lock)
```

# Event based concurrency

Used in GUI based application, or recently node.js. 
The idea is: 
you simply wait for something (i.e., an “event”) to occur; when it does, you check what type of
event it is and do the small amount of work it requires 
(which may include issuing I/O requests, or scheduling other events for future handling, etc.). 

```
while (1) {
  events = getEvents();
  for (e in events)
    processEvent(e);
}
```

Sample code using poll

```
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>
#include <sys/types.h>
#include <unistd.h>

int main(void) {
  // open and set up a bunch of sockets (not shown)
  // main loop
  while (1) {
    // initialize the fd_set to all zero
    fd_set readFDs;
    FD_ZERO(&readFDs);

    // now set the bits for the descriptors
    // this server is interested in
    // (for simplicity, all of them from min to max)
    int fd;
    for (fd = minFD; fd < maxFD; fd++)
      FD_SET(fd, &readFDs);

    // do the select
    int rc = select(maxFD+1, &readFDs, NULL, NULL, NULL);

    // check which actually have data using FD_ISSET()
    int fd;
    for (fd = minFD; fd < maxFD; fd++)
      if (FD_ISSET(fd, &readFDs))
        processFD(fd);
  }
}
```

With an event-based approach, however, there are no other threads to
run: just the main event loop. We thus have a rule that must be
obeyed in event-based systems: *no blocking calls are allowed*.
To overcome this limit, many modern operating systems have 
introduced new ways to issue I/O requests to the disk system, referred to
generically as asynchronous I/O. 
