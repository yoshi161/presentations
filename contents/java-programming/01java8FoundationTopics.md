<!-- .slide: data-state="main" data-background="../../images/master.png" data-background-size="contain" -->

# Java 8

## Lambda Expressions, Streams, and More

---

# Agenda

- Installation, Setup, and Review of Foundational Topics
- Java 8 Lambda Expressions Part 1
- Java 8 Lambda Expressions Part 2
- Java 8 Lambda Expressions Part 3
- Static & Default Methods in Java 8 Interfaces
- Java 8 Lambda Expressions Part 4
- Java 8 Streams Part 1
- Java 8 Streams Part 2
- Java 8 Streams Part 3

---

# Agenda (continue)

- Lambda-Related Methods Directly in Lists and Maps
- File I/O in Java 8 Part 1: Treating Files as Streams of Strings
- File I/O in Java 8 Part 2:
  - Using Lambdas and Generic Types to Make File-Reading Code More Flexible, Reusable, and Testable

---

# Installation, Setup, and Review of Foundational Topics
## Topic Covered:
- Installing Java 8
- Installing and IDE that supports Java 8
- References
- Review of basic handlers and anonymous classes: separate classes, main class that implements the interface, named inner classes, anonymous inner classes
- Review of making generic classes and methods

---

# Why Java 8 ?

- Make code more flexible and reusable
  - Lambda expressions
- Make code more convenient to write
  - High-level methods in Stream interface
- Make code faster and more memory efficient
  - Lazy evaluation and automatic parallelization for streams
- Adapt to the times
  - Others will be using lambdas and streams, since they are standard part of Java now.
  So, you have to learn it simply to be able to use and modify others’ code.
- Besides, once you get a taste of the benefits,  you will want to use the new features frequently

---

# Step 1: Get Java 8 Code and Java Docs
- JDK 8 itself
  - http://oracle.com/technetwork/java/javase/downloads
- Online API
  - http://docs.oracle.com/javase/8/docs/api/
- Java Lambda
  - http://openjdk.java.net/projects/lambda/

---

# Step 2: Install IDE that Supports Java 8
- Eclipse: Luna or later
- NetBeans: version 8 or later
- IntelliJIDEA: version 13.1 or later

---

# Java 8 Online References
- The Oracle Java tutorial
  - Lambda expressions
    https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html
  - Method references
    https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html
  - Streams
      https://docs.oracle.com/javase/tutorial/collections/streams/
- Others

---

# Quick Review of Pre-Lambda Handlers

**Big idea**
- You need to designate code to be executed by other entity
  - E.g., you need code to respond a button click, to  run in the background for threads, or to handle sorting comparisons


**Pre-lambda alternatives**
- Separate (normal) class :
	`Arrays.sort(theArray, new SeparateClass(...));`
- Main class (which implements interface) :
    `Arrays.sort(theArray, this);`
- Named inner class :
  `Arrays.sort(theArray, new InnerClass(...));`
- Anonymous inner class :
  `Arrays.sort(theArray, new Comparator<String>() { ... } );`

---

# Separate Class
```java
public class StringSorter1 {
    public static void doTests() {
        String[] testStrings = {"one", "two", "three", "four"};
        System.out.print("Original: ");
        ArrayUtils.printArray(testStrings);
        Arrays.sort(testStrings, new StringLengthComparator());
        System.out.print("After sorting by length: ");
        ArrayUtils.printArray(testStrings);
        Arrays.sort(testStrings, new LastLetterComparator());
        System.out.print("After sorting by last letter: ");
        ArrayUtils.printArray(testStrings);
    }
}
```

---

# Separate Classes (continued)

```java
public class StringLengthComparator
		implements Comparator<String> {

  @Override
  public int compare (String s1, String s2) {
  	return (s1.length() - s2.length());
  }
}
```
---

# Separate Classes (continued)

```java
public class LastLetterComparator
			implements Comparator<String> {
  @Override
  public int compare(String s1, String s2){
  	return (s1.charAt(s1.length() - 1) -
    		s2.charAt(s2.length() - 1));
  }
}
```

---

# Test Class

```java
public class StringSorter1Test {

	public static void main (String[] args) {
    		StringSorter1Test.doTests();
    }
}
```
```
Output
Original: {one, two, three, four}
After sorting by length: {one, two, four, three}
After sorting by last letter: {one, three, two, four}
```

---

# Helper Class (for Printing Array)
```java
public class ArrayUtils {
	public static <T> void printArray(T[] entries) {
    		System.out.println(Arrays.asList(entries));
    }
}
```

---

# Separate Classes: Pros and Cons
**Advantages**
- Flexible: can pass arguments to class constructor
- More reusable: loosely coupled

