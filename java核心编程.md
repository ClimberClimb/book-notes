

# Java核心编程

## Chapter One Intro
* Simple
	* The syntax for java is a simple version of C++.
	* There is no need for header files, pointer arithmetic, structures, unions, operator overloading, virtual base classes.
	* There is now a separate Java Micro Edition with a smaller library, suitable for embedded device
* Object-Oriented
	* Object-oriented design is a programming language technique that focuses on the data and on the interfaces to that object
* Distributed 
* Robust
	* The jave compiler detects many problems that in other languages would  show up only at runtime
* Secure 
	* Java was designed to make certain kinds of attacks impossible among them
		* Overruning the runtime stack
		* Corrupting memory outside its own process space
		* Reading or writing files without permission
* Architecture-Neutral
	* Virtual machines have the option of translating the most frequently executed bytecode sequences into machine code-a process called just-in-time compilation
* Portable
	* The java librarie do a great job of letting you work in. a platform-independent manner
* Interpreted
* High-Performance
* Multithreaded
* Dynamic
	* Libraries can freely add new methods and instance variables without any effect on their clients

## Chapter Two Environment

* JDK includes the compiler , JRE includes a Java Runtime Environment

* Java Standard Edtion(SE), Java EE(Enterprise Edition), Java ME(Micro Edition)

* the javac program is the java compiler it compiles the file Welcome.java into the file Welcome.class, the java program launches the JVM, ite executes the bytescodes that the compiler placed in the class file

* produce jar file 

  ```java
  jar cvfm RoadApplet.jar RoadApplet.mf *.class
  
  ```

## Chapter Three Fundamentals

* Java is case sensitive
* The keyword public is called an access modifier, these modifiers control the level of access other parts of a program have to this code
* Everything in a Java program must be inside a class
* you need to make the file name for the source code the same as the name of the public class
* the main method must be declared public
* the idea of static member functions in C++, that do not operate on objects, the main method is always static

* Java is strongly typed language, every varibale must have a declared type

### Data Types

* int 4 bytes, short 2 bytes, long 8 bytes, byte 1byte

* float 4 bytes, double 8 bytes, type float have a suffix F or f, 

* three special floating-point values: positive infinity, negatibe infinity, NaN(not a number), dividing a positive number by 0 is positive infinity, computing 0/0 or the square root of a negative number yields NaN, integer division by 0 raises an exception

* x == Double.NaN is never true, if (Double.isNan(x)) // check whether x is not a number

* if you need previse numerical computations without roundoff errors, use the BigDecimal class

* 'A' us a character constant with value 65, "A" a string containing a single character

* unicode escapes sequences are processed before the code is parsed

* char type in java is 16-bit type

* a code point is a code value that is associated with a character in an encoding scheme

* code points U+0000 to U+FFFF, basic mutilingual plane, sixteen additional planes hold the supplementary characters. the supplementary character are encoded as consecutive pairs of code units

* the char type describes a code unit in the UTF-16 encoding, **not to use char type in your progam unless you a re actualy manipulating UTF-16 code units**

* you cannot convert between integers and boolean values

* In Java, it is considered good style to declare variables as closely as possible to the point where they are first used

* the keyword **final** indicates that you can assign to the variable once and then its value is set once and for all

* set up a class constant with the keywords **static final**, if the constant is declared public , methods of other classes can also use it

* methods tagged with the strictfp keyword must use strict floating-point operations that yield reproducible results, **public static strictfp void main(String[] args)**

* **import static java.lang.Math.**, static imports

* you cannot cast betweeen boolean values and any numetic type to prevent common errors, you can use a conditional expression such as b ? 1:0 

* the bitwise operators are & and, |  or, ^ xor, ~ not

* > > > > > > > > > > > > > > > > > ">>>" operator fills the top bits with zero

* Java does not have a common operator, you can use common in the for statement

* enum Size {SMALL, MEDIUM, LARGE}

* Immutable strings have one great advantage, the compiler can arrange that strings are shared

* C++ strings are mutable, you can modify individual chatacters in a string

* test string for equal , **s.equals(t) ** the == operator only determines whether or not the strings are stored in the same location

* java makes strings behave like pointers for equality testing

* null indicates that no object is currently associated with the variable, str == null, str != null && str.length() != 0

* the length method yields the number of code units required for a given string in the UTF-16 encoding

* chatAt return the code unit at position n, n is between 0 and s.length() - 1

* **greeting.CodePoint return the number of code points, offsetByCodePoints**

* str.CodePoints().toArray(), new String(codePoints, 0, codePoints.length)

* StringBuilder is more efficient that the string cancatenation

  ```java
  StringBuilder builder = new StringBuilder()   
  ```

* the Scanner class is defined in the java.util package

* Java SE 6 introduces a Console class specifically for this purpose to read a password

* you can use the static String.format method to create a formatted string without printing it

* for new code you should  use **java.time** package for date and time

* $ specifes the index of the argument to be formatted   

  Sytem.out.printf("%1$s %2$tB %2$te, %2$tY", "Due date:", new Date());

* 
  ```java
  Scanner in = new Scanner(Paths.get("myfile.txt"), "UTF-8")
  PrintWriter out = new PrintWriter("myfile.txt", "UTF-8")w
   write to a file
  ```

* In Java, you may not declare identically named variables in two nested blocks

* javac -Xlint:fallthrough Test.java, issue a warning whenever an alternative does not end with a break statement

* labeled break, read_data: ..... break read_data; // using the babel break the outmost loop

* BigInteger, BigDecimal does the same for floating-point numbers

* int[] a = new int[100];  array , if you frequently need to expand the size of an arrya while your program is running, you should use a different data structure called an array list

* for (variable: collection) statement, for each

* Array.copyOf is often used to increase the size of an array

* In Java program, the name of the program is not stored in the args arrary
* to print out a quick and dirty list of the elements call System.out.println(Arrays,deepToString(a));
* Java has no multidimensional arrays at all ,only one-dimensional arrays



## Chapter four Objects and Classes

* object oriented programming: puts the data first and then looks at the algorithms to operate on the data
* the bits of data in an object are called its instance fields, and the procedures that operate on the data are called its method
* when you invoke a method on an object its state may change
* instances of a class always differ in their identity and usually differ in their state
* A simple rule of thumb in identifying classes is to look for nouns in the problem analysis, methods, on the other hand, correspond to verbs
* Dependence uses a, Aggregation has-a , Inheritance Is-a
* Many programmers use the UML notation to draw class diagrams that describe the relationship between classes
* In the Java programming language, you use constructors to construct new instances
* Constructors always have the same name as the class name, thus the constructor for the Date class is called Date
* an object variable refers to an object, In Java the return value of the new operator is also a reference
* all java objects live on the heap, when an object contains another object variable, it contains just a pointer to yet another heap object
* the Date class represents a point in time the LocalDate class expresses days in the familiar calendar notation
* mutator method, accessor methods(without modifying objects)
* 

