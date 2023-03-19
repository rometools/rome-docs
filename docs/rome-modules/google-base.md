# Google Base

[Google Base](https://support.google.com/merchants/answer/160589) is an
extension for RSS and Atom feeds that allows to transport many additional
content-related informations.

## Sample

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:g-core="http://base.google.com/ns/1.0" version="2.0">
  <channel>
    <title>Title</title>
    <link>https://example.org/feed</link>
    <description>Description</description>
    <item>
      <title>Title</title>
      <link>https://example.org/entry</link>
      <description>Description</description>
      <guid>https://example.org/entry</guid>
      <g-core:year>2000</g-core:year>
      <g-core:model>Insight</g-core:model>
      <g-core:make>Honda</g-core:make>
    </item>
  </channel>
</rss>
```

## Create feed

```java
// create entry
SyndContent description = new SyndContentImpl();
description.setValue("Description");

GoogleBaseImpl vehicle = new GoogleBaseImpl();
vehicle.setMake("Honda");
vehicle.setModel("Insight");
vehicle.setYear(new YearType("2000"));

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Title");
entry.setDescription(description);
entry.setLink("https://example.org/entry");
entry.getModules().add(vehicle);

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

```java
Reader reader = new StringReader(xml);
SyndFeed feed = new SyndFeedInput().build(reader);

for (SyndEntry entry : feed.getEntries()) {
    
    GoogleBase googleBase = (GoogleBase) entry.getModule(GoogleBaseImpl.URI);
    String make = googleBase.getMake();
    String model = googleBase.getModel();
    YearType year = googleBase.getYear();
    
    System.out.println(make);
    System.out.println(model);
    System.out.println(year);

}
```
