# Convert feed

The process of converting a feed from one type to another involves parsing the
original feed, changing the feed type and writing the feed:

```java

# parse feed of any type
SyndFeedInput input = new SyndFeedInput();
SyndFeed feed = input.build(input);

# set desired type
feed.setFeedType("rss_2.0");

# write feed
SyndFeedOutput output = new SyndFeedOutput();
String xml = output.outputString(feed); 
```

For more details have a look at the guides for [reading](read-feed.md) and
[creating](create-feed.md) feeds.
