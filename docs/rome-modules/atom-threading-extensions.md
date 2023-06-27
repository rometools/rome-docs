# Atom Threading Extensions

This plugin is for use with Atom 1.0 elements inside RSS feeds

If you have an RSS feed that uses Atom extensions like link, author or
contributor, with Rome you can parse these elements

```xml
<rss xmlns:atom="http://www.w3.org/2005/Atom" version="2.0">
    <channel>
        <title>Lorem Ipsum</title>
        <link>http://example.org</link>
        <description>Lorem Ipsum</description>
        <atom:link href="http://test.com" type="application/rss+xml" rel="self" />
        <atom:link href="http://test.com/alt" type="application/rss+xml" rel="alternate" />
        <atom:author>
            <atom:name>Lorem Ipsum</atom:name>
            <atom:email>test@example.org</atom:email>
            <atom:uri>http://example.org</atom:uri>
        </atom:author>
        <atom:contributor>
            <atom:name>Lorem Ipsum</atom:name>
            <atom:email>test@example.org</atom:email>
            <atom:uri>http://example.org</atom:uri>
        </atom:contributor>
        <item>
            <title>Lorem Ipsum</title>
            <link>http://example.org/episode1</link>
            <description><![CDATA[Lorem Ipsum]]></description>
            <atom:author>
                <atom:name>Lorem Ipsum</atom:name>
                <atom:email>test@example.org</atom:email>
                <atom:uri>http://example.org</atom:uri>
            </atom:author>
            <atom:contributor>
                <atom:name>Lorem Ipsum</atom:name>
                <atom:email>test@example.org</atom:email>
                <atom:uri>http://example.org</atom:uri>
            </atom:contributor>
        </item>
    </channel>
</rss>
```

## Create feed

```java
Link link1 = new Link();
link1.setHref("http://test.com");
link1.setType("application/rss+xml");
link1.setRel("self");

SyndPerson author1 = new SyndPersonImpl();
author1.setName("Lorem Ipsum");
author1.setEmail("test@example.org");
author1.setUri("http://example.org");

SyndPerson contributor1 = new SyndPersonImpl();
contributor1.setName("Lorem Ipsum");
contributor1.setEmail("test@example.org");
contributor1.setUri("http://example.org");

AtomLinkModule atomLink = new AtomLinkModuleImpl();
atomLink.getLinks().add(link1);
atomLink.getAuthors().add(author1);
atomLink.getContributors().add(contributor1);

SyndContent description = new SyndContentImpl();
description.setValue("Lorem Ipsum");

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Lorem Ipsum");
entry.setDescription(description);
entry.setLink("http://example.org");
entry.getModules().add(atomLink);

SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("test-title");
feed.setDescription("test-description");
feed.setLink("https://example.org");
feed.getEntries().add(entry);
feed.getModules().add(atomLink);

System.out.println(new SyndFeedOutput().outputString(feed));
```

### Read feed

```java
SyndFeed feed = new SyndFeedInput().build(
    new XmlReader(new File("/foo_rss_atom.xml"))));

AtomLinkModule feedAtomModule = (AtomLinkModule) feed.getModule(AtomLinkModule.URI);
for (SyndPerson author : feedAtomModule.getAuthors()) {
    System.out.println(author.getEmail());
}
```
