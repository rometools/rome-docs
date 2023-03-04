# Read feed

ROME parses syndication feeds into `SyndFeed` instances. The developer does not
need to worry about selecting the right parser for a feed, the `SyndFeedInput`
will take care of it by peeking at the feed structure. All it takes to read a
feed using ROME are the following lines of code:

```java
File file = new File("feed.xml");
SyndFeed feed = new SyndFeedInput().build(file);
System.out.println(feed);
```

!!! Note 
    `SyndFeedInput` supports additional input types. Please take a look at the 
    Javadoc.
