# How to switch b/w ring 0 and ring 3
* int - interupt instruction
* int,syscenter or syscall instruction set CPL to 0; Change to KERNEL_CS and KERNEL_DS Segments
* sysenter is slower than syscall | 64 bit arch syscall is faster


## System Call

* One and only way for user application to enter the kernel to request OS services and priveledged opearations such as accessing th hardware
  ** A layer between the hardware and user-space processes.

* 
