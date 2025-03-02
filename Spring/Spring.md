https://github.com/onsever/spring-framework-notes/blob/main/README.md

![[Spring_1.jpg]]

- **Tight Coupling:** When two classes are tightly coupled, they are dependent on each other. If one class changes, the other class will also change.

- **Loose Coupling:** When two classes are loosely coupled, they are not dependent on each other. If one class changes, the other class will not change

```java
public class GameRunner {  
  // public MarioGame game; // Tightly coupled to a specific game, so we need to change this.  
  private final GamingConsole game; // Loosely coupled, it's not a specific game anymore. Games implement GamingConsole interface. Polymorphism.  
  public GameRunner(GamingConsole game) {  
  this.game = game;  
  }  
  
  public void run() {  
  System.out.println("Running game: " + game);  
  game.up();  
  game.down();  
  game.left();  
  game.right();  
  }  
}
```

![[Spring_2.jpg]]

![[Spring_3.jpg]]

![[Spring_4.jpg]]

#### What is a Spring Container?

Spring Container is the core of the Spring Framework. The Spring Container will create the objects, wire them together, configure them, and manage their complete life cycle from creation till destruction. The Spring Container uses DI to manage the components that make up an application.

The Spring Container manages Spring beans and their life cycle.

JVM (Java Virtual Machine) is the container that runs the Java application. Spring Container is the container that runs the Spring application. JVM contains Spring Context.

Spring Bean - a Java object that's managed by the Spring IoC container
Spring Container == Spring Context == Spring IoC Container


#### Different Types of IoC Containers
 Spring provides two types of IoC containers:

- **BeanFactory Container:** Basic-simple IoC container provided by Spring. It is the root interface for accessing a Spring BeanFactory.
- **ApplicationContext Container:** It is the advanced container provided by Spring. It is built on top of the BeanFactory container. It adds more enterprise-specific functionality.

Most of the Spring applications use ApplicationContext container. It is recommended to use ApplicationContext container over BeanFactory container. (Web applications, web services, REST API and microservices)

#### Spring Bean Configuration
Spring Bean Configuration is the process of defining beans. The Spring Bean Configuration can be done in two ways:
- XML Based Configuration
- Annotation Based Configuration

![[Spring_5.jpg]]

#### Dependency Injection
Dependency Injection is a design pattern that allows objects to receive their dependencies from an external source instead of creating it themselves.

Data Types to be injected: 
1. Primitive - char, boolean, byte, short, int, long, float, double
2. Collection - List, Set, Map, etc
3. Reference - Other Class Object

DI can be done in 2 ways:
1. Setter injection
   ![[Spring_6.jpg]]

2. Contructor Injection
   ![[Spring_7.jpg]]

