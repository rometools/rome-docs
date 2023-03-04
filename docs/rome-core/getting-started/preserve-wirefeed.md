# Preserve WireFeed

!!! warning "TODO"
    This page needs to be revised

WireFeeds will be preserved if the property `preserveWireFeed` is enabled on the
`SyndFeedInput` object:

```java
SyndFeedInput in = new SyndFeedInput();
in.setPreserveWireFeed(true);
SyndFeed syndFeed = in.build(..);
WireFeed wireFeed = syndFeed.originalWireFeed();
```

Atom entries and RSS items are also available from SyndEntry objects if the
WireFeed is preserved:

```java
Object obj = syndEntry.getWireEntry();
if (obj != null && obj instanceof Entry) {
    // Atom specific code
    Entry entry = (Entry) obj;
    System.out.println(entry.getRights());
} else if (obj != null && obj instanceof Item) {
    // RSS specific code
    Item item = (Item) obj;
    System.out.println(item.getComments());
}
```