**Disadvantage**
- Need extra step to call methods in main app. How does handler call code in the main application? It needs a reference to the main app to do so.
  - E.g., you pass main app instance to constructor
  - But, even then, the methods you call in the main app must be public

---

# Main App Implementing Interface
```java
public class StringSorter2 implements Comparator<String> {
  public void doTests() {
    String[] testStrings = {"one", "two", "three", "four"};
    System.out.print("Original: ");
    ArrayUtils.printArray(testStrings);
    Arrays.sort(testStrings, this);
    System.out.print("After sorting by length: ");
    ArrayUtils.printArray(testStrings);
    System.out.println("NO sorting by last letter.");
  }

  @Override
  public int compare(String s1, String s2) {
    return(s1.length() - s2.length());
  }
}

```
---

# Implementing Interface: Pros and Cons

**Advantages**
- No extra steps needed to call methods in main app
  - The code is part of the main app, so it can call any method or access any instance variable, even private ones.
- Simple
  - Widely used in real life for button handlers, when you know you will have only one button, or for threading code when you need only one method to run in the background.



**Disadvantages**
- Inflexible: hard to have multiple different versions since you cannot pass arguments to handler
  - For example, what if you want to sort arrays two different ways? If you supply “this” as second argument to Arrays.sort, it will refer to the same compare method both times.

---

# Named Inner Class

```java
public class StringSorter3 {
  public void doTests() {
    String[] testStrings = {"one", "two", "three", "four"};
    System.out.print("Original: ");
    ArrayUtils.printArray(testStrings);
    Arrays.sort(testStrings, new StringLengthComparator2());
    System.out.print("After sorting by length: ");
    ArrayUtils.printArray(testStrings);
    Arrays.sort(testStrings, new LastLetterComparator2());
    System.out.print("After sorting by last letter: ");
    ArrayUtils.printArray(testStrings);
  }

```

---

# Named Inner Class (cont.)

```java
private class StringLengthComparator2 implements Comparator<String> {
           @Override
           public int compare(String s1, String s2) {
             return(s1.length() - s2.length());
           }
         }

         private class LastLetterComparator2 implements Comparator<String> {
           @Override
           public int compare(String s1, String s2) {
             return (s1.charAt(s1.length() - 1) - s2.charAt(s2.length() - 1));
           }
         }
       }
```

---

# Named Inner Classes: Pros and Cons

**Advantages**
- No extra steps needed to call methods in main app. Inner classes have full access to
all methods and instance variables of surrounding class, including private ones.
  - However, if there is a name conflict, you have to use the unpopular OuterClass.this.name syntax
- Flexible: can define constructor and pass arguments


**Disadvantage**
- A bit harder to understand (arguably)

---

# Anonymous Inner Class

```java
public class StringSorter4 {
         public static void doTests() {
           String[] testStrings = {"one", "two", "three", "four"};
           System.out.print("Original: ");
           ArrayUtils.printArray(testStrings);
           Arrays.sort(testStrings, new Comparator<String>() {
             @Override
             public int compare(String s1, String s2) {
               return(s1.length() - s2.length());
             }
           });
           System.out.print("After sorting by length: ");
           ArrayUtils.printArray(testStrings);

```

---

# Anonymous Inner Class (cont.)

```java
           Arrays.sort(testStrings, new Comparator<String>() {
             @Override
             public int compare(String s1, String s2) {
               return(s1.charAt(s1.length() - 1) - s2.charAt(s2.length() - 1));
             }
           });
           System.out.print("After sorting by last letter: ");
           ArrayUtils.printArray(testStrings);
         }
       }

```

---

# Anonymous Inner Classes: Pros and Cons

**Advantages**
- As with named inner classes, full access to code of surrounding class (even private
methods and variables)
- Slightly more concise than named inner classes
  - But still bulky and verbose


**Disadvantage**
- Harder to understand (arguably)
- Not reusable
  - Cannot use the same anonymous class definition in more than one place

---

# Preview of Lambdas
**Desired features**
- Full access to code from surrounding class
- No confusion about meaning of “this”
- Much more concise, succinct, and readable
- Encourage a functional programming style


**Quick peek**
- Anonymous inner class
```java
Arrays.sort(testStrings, new Comparator<String>() {
        @Override
        public int compare(String s1, String s2) {
            return(s1.length() - s2.length());
        }
    });
```

- Lambda
```java
    Arrays.sort(testStrings,(s1, s2) -> s1.length() - s2.length());
```

---

# Building Generic Methods and Classes: Overview

---

## Using Existing Generic Methods and Classes
- **Basic capability**
  - Even beginning Java programmers need to know how to use classes that support generics
  - You cannot properly use Lists, Maps, Sets, etc. without this
  - Covered in earlier section

```java
List<Employee> workers = ...;
Map<String,Employee> workerDatabase = ...;
```

