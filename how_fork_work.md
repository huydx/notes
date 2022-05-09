# How fork works

Pretty easy to understand notes here

https://www.csl.mtu.edu/cs4411.ck/www/NOTES/process/fork/create.html#:~:text=System%20call%20fork()%20is,the%20fork()%20system%20call

The API looks simple:
- After `fork` called, OS will copy the current process's address space to child process 
- Both processes start their execution right after the system call fork()
- Other address spaces created by fork() calls will not be affected even though they have identical variable names.

Even the explanation looks simple, from the code perspective it looks pretty weird since only the codes that happen **after** the `fork` syscall will be executed afterward.


```
#include <stdio.h>
#include <unistd.h>
#include <string.h>

#define MAX_COUNT 200
#define BUF_SIZE 100

int main(void) {
    pid_t pid;
    int i;
    char buf[BUF_SIZE];
    fork();
    pid = getpid(); //<--- here is what will be executed afterward in both child proccess and parent process
    for (i = 0; i < MAX_COUNT; i++) {
        sprintf(buf, "this line from pid %d, value = %d\n", pid, i);
        write(1, buf, strlen(buf));
    }
}
```

In most cases, the fork model requires that both parent process and child process has the same logic, but if it's required we could check which is parent process using return pid_t returned from `fork`

```
pid = fork();
if (pid == 0) {
  // is child process 
} else {
  // is parent process
}
```

The if/else check makes code looks ugly though.

