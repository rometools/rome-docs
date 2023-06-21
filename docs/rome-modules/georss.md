# GeoRSS

This module adds support for [GeoRSS](https://en.wikipedia.org/wiki/GeoRSS).

## Sample

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:georss="http://www.georss.org/georss" version="2.0">
  <channel>
    <title>Title</title>
    <description>Description</description>
    <link>https://example.org/feed</link>
    <item>
      <title>Title</title>
      <description>Description</description>
      <link>https://example.org/entry</link>
      <guid>https://example.org/entry</guid>
      <georss:point>47.0 9.0</georss:point>
    </item>
  </channel>
</rss>
```

## Create feed

```java
// create entry
SyndContent description = new SyndContentImpl();
description.setValue("Description");

GeoRSSModule geo = new SimpleModuleImpl();
geo.setPosition(new Position(47.0, 9.0));

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Title");
entry.setDescription(description);
entry.setLink("https://example.org/entry");
entry.getModules().add(geo);

// assemble feed
SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("Title");
feed.setDescription("Description");
feed.setLink("https://example.org/feed");
feed.getEntries().add(entry);

// output feed
String xml = new SyndFeedOutput().outputString(feed);
System.out.println(xml);
```

## Read feed

```
Reader reader = new StringReader(xml);
SyndFeed feed = new SyndFeedInput().build(reader);

for (SyndEntry entry : feed.getEntries()) {

    GeoRSSModule module = GeoRSSUtils.getGeoRSS(entry);
    Position position = module.getPosition();
    double latitude = position.getLatitude();
    double longitude = position.getLongitude();
    
    System.out.println(latitude);
    System.out.println(longitude);

}
```
