Micro Java Web Framework
=====================

[![Travis CI Build Status](https://travis-ci.org/decebals/pippo.png)](https://travis-ci.org/decebals/pippo)
[![Maven Central](http://img.shields.io/maven-central/v/ro.pippo/pippo.svg)](http://search.maven.org/#search|ga|1|ro.pippo)

It's an open source (Apache License) micro web framework in Java, with minimal dependencies and a quick learning curve.     
The goal of this project is to create a micro web framework in Java that should be easy to use and hack.      

Sample code
---------------
First we must create an __Application__ and add some routes:  

```java
public class BasicApplication extends Application {

    @Override
    protected void onInit() {
		// send 'Hello World' as response
        GET("/", (routeContext) -> routeContext.send("Hello World"));

		// send a file as response
        GET("/file", (routeContext) -> routeContext.send(new File("pom.xml"));

        // send a json as response
        GET("/json", (routeContext) -> {
			Contact contact = createContact();
			routeContext.json().send(contact);
        });

        // send xml as response
        GET("/xml", (routeContext) -> {
			Contact contact = createContact();
			routeContext.xml().send(contact);
        });
        
        // send an object and negotiate the Response content-type, default to XML
        GET("/negotiate", (routeContext) -> {
			routeContext.xml().negotiateContentType().send(contact);
        });
        
        // send a template as response
        GET("/template", (routeContext) -> {
			routeContext.setLocal("greeting", "Hello");
			routeContext.render("hello");        
		});
    }

	private Contact createContact() {
		return new Contact()
			.setId(12345)
			.setName("John")
			.setPhone("0733434435")
			.setAddress("Sunflower Street, No. 6");	
	}
	
}
``` 

where `Contact` is a simple POJO:

```java
public class Contact  {

    private int id;
    private String name;
    private String phone;
    private String address;
    
    // getters and setters

}
```

The last step it's to start __Pippo__ with your application as parameter:

```java
public class BasicDemo {

    public static void main(String[] args) {
        Pippo pippo = new Pippo(new BasicApplication());
        pippo.start();
    }

}
```

Pippo launches the embedded web server (found in your classpath) and makes the application available on port `8338` (default value).
Open your internet browser and check the routes declared in Application:

 - `http://localhost:8338`
 - `http://localhost:8338/file`
 - `http://localhost:8338/json`
 - `http://localhost:8338/xml`
 - `http://localhost:8338/negotiate`
 - `http://localhost:8338/template` 

Documentation
---------------
Documentation is available on [www.pippo.ro](http://www.pippo.ro)
