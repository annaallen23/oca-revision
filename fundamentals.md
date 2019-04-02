# Java Fundamentals

## Class Structure

 - A class is declared using the `class` keyword
 - A class can have _fields_ and _methods_
   - A field is used for holding data
   - A method is used to define behaviour

```java
public class Dog {

    private String name;   // A field
    private String breed;  // Another field
    private int age;       // A third field

    // A method
    public String getName() {
        return name;
    }

    // Another method
    public void setAge(int newAge) {
        age = newAge;
    }
}
```

 - Fields must declare their data type
 - Methods must declare the data type of all of their inputs, and also the data type of their output
   - The `void` return type indicates that no value is returned.

## File Structure

  - Usually there is only one class per file.
  - It is possible to have more than one class in a file, but:
    - At most one class in the same file can be public
    - If there is a public class, its name must be exactly the same as the name of the file

## Running a Java program

  - A Java program begins running at the `main` method of a class.
  - The main method is declared using the incantation `public static void main(String[] args) {}`
    - `public static void main` cannot change
    - The method must accept an array of `String`s as input
      - Any one of the three choices `String[] args`, `String args[]` or `String... args` will work
    - The input parameter can have any name you like (not necessarily `args`)
  - The program is compiled using `javac` (the compiler) which is provided with the JDK (Java Development Kit).
  - The program is run using the `java` command which is part of the JRE (Java Runtime Environment).

```
javac Program.java
java Program
```

  - `javac` compiles `.java` files into _bytecode_ contained in `.class` files.
  - `java` _runs_ this bytecode
  - The `String` arguments to `main` can be passed to `java` on the command line:

```
javac Program.java
java Program arg0 arg1 arg2
```

## Packages and Imports

### Packages

  - Java has a lot of built-in functionality.
  - In order to use a particular class, the compiler needs to be told where to look for that class.
  - Packages are how Java organises the classes you write so that they can be referred to easily.
  - Package names are hierarchical, and match the directory structure of the code.

### Imports

  - An import statement is used to tell the compiler to include classes from a particular package.
  - `import my.package.MyClass` will import `MyClass` from the package `my.package`.
  - `import my.package.*` will import all classes in the package `my.package` - but not from its subpackages!
  - If multiple imports refer to classes with the same name, these rules are followed:
    - If both imports are explicit named imports, that will not compile due to the ambiguity.
    - If both imports are wildcards, that will also not compile.
    - If one import is explcit and the other is a wildcard, then the named import will take priority.

## Creating and Initialising Objects

### Creating Objects

  - An _instance_ of a class is called an _object_.
  - To create an _object_ you use the _constructor_ of a class.
  - A constructor is a special method on a class with no return type (not even `void`) and whose name exactly matches the name of the class.

```java
public class Dog {

    String name;

    public Dog(String theDogsName) {
        name = theDogsName;
    }
}
```

### Initialising fields

  - There are three ways to initialise fields when an instance is created
    - Field intialisation gives a value to the field on the line that it is declared.
    - Instance initializer blocks are code blocks in the definition of a class that will run when an instance of the class is created.
    - Fields can be initialised within a constructor (as in the above example).
  - Fields and instance initializer blocks will run in the order that they are written in the code.
  - Constructors will run _after_ all field and initializer blocks have been run

```java
public class Dog {

    String name = "Spot"; // Field initializer - will run first
    
    public Dog() {
        name = "Toto"; // Constructor - runs last
    }

    { name = "Fido"; } // Instance initializer - runs second
}
```  

## Primitive Types

 - Java has 8 primitive types
 - One Boolean type - `boolean` - which holds true/false values
 - Four integral types - `byte`, `short`, `int`, `long` - which hold integer values
   - `byte` is an 8-bit type, `short` is 16-bit, `int` 32-bit and `long` 64-bit
 - Two floating point types - `float`, `double` - which hold decimal values.
 - `char` which holds a single character (unlike `String` which holds multiple).

### Numeric types

 - Each integral type has a maximum and a minimum value it can hold - for example `byte` can only hold numbers between `-128` and `127`.
 - If you try to assign a value that is too large or too small, the code won't compile.
 - If you try to assign a larger value to a smaller type (e.g. a `long` value to an `int` variable) the code won't compile unless you explicitly _cast_ the value to the correct type.
 - To write a `long` literal you need to include the letter `L` at the end of the number.
 - Integer literals can be specified in base-10 (no prefix), binary (`0b` prefix), hexadecimal (`0x` prefix) or octal (`0` prefix)
 - Any integral value can be assigned to either floating point type without issue.
 - `float` literals have an `f` at the end of the number.
 - Numeric literals can contain underscores between any two digits and still be valid - but not at the beginning or end, or on either side of a decimal point.

## Reference types

  - Unlike primitive types, which store their values directly in memory, types which hold an object store the object elsewhere in memory and simply contain a _reference_ to the object.
  - Objects can have one, multiple, or no references to them at any given point in time.
  - If a variable has the value `null` then that means it doesn't point at any object at all.

## Variables

  - Variables are declared in Java by first writing the type, and then a name for the variable - e.g. `int x; String y;`
  - Multiple variables can be declared in one statement, separated by commas, as long as they all share the same type.

```java
String s1, s2, s3;
int i1, i2, i3, i4;
```

  - During a multiple-variable declaration, some, all or none of the variables can be given an initial value:

```java
int i1 = 1, i2, i3, i4 = 4
// i1 has the value 1
// i4 has the value 4
// i2 and i3 are uninitialized
```

  - Variables can have any name which is a valid identifier
    - Can only contain letters, numbers, `$` and `_`
    - Cannot start with a number
  - Fields (variables on a class) are given a default value when declared
    - `false` for `boolean`
    - `0` for integer primitives
    - `0.0` for floating point primitives
    - `\u0000` for `char`
    - `null` for objects
  - Local variables (variables within a method) are not default initialized and must be given a value before use.

## Variable Scope

  - Local variables are only valid for use within the method they are declared in.
  - Some variables have an even smaller scope than this - they can only be used within the _braces_ `{}` that they are declared in.

```java
public void doSomething(int x) {
    int y = 2;
    if (x > y) {
        int z = 3;
    }
    System.out.println(z); // Won't compile
}
```

## Garbage Collection

  - When an object has no more references to it, it becomes eligible for _garbage collection_ - an automatic process where the JVM frees up memory that is no longer needed.
  - Objects references are moved or destroyed when a variable is reassigned or goes out of scope.
  - `System.gc()` is a method that suggests to the JVM that it should run garbage collection now - but it's free to ignore the request!
  - Objects can implement a `finalize()` method that will be called when the JVM tries to garbage collect it.
    - If the garbage collection fails for whatever reason, `finalize` won't run a second time when garbage collection happens again.