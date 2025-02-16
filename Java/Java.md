
![[Java_1.jpg]]
![[Java_2.jpg]]
![[Java_3.jpg]]

Java is statically typed language that means we have to specify the datatype
![[Java_4.png]]

Typecasting:
```java
int a = 10;
double b = a; // implicit

double c = 54.0;
int d = (int)c; // explicit
```

Operators:
![[Java_5.jpg]]

Loops:
* For
* While
* Do While
* For Each -- for(int x: sum){}

break - to terminate the execution
continue - move to the next iteration

#### OOPs

*Object-Oriented Programming* concept refers to programming languages that use objects in programming.
aims to implement real-world entities like *inheritance*, *hiding*, *polymorphism*, etc. in programming. The main aim of OOPs is to bind together the data and the functions that operate on them so that no other part of the code can access this data except that function.

Classes and objects are the two main aspects of object-oriented programming.
a Class is a template for objects, and an object is an instance of a class.

`new` keyword - used to create new objects and allocate memory for them

**Constructor** - used to initialize the object
* If class does not have any constructor then java creates one default constructor
* constructor name == Class name
* constructor does not return any value, not even void

Constructor Overloading: 
* defining multiple constructors within a class with different parameter lists
* Atleast one must be present while constructor overloading
   1. Number of arguments may differ
   2. Type of arguments may differ
   3. Order of arguments may differ

Method Overloading:
* defining multiple functions within a class with same name
* Atleast one must be present while constructor overloading
   1. Number of arguments may differ
   2. Type of arguments may differ
   3. Order of arguments may differ

`this` keyword:
* Refers current invoking object
* this() => for calling current class constructor from inside different constructor within same class
  * it should be the first line in a class constructor

#### Pillars of OOPs

##### Abstraction
Data Abstraction is the property by which only the essential details are displayed to the user. showing features and hiding internal details (implementation).
```java
// abstract class
abstract class GFG {
    // abstract methods declaration
    abstract void add();
    abstract void mul();
    abstract void div();
}
```
##### Encapsulation
the wrapping up of data under a single unit. It is the mechanism that binds together the code and the data it manipulates. It is a protective shield that prevents the data from being accessed by the code outside this shield. Use setter getters
```java
// Employee class contains private data
// called employee id and employee name
class Employee {
  private int empid;
  private String ename;
  // Setter methods
  public void set_id(int empid) { 
    this.empid = empid; 
  }
  // Getter methods
  public int get_id() { 
    return empid; 
  }

}
public class Geeks {
  public static void main(String args[]){
    Employee e = new Employee();
    e.set_id(78);
    System.out.println("Employee id: " + e.get_id());
  }
}

```

#### Inheritance
It is the mechanism in Java by which one class is allowed to inherit the features (fields and methods) of another class. We are achieving inheritance by using ****extends**** keyword.
* base class or parent class or super class
* derived class or child class or sub class
![[Java_6.jpg]]

```java
//base class or parent class or super class
class A{
  String name;
  //parent class methods
  void method1(){}
  void method2(){}
}

//derived class or child class or base class
class B extends A{  //Inherits parent class methods
  //child class methods
  void method3(){}
  void method4(){}
}

```

##### Polymorphism
Differentiate between entities with the same name efficiently
1. Method Overloading: Also, known as compile-time polymorphism, is the concept where more than one method share the same name with different Parameters in a class. The return type of these methods can or cannot be same.

2. Method Overriding: Also, known as run-time polymorphism, is the concept where method in the child class has the same **name, return-type and parameters** as in parent class. The child class provides the implementation in the method already written.

```java
// Parent Class
class Parent {
    // Method Declared
  public void func(){
    System.out.println("Parent Method func");
  }

    // Method Overloading
  public void func(int a){
    System.out.println("Parent Method func " + a);
  }
}
// Child Class
class Child extends Parent {

  // Method Overriding
  @Override 
  public void func(int a){
    System.out.println("Child Method " + a);
  }
}

// Main Method
public class Main {
  public static void main(String args[]){
    Parent obj1 = new Parent();
    obj1.func();
    obj1.func(5);

    Child obj2 = new Child();
    obj2.func(4);
  }
}

```


`super` keyword in Java _is a reference variable that is used to refer immediate parent class_ when we're working with objects
* super.x
* super() - used to call parent class constructor
  * super() should be the first line inside child class constructor


#### Exception Handling

Exception = abnormal condition
Keywords for EH - `try, catch, finally, throw, throws`

![[Java_7.jpg]]

```java
public class App {
  public static void main(String[] args) {
    System.out.println("\nStarting");
    try {
      int n1 = Integer.parseInt(args[0]);
      int n2 = Integer.parseInt(args[1]);
      int div = n1 / n2;
      System.out.println("Division result is " + div);
    } catch (ArithmeticException e) {
      System.out.println("n2 cant be zero");
      System.out.println(e.getMessage());
    } catch (NumberFormatException e) {
      System.out.println("Invalid number");
      System.out.println(e.getMessage());
    } catch (Exception e) {
      // catching parent exception for all the other exception tht could occur
      System.out.println("Error!!");
      System.out.println(e.getMessage());
    } finally {
      // always gets executed
      System.out.println("Cleaning up");
    }
    System.out.println("Terminated....");
  }

}
```