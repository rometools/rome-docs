# FeedPress

The FeedPress module ([namespace|http://feed.press]) allows to specify custom
elements based on FeedPress public namespace. You have four elements available:
newsletterId, locale, podcastId and cssFile.

## Sample

```xml
<rss xmlns:feedpress="https://feed.press/xmlns" version="2.0">
    <channel>
        <title>Lorem Ipsum</title>
        <link>http://example.org</link>
        <description>Lorem Ipsum</description>
        <feedpress:newsletterId>abc123</feedpress:newsletterId>
        <feedpress:locale>en</feedpress:locale>
        <feedpress:podcastId>xyz123</feedpress:podcastId>
        <feedpress:cssFile>http://example.org/style.css</feedpress:cssFile>
        <item>
            <title>Lorem Ipsum</title>
            <link>http://example.org/episode1</link>
            <description><![CDATA[Lorem Ipsum]]></description>
        </item>
    </channel>
</rss>
```

## Create feed

```java
SyndContent description = new SyndContentImpl();
description.setValue("Lorem Ipsum");

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Lorem Ipsum");
entry.setDescription(description);
entry.setLink("http://example.org");

FeedpressModule feedpress = new FeedpressModuleImpl();
feedpress.setCssFile("http://example.org/style.css");
feedpress.setLocale("en");
feedpress.setNewsletterId("abc123");
feedpress.setPodcastId("xyz123");

SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("test-title");
feed.setDescription("test-description");
feed.setLink("https://example.org");
feed.getEntries().add(entry);
feed.getModules().add(feedpress);

System.out.println(new SyndFeedOutput().outputString(feed));
```

## Read feed

```java
SyndFeedInput input = new SyndFeedInput();
SyndFeed feed = input.build(new XmlReader(new File(getTestFile("feedpress/rss.xml"))));
FeedpressModule feedpress = (FeedpressModule) feed.getModule(FeedpressModule.URI);

System.out.println(feedpress.getCssFile());
```
