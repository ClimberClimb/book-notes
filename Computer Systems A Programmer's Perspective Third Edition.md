

# Computer Systems A Programmer's Perspective Third Edition

## Chapter one: A Tour Of Computer Systems

* In different contexts, the same sequence of bytes might represent an integer, , floating-point number ,or machine WX20190512-201104@2x.png

* C is the language of choice for system-level programming

* the compilation system:

  <div align="center"> <img src="pics/WX20190512-201104@2x.png" width="600
    "/> </div><br>

* the include command tells the preprocessor to read the contents of the system header file stdio.h and insert it into the program text
* The buses are typically designed to transfer fixed-size chunks of bytes known as words
* Main memory consists of a collection of dynamic random access memory
* Each IO devices is connected to the IO bus by either a controller or an adapter, the distinction between the two is mainly one of packaging, controllers are chip sets in the device itself or on the system's motherboard, an adapter is a card that plugs into a slow on the motherboard
* the idea behind caching is that a system can get the effect of both a very large memory and a very fast one by exploit locality
* the register file is a cache for the L1 cache, Caches L1 and L2 are caches for L2 and L3
* the operating system as a layer of software interposed between the application programs and the hardware
* operating system:
  * to protect the hardware from misuse by runaway applications
  * to provide applications with simple and uniform mechanisms for manipulating complicated and often wildly different low-level hardware devices

* files are abstractions for IO devices, VM is an abstraction for both the main memory and disk IO devices, and processes are abstractions for the processor, main memory and IO devices
* the operating system performs this interleaving with a mechanism known as context switching
* the kernel is the portion of the operating system code that is always resident in memory
* Kernel is code and data structures that the system uses to manage all the processes
* the process virtual address

<div align="center"> <img src="pics/WX20190512-225319@2x.png" width="600
  "/> </div><br>

* application programs are not allowed to read or write the contents of this area or to directly call functions defined in the kernel code
* The basic idea is to store the contents of a process's virtual memory on disk and then use the main memory as a cache for the disk
* Amdahl's Law: S = 1 / ((1- alpha) + alpha/k), alpha is the portion and k is the spped up times
* The term parallelism to refer to the use of concurrency to make a system run faster
* Hyper threading, called simultaneous multi-threading, is a technique that allows a single CPU to execute multiple flows of controls
* a hyper threaded processor decides which of its threads to execute on a cycle-by-cycle basis
* More recent processors can sustain execution rates of 2-4 instructions per cllock cycle
* On the processor side, the instruction set architecture provides an abstractions of the actual processor hardware
* SIMD, single-instruction multiple data parallelism



## Chapter two Representing and manipulating information

* The electronic circuitry for storing and performing computations on two-valued signals is very simple
* Floating-point encodings are a base-2 version of scientific notation for representing real numbers
* the C standards are designed to allow a wide range of implementations, the java standard is quite specific on the formats and encodings of Java
* C standard: --std=c11, --std=c99, --ansi, --std=c89
* every computer has a word size, indicating the nominal size of pointer data, since a virtual address is encoded by such a word, the most important system parameter determined by the word size is the maximum size of the virtual address space
*  compile you can use -m32 to generate program for 32 or 64 bit machine, or -m64 just for 64-bit machine
* ISO C99 introduce a class of data types where the data sizes are fixed regardless of compiler and machine settings
* char type, most compilers treat these as signed data, the C standard does not guarantee this, it is best to use signed char
* a multi-byte object is stored as a contiguous sequence of bytes, with the address of the object given by the smallest address of the bytes used
* The least significant byte comes first, is referred to as little-endian
* byte ordering becomes an issue, when binary data are communicated over a network between different machines
* size_t is the preferred data type for expressing the sizes of the data structures
* we can dereference a pointer with array notation, we can reference array elements with pointer notation.
* the ASCII code for decimal digit x happens to be 0x3x, text data are more platform independent than binary data
* the letters 'a' through 'z' have ascii codes 0x61 through 0x7A
* the strlen does not count the terminating null character in C
* 