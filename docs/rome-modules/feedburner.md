# FeedBurner

On June 3, 2007, FeedBurner was acquired by Google. On May 26, 2011, Google
announced that the FeedBurner APIs were deprecated. Google shut down the APIs on October 20, 2012.

Google “retired” AdSense for Feeds on October 2, 2012 and shut it down on December 3, 2012.

FeedBurner [module|http://rssnamespace.org/feedburner/ext/1.0] implementation allows to include
special elements into RSS feeds and entries

You can include awareness, origLink and origEnclosureLink elements in your feed.

## Sample

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:feedburner="http://rssnamespace.org/feedburner/ext/1.0">
  <channel>
    <title>Lorem Ipsum</title>
    <link>http://example.org</link>
    <description>Lorem Ipsum</description>
    <item>
      <title>Lorem Ipsum</title>
      <link>http://example.org/episode1</link>
      <feedburner:awareness>false</feedburner:awareness>
      <feedburner:origLink>http://orig.example.org</feedburner:origLink>
      <feedburner:origEnclosureLink>http://enclosure.example.org</feedburner:origEnclosureLink>
      <description>Lorem Ipsum</description>
      <guid isPermaLink="false">http://example.org/episode1</guid>
    </item>
  </channel>
</rss>
```

## Create feed

```java
FeedBurner feedBurner = new FeedBurnerImpl();
feedBurner.setAwareness("false");
feedBurner.setOrigLink("http://orig.example.org");
feedBurner.setOrigEnclosureLink("http://enclosure.example.org");

SyndContent description = new SyndContentImpl();
description.setValue("Lorem Ipsum");

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Lorem Ipsum");
entry.setDescription(description);
entry.setLink("http://example.org");
entry.getModules().add(feedburner);

SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("test-title");
feed.setDescription("test-description");
feed.setLink("https://example.org");
feed.getEntries().add(entry);

System.out.println(new SyndFeedOutput().outputString(feed));
```

## Read feed

```java
SyndFeedInput input = new SyndFeedInput();
SyndFeed feed = input.build(new XmlReader(new File("feedburner/rss.xml")));
feed.getModule(FeedBurner.URI);
SyndEntry entry = feed.getEntries().get(0);
entry.getModule(FeedBurner.URI);
SyndFeedOutput output = new SyndFeedOutput();
StringWriter writer = new StringWriter();
output.output(feed, writer);

System.out.println(writer);
```
