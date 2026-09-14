# Improving system call performance

* Software: vDSO (virtual dynamically linked shared object)
    * A kernel mechanism for exporting a kernel space routines to user space applications
    * No context sitching overhead
    * e.g, gettimeofday()
         * the kernel allows the page cotaining the current time to be mapped read-only into user space
Instead of making system call we are just reading from vdso

Example C Code:

struct timeval tv;
int ret;

ret = gettimeofday(&tv

* Software: FlexSC: Exception-less system-call

# Motivation

The synchronous system call interface iis a legacy from the single core era


Expensive! Cost are:
* Direct Call: mode switch
* Indirect Call: processor strcuture pollution (cache miss)

FlexSC implements efficient and flexible system calls for the multicore era

# Perrformance impact of synchronous syscalls

* Xalan from SPEC CPU 2006
   * Virtually no time in the OS
* Linux on Intel Core i7 (Nehalem)
* Injected exceptions with varying frequencies:
   * Direct: emulate null system
   * Indirect : emulate "write()" system call
* Measured only user-mode time
    * Kernel time ignored
 
Ideally, user-mode performance is unaltered

# Degradation due to sync. syscalls

* System calls can half processor efficiency; indirect cause is major comtributor.
* MySQL (less syscall heavy than pache)
      * MYSQL makes syscall every 10k instructions

  Delta between sysnull and syswrite implies indirect cost.


SYstem calls can half process efficiency.


Based on graph indirect cost is very significant.


# Processor state pollution

Key source of performance impact

* On a Linux write() call:
    *  up to 2/3rd of the L1 data cache and data TLB are evicted by kernel code.
* Kernel performance equally affected
    * processor efficiency for OS code is also cut in half.
  
  
