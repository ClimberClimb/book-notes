# Algorithms

## Fundamentals

###  Basic Programming Model

* Recursion
  * The Recursion has a base case
  * Recursive calls must address subproblems taht are smaller in some sense
  * Recursive calls should not address subproblems that overlap

* The purpose of an API is to separate the client from the implementation
* Math.abs(-2147483648) =  -2147483648, integear overflow （-2147483648，2147483647]

### Date Abstraction

* Objects are characterized by 3 essential properties: state, identity, behavior
* Java methods allow only one return value, so using objects enables us to write code which returns multiple values
* Subtyping makes modular programming more difficult, first, the subclasses is completely dependent on the superclass, and the subclass code is having access to instance variables, can subvert the intention of the superclass code
* If you use reflection, you can access the `private` fields of any class and change them

### Bags, Stacks

* Programming with linked lists presents all sorts of challenges for programs and data

* ResizingArrayStack is a model for keeping their memory usage under control

* a wild interface is something we strive to avoid

* Primitive type can store every value of their corresponding wrapper type except `null`.

* values between -128 and 127 appear to refer to the same immutable Integer objects (Java's implementation of `valueOf()` retrieves a cached values if the integer is between -128 and 127), while Java constructs new objects for each integer outside this range

  ```java
  Integer a1 = 100;
  Integer a2 = 100;
  System.out.println(a1 == a2);   // true
  Integer c1 = 150;
  Integer c2 = 150;
  System.out.println(c1 == c2);   // false
  ```

* 

### Analysis of algorithms