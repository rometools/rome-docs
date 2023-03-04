# Aggregate feeds

To aggregate multiple feeds into one feed you only have to parse the desired
feeds and add their entries to a new feed:

```java
List<SyndEntry> entries = new ArrayList<SyndEntry>();

# parse feeds / repeat for every feed
File file = new File("feed.xml");
SyndFeedInput input = new SyndFeedInput();
SyndFeed feed = input.build(file);
entries.addAll(feed.getEntries());

# create aggregated feed
SyndFeed aggregationFeed = new SyndFeedImpl();
aggregationFeed.setFeedType("rss_2.0");
aggregationFeed.setTitle("Aggregator");
aggregationFeed.setLink("https://www.example.com");
aggregationFeed.setDescription("Description");
aggregationFeed.setEntries(entries);

# write aggregated feed
SyndFeedOutput output = new SyndFeedOutput();
String xml = output.outputString(aggregationFeed);
```

For more details have a look at the guides for [reading](read-feed.md) and
[creating](create-feed.md) feeds.