---

## Creating Your Own Generic Methods and Classes
- **Intermediate capability**
  - Intermediate Java developers should also to be able to define classes or methods that support generics
  - In Java 7 and earlier, being able to do this was mostly reserved for advanced developers, but it is done much more commonly in Java 8
    - Because lambda functions and generic types work together for same goal: to make code more reusable

```java
    public interface Map<K,V> { ... }
    public static <T> T lastElement(List<T> elements) { ... }
```
---

## Generic Classes and Methods: Syntax Overview

- **Using ```<TypeVariable>```**
  - If you put variables in angle brackets in the class or method definition, it tells Java
that uses of those variables refer to types, not to values
  - It is conventional to use short names in upper case, such as T, R (input type, result
type) or T1, T2 (type1, type2), or E (element type)


- **Examples**
```java
    public class ArrayList<E> ... {
        ...
    }
```
```java
    public static<T> T randomElement(T[] array) {
        ...
    }
```

---

# Generic Methods

---

## Generic Classes and Methods: Syntax Details
- Declaring methods that support generics
  - This says that the best method takes a List of T’s and returns a T.
  - The T at the beginning means T is not a real type, but a type that Java will figure out from the method call.
```java
    public static <T> T best(List<T> entries, ...) { ... }
```



- Java will figure out the type of T by looking at parameters to the method call
```java
    List<Person> people = ...;
    Person bestPerson = Utils.best(people, ...);
    List<Car> cars = ...;
    Car bestCar = Utils.best(cars, ...);
```

---

## Partial Example: randomElement

```java
    public class RandomUtils {
    ...

        public static <T> T randomElement(T[] arrya) {
            return (array[randomIndex(array)]);
        }
    }
```
**Notes:**
- In rest of method, T refers to a type
- Java will configure out what type T is by looking at the parameters of the method call
- Even if there is an existing class actually called T, it is irrelevant here.

---

## Complete Example: randomElement
```java
    public class RandomUtils {
      private static Random r = new Random();

      public static int randomInt(int range) {
        return(r.nextInt(range));
      }

      public static int randomIndex(Object[] array) {
        return(randomInt(array.length));
      }

      public static <T> T randomElement(T[] array) {
        return(array[randomIndex(array)]);
      }
    }
```

---

## Using RandomUtils

- **Examples**
```java
    String[] names = { "Joe", "John", "Jane" };
    String name = RandomUtils.randomElement(names);
    Color[] colors = { Color.RED, Color.GREEN, Color.BLUE };
    Color color = RandomUtils.randomElement(colors);
    Person[] people = { new Person("Larry", "Page"), new Person("Larry", "Ellison"),
    new Person("Larry", "Bird"), new Person("Larry", "King") };
    Person person = RandomUtils.randomElement(people);
    Integer[] nums = { 1, 2, 3, 4 }; // Integer[], not int[]
    int num = RandomUtils.randomElement(nums);
```
- **Points**
  - No typecast required to convert to String, Color, Person, Integer
  - Autoboxing lets you assign entry from Integer[] to an int, but array passed to randomElement must
      be Integer[] not int[], since generics work only with Object types, not primitive types

---

# Generic Classes or Interfaces

---

## Generic Classes and Methods: Syntax Details
- Declaring classes or interfaces that support generics
    ```java
        public class SomeClass <T> { ... }
    ```
- Methods in the class can now refer to T both for arguments and for return values
    ```java
        public T getSomeValue(int index) { ... }
    ```
- Java will figure out the type of T by your declaration
  ```java
    SomeClass <Person> blah = new SomeClass<>();
  ```

---

## Example: Generic Class (Simplified)

```java
    public class ArrayList<E> {
        public E get(int index) {...}

        public boolean add(E element) { ... }
        ...
    }
```
- ```<E>``` at first line says, In rest of class, E does not refer to an existing type. Instead, it refers to whatever type was defined when you created the list, then E refers to String. As below example:
```java
    ArrayList<String> words = ... ;
```
- At second line says, that get return an E. So, if you created ArrayList<Employee>, get returns an Employee. No typecaast required in the code that calls get.
- At third line says, that add taken an E as a parameter. So, if you created ArrayList```<Circle>```, add can take only a Circle.

---

# Wrap-Up

---

## Summary
- **Install Java 8** http://www.oracle.com/technetwork/java/javase/downloads/
- **Bookmark the Java 8 JavaDocs API** http://docs.oracle.com/javase/8/docs/api/
- **Get an IDE**
  - Eclipse, NetBeans, IntelliJ IDEA, etc.
- **Review options for handlers**
  - Especially anonymous inner classes.
    - This is the main alternative that Java 8 lambdas replace.
- **Review making generic classes and methods**
  - Techniques used frequently with lamb

---

# Questions ?

---
