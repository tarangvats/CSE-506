# Operating System Design
* OS Design
  * What kind of system calls?
  * What abstractions?
* OS architectures 
  * Monolithic Kernel (traditional)
  * Microkernel
  * Exokernel (today's paper)

# Monolithic Kernel
1. one big program
2. e,g. Unix,Linux, Windows
3. Big abstractions

## Big abstraction: Processing
* Kernel is in charge of scheduling
  * Scheduling is hidden from process
* Kernel provides each process with its own virtual CPU
  * On context switch,
    * Save all registers of old process
    * Restore all registers for new process

## Big abstraction: Virtual Memory
* Kernel is in charge of memory management
  * Physical memory, MMU etc are hidden from a process
* Optimizations
  * Lazy page table fill
  * copy-on-write fork
  * Demand Paging
  * Share physical memory for executables and libraries
## Big abstraction: File and directories
* Kernel is in charge of file management,disk layout,etc.
* Simple interface: file descriptors for I/O
   * no specialized I/O
* Permissions per a file or a directory

## Monolithic Kernel: Pros and Cons
* Pros
  * easy for subsystems to cooperate
  * all codes run with kernel priveledges
* Cons
  * complex
  * 3rd party device drivers
  * too big abstraction (?)

## Too Big Abstraction
* Abstraction may be too big/general than needed -> slow
* Context switch
    1. Process may know it does not use some(e.g, floating-point) regs
    2. kernel (blindly)save/restore all registers
* Disk Layout
   1. Database may want to lay out data as a B-tree on a raw disk
   2. Kernel does not expose block input output interface
   3. Kernel only exposes read(), write(), etc.

# Monolithic Kernel <-> Microkernel
* Much small kernel
   1. Mostly basic IPC layer + some scheduling and virtual memory
* Traditional OS services are offered as processess (servers)


## Microkernel: Lessons
* A small kernel == a fast IPC layer
   * OS servuce processes
   * New user-level servers example X server
* Pros: Modularity
* Cons : May be hard to do cross module optimization
* 1st gen: [Mach](https://www.cs.cmu.edu/afs/cs/project/mach/public/www/mach.html) by Richard Rashid and Avie Tevanian
   * later, Mach + BSD -> NeXT -> MacOS
* 2nd gen: L4 [mu-kernal](http://os.inf.tu-dresden.de/L4/overview.html)

# Exokernel

* Exokernel: an opearting systen architecture for application resource management.
    * written by dawsonR.engler, Mfrank kaashoek and James O'Toole Jr.
    * SOSP 1995
* Application can deal with:
    * scheduling: saving/restore on context switch
    * page faults: managing TLB entries.

## Exokernel: Apps + Library OS

## Exokernel
* Kernel exposes:
   1. hardware(Memory,Disk,CPU etc.)
   2. Allocation
   3. Revocation
   4. Names

## Background: MSP CPU
* Risk architecture
* MMU
  * no page table support
  * only(larger) TLB
* Kernel manages TLB
    * On TLB fault, kernel adds a new TLB entry covering the faulting virtual address.

## Exokernel: App-level Memory Management
* Kernel exposes:
    * Physical pages and VA -> PA MMU mappings (directly to user based application)
* Syscalls (app -> kernel)
    1. pa= AllocPage() ->(application can get physical page)
    2. TLBWrite (va,pa, permissions) -> application can update TLB directly
    3. Grant (env,pa): allow another process to acces pa
    4. DeAllocPage (pa) -> application can delete physical page directly
* Upcalls (kernel -> app)
    * PageFault (va,info) -> hey application tehre is a page fault at this address.

# Exokernel: App-level Memory Management
* What if we run out of physical pages?
   * Kernel asks an app to give up a page.
   * An app decides which page to give up.
* Upcalls (kernel -> apps)
   * Please ReleaseMemory(amount)
* Syscalls (app ->kernel)
   *  DeAllocPage (pa)
* Nice kernel and bad app
  * kernel may force ....

## Exokernel : App-level Memory Management
* Kernel keeps track of:
  * who owns (for e.g-> the particular page)
  * wo has access to it
  * which mappings it uses
* Q: Can a kernel do less than this?

## Usecase
* A database manages in-memory page caches 

## Exokernel: App-level CPU Management

* Resource: CPU
* Syscalls (app->kernel)
  * Yield() gives up CPU
* Upcalls (kernel -> app)
  * PleaseYield() (application is using cpu alot please yield)
  * Resume()
* Appdecides how to yield/resume
  * OnPleaseYield, app saves registers
  * OnResure, app restores registers
* Nice kernel and badd app
  * Cut next quota
  * or forcefully preempt(upcall, you are about to be killed)
## Usecase
* PleaseYield() in the middle of a critical section
* App yields after releasing a lock
## Exokernel: App-level IPC Management
* Syscalls (app -> kernel)
  * Yield (process)
* Upcalls (kernel -> app)
  * IPCRecieve()
* Scheduling
  * App1 schedules App2
* Message passing
  * App1 leaves registers along
  * App2 gets all registers from app1

## Exokernel : Peformance
* Fast traps
  * trap path doesn't save (most) registers
* Fast upcalls
  * no need to restore registers
* Protected calls to IPC
  * message passing through registers
* Map some (read-only) kernel structures to user space
## Exokernel: Lessons
* Custom abstraction
* Much of kernel can be implemented in user level
  * Decompose ...
* Library OS




