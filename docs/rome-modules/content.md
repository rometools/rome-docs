# Content

The [Content Module](http://purl.org/rss/1.0/modules/content/) was developed as
a supplement for the RSS description element that is often used to transport
HTML contents even though it was never intended for this. The content:encoded
element was specifically designed to transport HTML content either
entity-encoded or [CDATA](https://en.wikipedia.org/wiki/CDATA)-escaped.

## Sample

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:content="http://purl.org/rss/1.0/modules/content/" version="2.0">
  <channel>
    <title>Title</title>
    <description>Description</description>
    <link>https://example.org/feed</link>
    <item>
      <title>Title</title>
      <description>Description</description>
      <link>https://example.org/entry</link>
      <guid>https://example.org/entry</guid>
      <content:encoded><![CDATA[<p>HTML</p>]]></content:encoded>
    </item>
  </channel>
</rss>
```

## Create feed

```java
// create entry
SyndContent description = new SyndContentImpl();
description.setValue("Description");

ContentModuleImpl content = new ContentModuleImpl();
content.setEncodeds(Arrays.asList("<p>HTML</p>"));

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Title");
entry.setDescription(description);
entry.setLink("https://example.org/entry");
entry.getModules().add(content);

// assemble feed
SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("Title");
feed.setDescription("Description");
feed.setLink("https://example.org/feed");
feed.getEntries().add(entry);

// output feed
String xml = new SyndFeedOutput().outputString(feed);
```

## Read feed

```java
Reader reader = new StringReader(xml);
SyndFeed feed = new SyndFeedInput().build(reader);

for (SyndEntry entry : feed.getEntries()) {

    ContentModule module = (ContentModule) entry.getModule(ContentModule.URI);
  
    for (String encoded : module.getEncodeds()) {
        System.out.println(encoded);
    }

}
```