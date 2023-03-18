# Preserve WireFeed

`WireFeed` is the base class for all RSS and Atom specific feed classes. To
preserve it while parsing, set the property `preserveWireFeed` of
`SyndFeedInput` to `true`.

```java
File file = new File("feed.xml");

SyndFeedInput input = new SyndFeedInput();
input.setPreserveWireFeed(true);
SyndFeed syndFeed = input.build(file);

WireFeed wireFeed = syndFeed.originalWireFeed();
```

!!! Note 
    `SyndFeedInput` supports additional input types. Please take a look at the 
    Javadoc.

Atom entries and RSS items are also available from `SyndEntry` objects if the
WireFeed is preserved:

```java
for (SyndEntry syndEntry : syndFeed.getEntries()) {

    Object wireEntry = syndEntry.getWireEntry();

    if (wireEntry != null) {

        if (wireEntry instanceof Entry) {

            Entry entry = (Entry) wireEntry;
            // Atom specific code

        } else if (wireEntry instanceof Item) {

            Item item = (Item) wireEntry;
            // RSS specific code

        }

    }

}
```
