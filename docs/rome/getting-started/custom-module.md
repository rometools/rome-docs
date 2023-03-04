# Create a custom module

This tutorial walks through the steps of creating an extension for feeds that
support additional namespaces. To understand this tutorial, you should be
familiar with ROME and the use of modules in feeds.

The goal of this tutorial is to extend an RSS 2.0 feed with the following 3
additional elements:

- `foo` may occur several times
- `bar` may occur at most once
- `date` may occur at most once

Such a feed could look like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:sample="https://example.org/module/sample/1.0" version="2.0">

  <channel>

    <title>Feed title</title>
    <link>https://example.org/feed</link>
    <description>Feed description</description>
    <sample:bar>Feed bar</sample:bar>
    <sample:date>2023-03-18T17:04:07Z</sample:date>

    <item>
      <title>Item title</title>
      <link>https://example.org/item</link>
      <description>Item description</description>
      <sample:foo>Item foo 1</sample:foo>
      <sample:foo>Item foo 2</sample:foo>
    </item>

  </channel>

</rss>
```

## Module

First we start with the `SampleModule` interface that has to extend the `Module`
interface. The URI and namespace of the module are defined as constants. This is
for convenience and good practice only. In addition, the interface of the new
module defines getters and setters for the new properties:

```java
public interface SampleModule extends Module {

	String URI = "https://example.org/module/sample/1.0";

	Namespace NAMESPACE = Namespace.getNamespace("sample", URI);

	List<String> getFoos();

	void setFoos(List<String> foos);

	String getBar();

	void setBar(String bar);

	Date getDate();

	void setDate(Date date);

}
```

Next we have to implement the interface. `SampleModuleImpl` extends `ModuleImpl`
which provides default implementations for the `getUri`, `equals`, `hashCode`,
`toString` and `clone` methods of the `Module` interface.

The module properties are standard Java Bean properties. The only catch is that
if a Collection is `null` the getter has to return an empty Collection.
Furhermore, only reference-types may be used.

The weird part are the bits for the `CopyFrom` (inherited through the `Module`
interface) logic. The
[CopyFrom interface documentation](../technical-docs/copyfrom-interface.md)
explains its logic in detail. In short `CopyFrom` enables to copy properties
from one implementation of an interface to another implementation of the
interface. The `getInterface()` method returns the interface of the
implementation (necessary for collections containing subclasses such as a list
of modules) and the `copyFrom()` method copies all the properties from the
parameter object (which must be an implementation of the interface) into the
caller bean properties. Note that your implementation has to make a deep copy.

```java
public class SampleModuleImpl extends ModuleImpl implements SampleModule {

	private List<String> foos;
	private String bar;
	private Date date;

	public SampleModuleImpl() {
		super(SampleModule.class, SampleModule.URI);
	}

	public List<String> getFoos() {
		return Lists.createWhenNull(this.foos);
	}

	public void setFoos(List<String> foos) {
		this.foos = foos;
	}

	public String getBar() {
		return this.bar;
	}

	public void setBar(String bar) {
		this.bar = bar;
	}

	public Date getDate() {
		return this.date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

	public Class<? extends CopyFrom> getInterface() {
		return SampleModule.class;
	}

	public void copyFrom(CopyFrom obj) {
		SampleModule module = (SampleModule) obj;
		setFoos(new ArrayList<String>(module.getFoos()));
		setBar(module.getBar());
		setDate((Date) module.getDate().clone());
	}

}
```

## Parser

To be able to parse the new properties, we have to create an implementation of
the `ModuleParser` interface that defines the following methods:

- `getNamespaceUri()` returns the URI of the module 
- `parse(Element, Locale)` extracts the new properties from the given element

The parser will be invokek with a feed element or an item element and has to
extract the new properties from the children of the given element.

When any of our newly supported elements was found, the parser returns an
instance of our module, otherwise `null`. This is to avoid having an empty
instance of a module that is not present in the feed or its entries.

```java
public class SampleModuleParser implements ModuleParser {

	public String getNamespaceUri() {
		return SampleModule.URI;
	}

