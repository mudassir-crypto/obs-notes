
###### Servlets
An AEM Servlet is a Java class in Adobe Experience Manager that handles HTTP requests and generates dynamic responses, acting as a server-side script to process data, interact with backend systems, registered as an OSGi service for modularity and dynamic management. They extend Sling servlets (like SlingSafeMethodsServlet or SlingAllMethodsServlet) and use OSGi properties (like sling.servlet.paths) to map to specific URLs, allowing for custom endpoints beyond standard page rendering.

![[26.png]]

![[27.png]]

GET -> Sling Safe
POST -> Sling ALL 

![[28.png]]

![[29.png]]

Example of resource resolver in servlet:

![[30.png]]

Example of resource resolver in model:

![[31.png]]
![[32.png]]
![[33.png]]


Resource Resolver using Resource Resolver Factory:

```
create a system user
give all permissions in useradmin
In configMgr, go to apache sling service user mapping
practice.core:resolver_factory_service=tutorial-system-user

service is now associated with the user with correct level of permissions
mapping is mandatory to avoid unrestricted access
```
![[34.png]]

![[35.png]]


###### Sling servlet by resourceType

![[36.png]]

Try accessing:
```
http://localhost:4502/content/practice/us/en/simple-page/jcr:content.txt
http://localhost:4502/content/practice/us/en/simple-page/jcr:content.aem-tutorial.txt (if selector is mentioned in servlet)
```
It will work same in inherited resourceTypes
practice/components/page => practice/components/pagev3


###### Sling servlet by Path

![[37.png]]
```
http://localhost:4502/bin/aemtutorial => will return HTTP response
```
![[38.png]]
```
Certain paths are allowed by aem but path starting from /aem is not allowed, we need to configure the path in sling
Go to configMgr => Apache Sling Servlet resolver => add /aem in execution paths
```

Sling servlet POST:

![[39.png]]


###### Debugging in AEM

```
private static final Logger log = LoggerFactory.getLogger(SimpleServlet.class);

logger.info();

logs will be printed in error.log
```

AEM in debugging mode:
```bat
replace one line 25 in start.bat

if not defined CQ_JVM_OPTS set CQ_JVM_OPTS=-Xms4G -Xmx4G -Djava.awt.headless=true -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=30303

After this, go to eclipse, open SimpleServlet.java
Right Click -> Debug As -> Debug Config -> Remote Java Application -> New Launch Config -> Change Port -> 30303 (pointed to our debugger set in start.bat)

Add breakpoints in SimpleServlet.java -> hit http://localhost:4502/content/practice/us/en/simple-page/jcr:content.aem-tutorial.txt

eclipse will open -> hit f6 (it will go through each and every line) -> hover over the objects, it will show what its holding in it or
select an object, right click and click on inspect

to manually end the debug, click on disconnect on top
```

![[40.png]]

###### Servlets Advanced

![[41.png]]
![[42.png]]

Node Modification:
```java
@Component(service = { Servlet.class })
@SlingServletResourceTypes(resourceTypes = "practice/components/page", selectors = "aem-tutorial-api", methods = HttpConstants.METHOD_POST, extensions = "txt")
@ServiceDescription("Simple Demo Servlet")
public class NodeModificationServlet extends SlingAllMethodsServlet {

	private static final long serialVersionUID = 1L;

	@Override
	protected void doPost(final SlingHttpServletRequest req, final SlingHttpServletResponse resp)
			throws ServletException, IOException {
		final Resource resource = req.getResource();
		ResourceResolver resolver = req.getResourceResolver();
		Session session = resolver.adaptTo(Session.class);
		try {
			Node node = session.getNode(resource.getPath());
			if(node.hasProperty("jcr:title")) {
			node.setProperty("jcr:title", "Title set from Node Modification Servlet");
			session.save();
		}
		} catch (PathNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (RepositoryException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		resp.setContentType("text/plain");
		resp.getWriter().write("Title from Post Servlet = " + resource.getValueMap().get(JcrConstants.JCR_TITLE));
	}
}
```

WCM API:

![[43.png]]
```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getPage("/content/practice/us/en/hello-world");
```

Servlet Component:

```html
<div class="my-button-component" data-current-resource="${resource.path}">
 <h1 class="header">${properties.header}</h1>
 <h3 class="title">${properties.title}</h3>
 <button id="myButton" type="button" onclick="myButtonFunction()">${properties.approvebuttonlabel}</button>

</div>

<sly data-sly-use.clientlib="/libs/granite/sightly/templates/clientlib.html"
> 
<sly data-sly-call="${clientlib.all @ categories='aem.tutorials'}"
/>
```
```js
function myButtonFunction() {

  const servletResource = $(".my-button-component").data("current-resource");

  var settings = {
        url: servletResource + ".buttoncomponent.html",
        method: "POST",
      };

      $.ajax(settings)
        .done(function (response) {
          console.log("Success")
        })
        .fail((response) => {
          console.log("Failure")
        });
}
```

