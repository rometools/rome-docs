# Create feed

ROME represents feeds as instances of the `SyndFeed` interface. The SyndFeed
interface and its properties follow the JavaBean specification.

## RSS 2.0 example
```java
// create entry
SyndContent content = new SyndContentImpl();
content.setType("text/html");
content.setValue("<p>Content</p>");

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Title");
entry.setLink("https://example.org/entry");
entry.setDescription(content);
entry.setPublishedDate(new Date());

// assemble feed
SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("Title");
feed.setLink("https://example.org/feed");
feed.setDescription("Description");
feed.setPublishedDate(new Date());
feed.setEntries(Arrays.asList(entry));

// output feed
String xml = new SyndFeedOutput().outputString(feed);
System.out.println(xml);
```

!!! note 
    `SyndFeedOutput` supports additional output formats. Please take a look at 
    the Javadoc.

## Atom 1.0 example
```java
// create entry
SyndContent content = new SyndContentImpl();
content.setType("text/html");
content.setValue("<p>Content</p>");

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Title");
entry.setUri("https://example.org/entry");
entry.setContents(Arrays.asList(content));
entry.setPublishedDate(new Date());

// assemble feed
SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("atom_1.0");
feed.setTitle("Title");
feed.setUri("https://example.org/feed");
feed.setDescription("Description");
feed.setPublishedDate(new Date());
feed.setEntries(Arrays.asList(entry));

// output xml
String xml = new SyndFeedOutput().outputString(feed);
System.out.println(xml);
```

!!! note 
    `SyndFeedOutput` supports additional output formats. Please take a look at 
    the Javadoc.

## Supported feed types

Besides RSS 2.0 and Atom 1.0 ROME supports various other (outdated) formats:

- atom_0.3 (Atom 0.3)
- atom_1.0 (Atom 1.0)
- rss_0.9 (RSS 0.90)
- rss_0.91U (RSS 0.91 Userland)
- rss_0.91N (RSS 0.91 Netscape)
- rss_0.92 (RSS 0.92)
- rss_0.93 (RSS 0.93)
- rss_0.94 (RSS 0.94)
- rss_1.0 (RSS 1.0)
- rss_2.0 (RSS 2.0)