	@Override
	public Module parse(Element element, Locale locale) {

		boolean match = false;

		SampleModule module = new SampleModuleImpl();

		List<Element> fooElements = element.getChildren("foo", SampleModule.NAMESPACE);
		if (!fooElements.isEmpty()) {
			match = true;
			List<String> foos = new ArrayList<>();
			for (Element fooElement : fooElements) {
				foos.add(fooElement.getText());
			}
			module.setFoos(foos);
		}

		Element barElement = element.getChild("bar", SampleModule.NAMESPACE);
		if (barElement != null) {
			match = true;
			module.setBar(barElement.getText());
		}

		Element dateElement = element.getChild("date", SampleModule.NAMESPACE);
		if (dateElement != null) {
			match = true;
			module.setDate(DateParser.parseW3CDateTime(dateElement.getText(), locale));
		}

		return match ? module : null;

	}

}
```

## Generator

To be able to generate feeds with the new elements, we have to implement the
`ModuleGenerator` interface that defines the following methods:

- `getNamespaceUri()` returns the URI of the module
- `getNamespaces()` returns the namespaces that should be added into the feed
- `generate(ModuleInterface, Element)` injects the module data into the given
  element

The generator will be invoked with a feed element or an item element and injects
the module properties into the given element. This injection has to be done
using the right namespace. The set of namespaces returned by the
`getNamespaces()` method will be used to declare the namespaces used by the
module in the feed root element.

If our module is not existing in the feed bean, our generator is not invoked at
all.

```java
public class SampleModuleGenerator implements ModuleGenerator {

	private static final Set<Namespace> NAMESPACES;

	static {

		Set<Namespace> namespaces = new HashSet<>();
		namespaces.add(SampleModule.NAMESPACE);

		NAMESPACES = Collections.unmodifiableSet(namespaces);

	}

	public String getNamespaceUri() {
		return SampleModule.URI;
	}

	public Set<Namespace> getNamespaces() {
		return NAMESPACES;
	}

	public void generate(Module module, Element element) {

		// add namespace definition to feed and item
		Element root = element;
		while (root.getParent() != null && root.getParent() instanceof Element) {
			root = (Element) element.getParent();
		}
		root.addNamespaceDeclaration(SampleModule.NAMESPACE);

		SampleModule sampleModule = (SampleModule) module;

		List<String> foos = sampleModule.getFoos();
		for (String foo : foos) {
			element.addContent(generateElement("foo", foo.toString()));
		}
		if (sampleModule.getBar() != null) {
			element.addContent(generateElement("bar", sampleModule.getBar()));
		}
		if (sampleModule.getDate() != null) {
			element.addContent(generateElement("date", DateParser.formatW3CDateTime(sampleModule.getDate(), Locale.US)));
		}

	}

	private Element generateElement(String name, String value) {
		Element element = new Element(name, SampleModule.NAMESPACE);
		element.addContent(value);
		return element;
	}

}
```

## Configuration

The last step is to register our parser and/or generator in a Properties file
called `rome.properties`. The file has to be in the root of the modules
classpath.

The registration indicates:

- which feed formats (only RSS 2.0 in this example) are supported
- whether parsing and/or generating of feed and/or item elements is supported

```properties
# register feed element parser
rss_2.0.feed.ModuleParser.classes=sample.SampleModuleParser

# register item element parser
rss_2.0.item.ModuleParser.classes=sample.SampleModuleParser

# register feed element generator
rss_2.0.feed.ModuleGenerator.classes=sample.SampleModuleGenerator

# register item element generator
rss_2.0.item.ModuleGenerator.classes=sample.SampleModuleGenerator
```

If you want to register multiple modules, the classes have to be separated by
commas or spaces.

## Usage

### Generate feed

This is the code that was used to generate the example on top of the page:

```java
// create entry
SampleModuleImpl entryModule = new SampleModuleImpl();
entryModule.setFoos(Arrays.asList("Foo 1", "Foo 2"));

SyndContent content = new SyndContentImpl();
content.setType("text/html");
content.setValue("<p>Content</p>");

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Title");
entry.setLink("https://example.org/entry");
entry.setDescription(content);
entry.setPublishedDate(new Date());
entry.getModules().add(entryModule);

// assemble feed
SampleModuleImpl feedModule = new SampleModuleImpl();
feedModule.setBar("bar");
feedModule.setDate(new Date());

SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("Title");
feed.setLink("https://example.org/feed");
feed.setDescription("Description");
feed.setPublishedDate(new Date());
feed.setEntries(Arrays.asList(entry));
feed.getModules().add(feedModule);

// output feed
String xml = new SyndFeedOutput().outputString(feed);
System.out.println(xml);
```

### Parse feeds

This is an example how to extract the new fields:

```java
File file = new File("feed.xml");
SyndFeed feed = new SyndFeedInput().build(file);

// get properties from feed
SampleModule feedModule = (SampleModule) feed.getModule(SampleModule.URI);
System.out.println(feedModule.getFoos());
System.out.println(feedModule.getBar());
System.out.println(feedModule.getDate());

for (SyndEntry entry : feed.getEntries()) {
	// get properties from entries
	SampleModule entryModule = (SampleModule) entry.getModule(SampleModule.URI);
	System.out.println(entryModule.getFoos());
	System.out.println(entryModule.getBar());
	System.out.println(entryModule.getDate());
}
```