```java
@Component(service = { Servlet.class })
@SlingServletResourceTypes(resourceTypes = "practice/components/servletcomponent", selectors = "buttoncomponent", methods = HttpConstants.METHOD_GET)
@ServiceDescription("Simple Demo Servlet")
public class SimpleComponentServlet extends SlingAllMethodsServlet {

	private static final long serialVersionUID = 1L;

	@Override
	protected void doGet(final SlingHttpServletRequest req, final SlingHttpServletResponse resp)
			throws ServletException, IOException {
		final Resource resource = req.getResource();
		String statusFlag = req.getParameter("statusFlag");
		ResourceResolver resolver = req.getResourceResolver();
		Session session = resolver.adaptTo(Session.class);
		try {
			Node node = session.getNode(resource.getPath());
			resp.setContentType("text/plain");
			resp.getWriter().write("Status Flag = " + statusFlag);
		} catch (PathNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (RepositoryException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

###### QueryBuilder

Query Builder is a server-side framework and API used to create and execute search queries against the JCR 

![[44.png]]

Basic Query:
```python
type=cq:Page
path=/content/practice/us/en
p.limit=-1
property=jcr:content/cq:template
property.value=/conf/practice/settings/wcm/templates/page-tuts
```

QueryBuilder API:
```java
QueryBuilder queryBuilder = resolver.adaptTo(QueryBuilder.class);
Query query = queryBuilder.createQuery(PredicateGroup.create(map), session);
SearchResult searchResult = query.getResult();
List<Hit> hits = searchResult.getHits();
```

QueryBuilder Servlet:
```java
@Component(service = { Servlet.class })
@SlingServletResourceTypes(resourceTypes = "practice/components/page", selectors = "query-builder", methods = HttpConstants.METHOD_POST)
@ServiceDescription("Simple Demo Servlet")
public class QueryBuilderServlet extends SlingAllMethodsServlet {

	private static final long serialVersionUID = 1L;

	@Override
	protected void doPost(final SlingHttpServletRequest req, final SlingHttpServletResponse resp)
			throws ServletException, IOException {
		final Resource resource = req.getResource();
		ResourceResolver resolver = req.getResourceResolver();
		Session session = resolver.adaptTo(Session.class);
		try {
			JsonArray resultArray = new JsonArray();
			Map<String, String> map = new HashMap<>();
			map.put("type", "cq:Page");
			map.put("path", "/content/practice/us/en");
			map.put("p.limit", "-1");
			map.put("property", "jcr:content/cq:template");
			map.put("property.value", "/conf/practice/settings/wcm/templates/tutorial-template");
			QueryBuilder queryBuilder = resolver.adaptTo(QueryBuilder.class);
			Query query = queryBuilder.createQuery(PredicateGroup.create(map), session);
			SearchResult searchResult = query.getResult();
			List<Hit> hits = searchResult.getHits();
			for (Hit hit : hits) {
				JsonObject pageObj = new JsonObject();
				pageObj.addProperty("path", hit.getPath());
				resultArray.add(pageObj);
			}
			resp.setContentType("application/json");
			resp.getWriter().write(resultArray.toString());
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (RepositoryException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			if (session != null) {
				session.logout();
			}
		}
	}
}
```

Exercise to retrieve which page contains our component:
```java
JsonArray resultArray = new JsonArray();
Map<String, String> map = new HashMap<>();
map.put("type", "nt:unstructured");
map.put("path", "/content/practice/us/en");
map.put("p.limit", "-1");
map.put("property", "sling:resourceType");
map.put("property.value", "practice/components/servletcomponent");
QueryBuilder queryBuilder = resolver.adaptTo(QueryBuilder.class);
Query query = queryBuilder.createQuery(PredicateGroup.create(map), session);
SearchResult searchResult = query.getResult();
List<Hit> hits = searchResult.getHits();
	for (Hit hit : hits) {
		String hitPath = hit.getPath();
		String pagePath = hitPath.split("/jcr:content")[0];
		JsonObject pageObj = new JsonObject();
		pageObj.addProperty("path", pagePath);
		resultArray.add(pageObj);

	}
resp.setContentType("application/json");
resp.getWriter().write(resultArray.toString());
```

##### Indexing in AEM
Indexing is used to increase the performance of the query and reduce the overhead on AEM i.e it improves the speed of data retrieval operations.

###### Types of Indexing - 

**Synchronous Indexing**
![[45.png]]

**Asynchronous Indexing**
![[46.png]]


![[47.png]]

Custom Index:

Create an index in /oak:index/slingResourceTypeIndex
![[48.png]]

after this, then refresh and check if indexRules is created. if not then restart agn
rename nt:base => nt:structured
rename prop0 => sling:resourceType
  isRegExp: true
  propertyIndex:true
  ordered:false
On slingResourceTypeIndex, reindex:true
