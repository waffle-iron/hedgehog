[![Stories in Ready](https://badge.waffle.io/danjee/hedgehog.png?label=ready&title=Ready)](https://waffle.io/danjee/hedgehog)
# Hedgehog

### Hedgehog is a Spring, Guice and Weld CDI bean injector in non-managed business java classes. 

##### Inspired from Wicket injectors

##### Use it if you want to inject some managed components in legacy classes or classes that you simple do not want to have it managed like Swing or JavaFX panels

## Usage sample:

### Spring

Use the dependency:

```xml
<dependency>
	<groupId>ro.fortsoft</groupId>
	<artifactId>hedgehog-spring</artifactId>
	<version>0.0.1</version>
</dependency>
```

The configuration class:

```java
@Configuration
@ComponentScan(basePackages = "ro.fortsoft.beans")
public class AppConfig {

}
```

The bean:

```java
@Component
public class Child {
	@Override
	public String toString() {
		return "I'm a child";
	}
}
```
A business panel (non managed bean)

```java
public class Panel {

	@Sting
	private Child child;
	
	public Panel(){
		Injector.get().inject(this);
	}
	
	public void test() {
		System.out.println(child);
	}
}
```

Main class
```java
public class App implements StingAwareApplication {

	private MetaDataEntry<?>[] metaData;

	public static void main(String[] args) {
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
		SpringComponentStinger stinger = new SpringComponentStinger(context);
		App app = new App();
		stinger.bind(app);
		Panel panel = new Panel();
		panel.test();
	}

	public Injector getMetaData(MetaDataKey<Injector> key) {
		return key.get(metaData);
	}

	public void setMetaData(final MetaDataKey<Stinger> key, final Object object) {
		metaData = key.set(metaData, object);
	}
}
```

### Guice

Use the dependency:

```xml
<dependency>
	<groupId>ro.fortsoft</groupId>
	<artifactId>hedgehog-guice</artifactId>
	<version>0.0.1</version>
</dependency>
```

The Guice module class:

```java
public class GuiceModule extends AbstractModule {

	@Override
	protected void configure() {
		bind(ContactService.class).to(InMemoryContactService.class).asEagerSingleton();
	}
}
```

The beans:

```java
public interface ContactService {
	
	String add(String word);
	
	void modify(String word);
	
	void delete(String word);
}
```

```java
public class InMemoryContactService implements ContactService {

	public String add(String word) {
		// TODO Auto-generated method stub
		return null;
	}

	public void modify(String word) {
		// TODO Auto-generated method stub
		
	}

	public void delete(String word) {
		// TODO Auto-generated method stub
		
	}
}

```

A business panel (non managed bean)

```java
public class Panel {

	@Sting
	private ContactService contactService;
	
	public Panel(){
		Stinger.get().sting(this);
	}
	
	public void test(){
		System.out.println(contactService);
	}
}

```

Main class
```java
public class App implements StingAwareApplication {
	
	private MetaDataEntry<?>[] metaData;
	
	public static void main(String[] args) {
		 Injector injector = Guice.createInjector(new GuiceModule());
		 GuiceComponentsStinger stinger = new GuiceComponentsStinger(injector);
		 App app = new App();
		 stinger.bind(app);
		 Panel panel = new Panel();
		 panel.test();
	}

	public Stinger getMetaData(MetaDataKey<Stinger> key) {
		return key.get(metaData);
	}

	public void setMetaData(MetaDataKey<Stinger> key, Object object) {
		metaData = key.set(metaData, object);
	}
}
```

### Weld

Use the dependency:

```xml
<dependency>
	<groupId>ro.fortsoft</groupId>
	<artifactId>hedgehog-weld</artifactId>
	<version>0.0.1</version>
</dependency>
```

The service class

```java
public class Child {
	@Override
	public String toString() {
		return "I'm a child";
	}
}
```

A business panel (non managed bean)

```java
public class Panel {

	@Sting
	private Child child;
	
	public Panel(){
		Stinger.get().sting(this);
	}
	
	public void test() {
		System.out.println(child);
	}
}
```

Main class

```java
public class Main implements StingAwareApplication {

	private MetaDataEntry<?>[] metaData;

	public static void main(String[] args) {
		Weld weld = new Weld();
		WeldContainer container = weld.initialize();

		WeldComponentStinger stinger = new WeldComponentStinger(container);
		Main test = new Main();
		stinger.bind(test);
		Panel panel = new Panel();
		panel.test();
	}

	public Stinger getMetaData(MetaDataKey<Stinger> key) {
		return key.get(metaData);
	}

	public void setMetaData(final MetaDataKey<Stinger> key, final Object object) {
		metaData = key.set(metaData, object);
	}
}
```

## Versioning

Hedgehog will be maintained under the Semantic Versioning guidelines as much as possible.

Releases will be numbered with the follow format:

`<major>.<minor>.<patch>`

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major
* New additions without breaking backward compatibility bumps the minor
* Bug fixes and misc changes bump the patch

For more information on SemVer, please visit http://semver.org.