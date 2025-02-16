Spring Container, Spring Context, IOC container, Application context are all same thing
It takes the input of class and creates a runtime environment

##### Java Bean vs POJO vs Spring Bean 
  ### Java Bean
    Have public default no-arg constructor
    Allow access to their props using getters and setter
    Implement java.io.Serializable

  ### POJO - plain old java object
    No constraints
    Any java object is a pojo

  ### Spring Bean - Any java object managed by spring
    Spring uses IOC container (Bean factory or Application Context) to manage thes objects 


##### List all beans managed by Spring
  ```java
  var context = new AnnotationConfigApplicationContext()
  Arrays.stream(context.getBeanDefinitionNames())
    .forEach(System.out::println);
  ```

##### What is multiple matching beans are available?
Using @Primary annotation to give priority to the matching beans 
If you want to use the other ones, then use @Qualifier("classname")
  ```java
  in main:
  System.out.println(context.getBean(Address.class));

  in config:
	@Bean(name = "address3")
	@Primary
	public Address address3() {
		return new Address("Morland", "Mumbai");
	}
  ```

Bean -> Class managed by Spring framework

To make function a bean - @Bean (applied only to the configuration class methods)
To make class a bean - @Component
ComponentScan("packagename")  - This is how spring finds the component classes
##### Dependency injection types
  Constructor based - on constructor injection @Autowired is not necessary
  Setter-based     - using @Autowired
  Field-based - using @Autowired
Spring team recommends constructor based injection
Dependency injection is a programming technique in which an object or function receives other objects or functions that it requires, as opposed to creating them internally


#### Annotations
  @Configuration - Indicates that a class declared one or more @Bean methods
  @ComponentScan - Define specific packages to scan for components. If package is not defined then it will search in its own package
  @Bean - Indicates that a method produces a bean to be managed by spring container
  @Component - Indicates that a whole class is a bean
  @Service - Specialisation of @Component indicating that the class has business logic
  @Controller - Specialisation of @Component indicating that the class has controller of rest api
  @Repository - Specialisation of @Component indicating that the class is used to perform crud on db
  @Primary  -  Indicates that a bean should be given preference
  @Qualifier  -  used on field or parameter
  @Lazy  -  Indicates that a bean has to be lazily initialized, absence of this will lead to eager initialization
  @Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE) - Many instance of object will be created
  @PostConstruct  -  Identifies the method that will be executed after dependency injection is done to perform any initialization
  @PreDestroy  -  Cleanup
  @Named   -   Jakarta Context and dependency injection(CDI) similar to @Component
  @Inject    -    Jakarta CDI similar to @Autowired

Spring-boot files:
	ErrorMvcAutoConfiguration

Profile config:
```java
	spring.profiles.active=dev (corresponds to file application-dev.properties)
	spring.profiles.active=prod (corresponds to file application-prod.properties)
```

Log (in application.properties):
```java
logging.level.org.springframework=debug
types: error info trace debug warning off
```

In maven build: 
	goals = clean install
	build path we got as C:\Users\Crypto\.m2\repository\com\example\springboot_basic\0.0.1-SNAPSHOT\springboot_basic-0.0.1-SNAPSHOT.jar
	java -jar springboot_basic-0.0.1-SNAPSHOT.jar

Make jar not war

##### Hibernate vs JPA
  - JPA defines the specification. It is an API
      - How to define entities?
      - How to map attributes?
      - Who manages the entity?
  - Hibernate is one of the popular implementation of JPA
  - Using Hibernate directly would result in a lock in to Hibernate
       - There are other implementations(eg. Toplink)


##### Java solution to solve null values:
```java
List<String> courses = List.of("Spring", "Azure", "Spring Framework", "AWS", "API", "Microservice", "Spring Boot", "Docker");

Predicate<? super String> predicate = course -> course.startsWith("A");
Optional<String> optional = courses.stream().filter(predicate).findFirst();

System.out.println(optional);
System.out.println(optional.isEmpty());
System.out.println(optional.isPresent());
System.out.println(optional.get());

Optional<String> empty = Optional.empty();
Optional<String> fruit = Optional.of("banana");

System.out.println(fruit);
```


##### To enable the swagger docs UI:
Access the api docs at - http://localhost:8080/swagger-ui.html 
Access the yaml file at -  http://localhost:8080/v3/api-docs
```xml
<dependency>
	<groupId>org.springdoc</groupId>
	<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
	<version>2.2.0</version>
</dependency>
```

##### To enable content negotiation:
  Dependency:
  ```xml
  <dependency>
	<groupId>com.fasterxml.jackson.dataformat</groupId>
	<artifactId>jackson-dataformat-xml</artifactId>
  </dependency>
  ```
    Add the HTTP request header based on the response format you want:
      Accept: application/xml
      Accept: application/json

  Changing language of the response:
     in messages.properties:
        
 
	Accept-Language: en   (For English)
	Accept-Language: nl   (For Dutch)
	Accept-Language: fr   (For French)


##### Versioning REST API options:
  - URI Versioning
      - http://localhost:8080/v1/person
      - http://localhost:8080/v1/person
  - Request Parameter Versioning
      - http://localhost:8080/person?version=1
      - http://localhost:8080/person?version=2
  - Custom Headers versioning
      - Same URL, headers=[X-API-VERSION=1]
      - Same URL, headers=[X-API-VERSION=2]
  - Media type versioning (content negotiation or Accept header)
      - Same URL, produces=application/vnd.company.app-v1+json
      - Same URL, produces=application/vnd.company.app-v2+json

##### Customizing API responses
  1. Customize field name in response
      @JsonProperty("user_name")
  2. Return only specified fields (filtering, eg. password)
       Two types:
        - Static Filtering: Same filtering for a bean across different REST API
            @JsonIgnoreProperties (on class, specify the field names), @JsonIgnore (on field)
        - Dynamic Filtering: Customize filtering for a bean for specific REST API
             @JsonFilter with FilterProvider (for this to work, add @JsonFilter on the class)