# Convert feed

The process of converting a feed from one type to another involves parsing the
original feed, updating the feed type and writing the feed:

```java
File file = new File("atom_1.0.xml");

// parse feed
SyndFeed feed = new SyndFeedInput().build(file);

// update feed type
feed.setFeedType("rss_2.0");

// write feed
String xml = new SyndFeedOutput().outputString(feed);
System.out.println(xml);
```

!!! Note 
    `SyndFeedInput` supports additional input types. Please take a look at the 
    Javadoc.

!!! note 
    `SyndFeedOutput` supports additional output formats. Please take a look at 
    the Javadoc.

For more details take a look at the guides for [reading](read-feed.md) and
[creating](create-feed.md) feeds.
