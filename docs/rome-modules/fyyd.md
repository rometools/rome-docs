# fyyd

This is a ROME module that provides support for the [https://fyyd.de/fyyd-ns/] namespace.

By example, get the Fyyd verification token in RSS or Atom feed.

## Sample

```xml
<feed xmlns="http://www.w3.org/2005/Atom" xmlns:fyyd="https://fyyd.de/fyyd-ns/">
    <title>Lorem Ipsum</title>
    <link href="http://example.org"/>
    <fyyd:verify>abcdefg</fyyd:verify>
    <entry>
        <title>Lorem Ipsum</title>
        <link href="http://example.org/episode1"/>
        <link rel="enclosure"
              href="http://example.org/episode1.m4a" />
    </entry>
</feed>
```

## Create feed

```java
SyndEnclosure encl = new SyndEnclosureImpl();
encl.setUrl("http://example.org/episode1.m4a");

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Lorem Ipsum");
entry.setLink("http://example.org/episode1");
entry.getEnclosures().add(encl);

FyydModule fyyd = new FyydModuleImpl();
fyyd.setVerify("abcdefg");

SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("atom_1.0");
feed.setTitle("Lorem Ipsum");
feed.setLink("https://example.org");
feed.getEntries().add(entry);
feed.getModules().add(fyyd);

System.out.println(new SyndFeedOutput().outputString(feed));
```

## Read feed

```java
SyndFeedInput input = new SyndFeedInput();
SyndFeed feed = input.build(new XmlReader(new File("fyyd/atom.xml")));
FyydModule fyyd = (FyydModule) feed.getModule(FyydModule.URI);


System.out.println(fyyd.getVerify());
```
