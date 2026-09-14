# How to switch b/w ring 0 and ring 3
* int - interupt instruction
* int,syscenter or syscall instruction set CPL to 0; Change to KERNEL_CS and KERNEL_DS Segments
* sysenter is slower than syscall | 64 bit arch syscall is faster


## System Call

* One and only way for user application to enter the kernel to request OS services and priveledged opearations such as accessing th hardware
    ** A layer between the hardware and user-space processes.

* Process management/scheduling: fork, ecit, execve, nice, {get | set} priority, {get| set}pid
* Memory management: brk, mmap
* FILE SYSTEM: OPEN, READ, WRITE,LSEEK,STAT
* Inter-Process COmmunication: pipe, scmget
* Time management: {get | set}timeofday
* Others: {get|set}timeofday
* Others: {get|set}uid, connect

# Q: Where are system call implementations in linux Kernel?

* The syscall table for x86_64 architecture
   ** linux/arch/x86/entry/syscall/syscall_64.tbl
  ** 64-bit system call numbers and entry vectors

  <number
*


# Transition: user space -> kernel Space

* x86 instruction to invoke a system call
    * int $x80: raise asoftware interupt 128 (old)
    * sysenter: fast system call (x86_32)
    * syscall: fast system call (x86_64)
* Passing a syscall ID and parameters
    * syscall ID: %rax
    * parameters (x86_64): rdi, rsi, rdx,r10,r8 and r9
 
# Invoking a syscall
* x86_64 architecture has a syscall function

.data

msg:
 .ascii "Hello world!\n"
 len = . - msg
.text
  .global _start

_start:
   mov $1, %rax    # System id: write
   mov $1, %rdi    # 1st arg: fd (Standard output)
   mov $msg, %rsi  # 2nd arg: msg
   mov $len, %rdx  # 3rd arg: length of msg
   syscall         # switch from user space to kernel space

   mov $60, %rax   # syscall id: exit
   xor %rdi, %rdi  # 1st arg: 0
   syscall         # switch from user space to kernel space


# Hanlding the syscall interupt
* The kernel syscall interupt jandler,, system call handler
    * entry_SYSCALL_64 invokes the entry function for the syscall ID
         * call do_syscall_64

# Transition: kernel spcae -> user spcae


# sys_gettimeofday

/* linux/kernel/time/time.c */



SYSCALL_DEFINE2(gettimeofday,struct timeval __user *, tv, struct timezone __user *,tz) /* __user: user-space address */
{
   if(likely(tv!=NULL




      



