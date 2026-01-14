![[1.png]]

##### AEM Architecture

![[3.png]]

![[4.png]]

![[5.png]]

![[6.png]]

##### Typical AEM Architecture

![[11.png]]

![[7.png]]

![[8.png]]

#####  AEM as Cloud Service Architecture

![[9.png]]

![[10.png]]

![[12.png]]

![[13.png]]

![[14.png]]

![[15.png]]

![[16.png]]

![[17.png]]

![[18.png]]

![[19.png]]

![[20.png]]

##### Sling Models

![[21.png]]

![[23.png]]

```java

@Model(adapatables = Resource.class)
public class SecondComponentModel {

	@ValueMapValue
	private String title;
	
	@ValueMapValue
	private String text;
	
	// generate getters
}
```
![[22.png]]

JUnit:
```json
{
"data" : {
	"jcr:primaryType": "nt:unstructured",
	"jcr:createdBy": "admin",
	"eyebrow": "Eyebrow example",
	"header": "Header example",
	"jcr:lastModifiedBy": "admin",
	"jcr:created": "Mon Mar 25 2024 12:48:41 GMT+0530",
	"text": "Text example",
	"title": "Title example",
	"jcr:lastModified": "Mon Mar 25 2024 13:17:58 GMT+0530",
	"sling:resourceType": "practice/components/secondcomponent"
}
}
```

```java
@ExtendWith(AemContextExtension.class)
class SecondComponentModelTest{

	private final AemContext aemContext = new AemContext()
	SecondComponentModel secondComponentModel;
	
	@BeforeEach()
	void setup() throws Exception {
		aemContext.addModelsForClasses(SecondComponentModel.class);
		aemContext.load().json("/components/secondcomponent/SecondComponentModel.json", "/component"); // json stored in test/resources/components
		Resource resource = aemContext.resourceResolver().getResource("/component/data");
		secondComponentModel = aemContext.getService(ModelFactory.class).createModel(resource, SecondComponentModel.class);
	}
	
	@Test
	void testGetTitle(){
		String expected = "Title 1";
		String actual = secondComponentModel.getTitle();
		assertEquals(expected, actual);
	}
	
	@Test
	void testGetText(){
		String expected = "Text 1";
		String actual = secondComponentModel.getText();
		assertEquals(expected, actual);
	}
}
```

###### Sling model for multifield

```java
@Model(adapatables = Resource.class)
public class ThirdComponentModel {

	@ValueMapValue
	private String title;
	
	@ValueMapValue
	private String text;
	
	@ChildResource
	private List<ThirdComponentListModel> multifieldList;
	
	// generate getters
}

@Model(adapatables = Resource.class)
public class ThirdComponentListModel {

	@ValueMapValue
	private String eyebrow;
	
	@ValueMapValue
	private String header;
	
	// generate getters
}

```

![[25.png]]


```java
{
"data" : {
	"jcr:primaryType": "nt:unstructured",
	"jcr:createdBy": "admin",
	"jcr:lastModifiedBy": "admin",
	"jcr:created": "Thu Mar 28 2024 14:18:20 GMT+0530",
	"text": "Text example",
	"title": "Title example",
	"jcr:lastModified": "Thu Mar 28 2024 14:18:37 GMT+0530",
	"sling:resourceType": "practice/components/thirdcomponent",
	"multifieldList": {
		"jcr:primaryType": "nt:unstructured",
		"item0": {
			"jcr:primaryType": "nt:unstructured",
			"eyebrow": "Eyebrow 1",
			"header": "Header 1"
		},
		"item1": {
			"jcr:primaryType": "nt:unstructured",
			"eyebrow": "Eyebrow 2",
			"header": "Header 2"
		}
	}
}
}
```

```java
@ExtendWith(AemContextExtension.class)
class ThirdComponentModelTest {

	private final AemContext aemContext = new AemContext();
	
	ThirdComponentModel thirdComponentModel;
	
	@BeforeEach
	void setUp() throws Exception {
		aemContext.addModelsForClasses(ThirdComponentModel.class);
		aemContext.load().json("/components/thirdcomponent/ThirdComponentModel.json", "/component");
		Resource resource = aemContext.resourceResolver().getResource("/component/data");
		thirdComponentModel = aemContext.getService(ModelFactory.class).createModel(resource,
				ThirdComponentModel.class);
	}

	@Test
	void testGetTitle() {
		String expected = "Title example";
		String actual = thirdComponentModel.getTitle();
		assertEquals(expected,actual);
	}

	@Test
	void testGetText() {
		String expected = "Text example";
		String actual = thirdComponentModel.getText();
		assertEquals(expected,actual);
	}

	@Test
	void testGetMultifieldList() {
		String expectedEyebrow = "Eyebrow 1";
		String actualEyebrow = thirdComponentModel.getMultifieldList().get(0).getEyebrow();
		assertEquals(expectedEyebrow,actualEyebrow);
		
		String expectedHeader = "Header 1";
		String actualHeader = thirdComponentModel.getMultifieldList().get(0).getHeader();
		assertEquals(expectedHeader,actualHeader);
		
	}
}
```

##### Templates: 

![[24.png]]


