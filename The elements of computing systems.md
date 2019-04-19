# The elements of Computing Systems

## Preface

* hardware and software systems are tightly interrelated through a hidden web of abstractions, interfaces, and contract-based implementations

* The map of the computing system

* <div align="center"> <img src="pics/course_map.png" width="600"/> </div><br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          

* In computer science,  we take the notion of abstraction very concretely, defining it to be a statement of what the entity does and ignoring the details of how it does it



## Boolean Logic

* Every Boolean function, no matter how complex, can be expressed using three Boolean operators only: And, Or and Not

* every Boolean function can be constructed from Nand operations alone

* <div align="center"> <img src="pics/boolean_functions.png" width="600"/> </div><br> 

* logic design is the art of interconnecting gates in order to implement more complex functionality, leading to the notion of composite gates
* a multiplexor is a three-input gate that uses one of the inputs called selection bit to select and output one of the other two inputs, called data bits
* a demultiplexor performs the opposite function of a multiplexor, it takes a single input and channels it to one of two possible outputs according to a selector bit 
* computer hardware is typically designed to operate on multi-bit arrays called buses

## Boolean Arithmetic

* The ALU is the centerpiece chip that executes all the arithmetic and logical operations performed by the computer
* most of the operations performed by digital computers can be reduced to elementary additions of binary numbers
* the system can code a total of 2^n signed number, of which the maximal and minimal numbers are 2^(n-1) - 1 and -2^(n-1), respectively
* 2's complement method facilitates the addition of any two signed numbers without requiring special hardware beyond that needed for simple bit-wise addition
* The material implications of these theoretical results are significant, basically, they imply that a single chip, called ALU, can be used to encapsulate all the basic arithmetic and logical operators performed in hardware
* 