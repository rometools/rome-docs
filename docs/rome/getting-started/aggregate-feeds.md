# Aggregate feeds

To aggregate multiple feeds into one feed you only have to parse the desired
feeds and add their entries to a new feed:

```java
List<SyndEntry> entries = new ArrayList<SyndEntry>();

// parse feeds
SyndFeedInput input = new SyndFeedInput();
for (File file : files) {
    SyndFeed feed = input.build(file);
    entries.addAll(feed.getEntries());
}

// create aggregated feed
SyndFeed aggregationFeed = new SyndFeedImpl();
aggregationFeed.setFeedType("rss_2.0");
aggregationFeed.setTitle("Aggregator Feed");
aggregationFeed.setLink("https://example.org/feed");
aggregationFeed.setDescription("Description");
aggregationFeed.setPublishedDate(new Date());
aggregationFeed.setEntries(entries);

// output aggregated feed
String xml = new SyndFeedOutput().outputString(aggregationFeed);
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