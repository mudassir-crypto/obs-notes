##### Bean (Student)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-contect.xsd">


	<bean class="com.springcore.Student" name="student1">
		<property name="studentId">
			<value>2344</value>
		</property>
		<property name="studentName">
			<value>John</value>
		</property>
		<property name="studentAddress">
			<value>Andhra</value>
		</property>
	</bean>
	<bean class="com.springcore.Student" name="student2">
		<property name="studentId" value="2387" />
		<property name="studentName" value="Jana" />
		<property name="studentAddress" value="Mumbai" />
	</bean>
	<bean class="com.springcore.Student" name="student3" p:studentId="3454"
		p:studentName="Billy" p:studentAddress="NYC" />

</beans>
```

main:
```java
ApplicationContext context = new ClassPathXmlApplicationContext("config.xml");
Student s1 = (Student)context.getBean("student1");
Student s2 = (Student)context.getBean("student2");
Student s3 = (Student)context.getBean("student3");
System.out.println(s1);
System.out.println(s2);
System.out.println(s3);
```

##### Collection types (Employee)
```xml
<bean class="com.springcore.collection.Employee" name="emp1">
	<property name="name" value="Kitty" />
	<property name="phones">
		<list>
			<value>98346587435</value>
			<value>456645656</value>
			<value>876433243</value>
			<null />
		</list>
	</property>
	<property name="addresses">
		<set>
			<value>address1</value>
			<value>address2</value>
			<value>address3</value>
		</set>
	</property>
	<property name="courses">
		<map>
			<entry key="java" value="2month" />
			<entry key="python" value="4month" />
		</map>
	</property>
	<property name="props">
		<props>
			<prop key="name">Nilly Boy</prop>
			<prop key="dbname">myspace</prop>
		</props>
	</property>
</bean>
```

Emp.java
```java
public class Employee {
	private String name;
	private List<String> phones;
	private Set<String> addresses;
	private Map<String, String> courses;
	private Properties props;
}
```

executed same as student

##### Injecting reference types (A, B)

A.java & B.java
```java
public class A {
	private int x;
	private B ob;
}

public class B {
	private int y;
}
```

config.xml
```xml
<bean class="com.springcore.reference.B" name="bref">
	<property name="y" value="90" />
</bean>
	
<bean class="com.springcore.reference.A" name="aref">
	<property name="x" value="54" />
	
	<property name="ob">
		<ref bean="bref" />
	</property>

	// another way
	<property name="ob" ref="bref"></property>
</bean>

// another way using p schema
<bean class="com.springcore.reference.A" name="aref" p:x="54" p:ob-ref="bref" />
```

##### Constructor injection

Person and cert java
```java
with full fields constructors
public class Person {
	private String name;
	private int id;
	private Cert cert;
}
public class Cert {
	private String name;
}
```

```xml
<bean class="com.springcore.ci.Cert" name="certRef" c:name="AWS" />

<bean class="com.springcore.ci.Person" name="person1">
	<constructor-arg value="Jugish" />
	<constructor-arg value="345" type="int" />
	<constructor-arg ref="certRef" />
</bean>
```