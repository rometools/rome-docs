# Microsoft Simple Sharing Extensions

!!! warning "TODO"
    This page needs to be revised

!!! warning "TODO"
    Check if the module is used at all

This ROME module supports Microsoft Simple Sharing Extensions, an RSS and
[OPML](../rome-opml/index.md) extension designed to support data synchronization
between bi-directional feeds.

## Sample Usage

```java
SyndFeedInput input = new SyndFeedInput();
SyndFeed syndfeed = input.build(new XmlReader(feed.toURL()));

List entries = syndfeed.getEntries();
Iterator it = entries.iterator();

for (int id = 101; it.hasNext() && id <= 113; id++) {
    SyndEntry entry = (SyndEntry) it.next();
    Sync sync = (Sync) entry.getModule(SSEModule.SSE_SCHEMA_URI);
    assertEquals(String.valueOf(id), sync.getId());

    History history = sync.getHistory();
    assertNotNull(history);

    Date when = history.getWhen();
    assertNotNull(when);
    Date testDate = DateParser.parseRFC822("Fri, 6 Jan 2006 19:24:09 GMT");
    assertEquals(testDate, when);
}
```
