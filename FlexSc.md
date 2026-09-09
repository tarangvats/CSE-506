# System Call


# Hardware isolation in x86 (aka ring)

* Four priveledge levels
* Two levels, App(Ring 3) and

# Current oriveledge level (CPL) in x86

* Only system call can change the values (need spec)
* Two bits in the code selector (CS) register indicate the current priveledge level (CPL) of a program running on a CPU
* Priviledged Mode
  * Ring - <-> CPL=0

# What does "ring 0" protect ?

* Certain registers can be accessed/modified only if CPL ==0
  * FLAGS register: IF (interrupt), IOPL (I/O) etc.
  * Control register
 
# How to sswitch b/w ring 0 to ring 3

# System Call
 * One and only way for user space application to enter the kernel to request OS services and priviledged operation such as accessing the hardware
    * A layer between the hardware and user-space processes
    * An abstract hardware interface for user-space
    * Ensure system security and stability
