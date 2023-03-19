# Creative Commons

The
[Creative Commons Module](http://backend.userland.com/creativeCommonsRssModule)
provides a unified rights and license system for Atom and RSS feeds.

## Sample
```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:creativeCommons="http://backend.userland.com/creativeCommonsRssModule" version="2.0">
  <channel>
    <title>Title</title>
    <description>Description</description>
    <link>https://example.org/feed</link>
    <creativeCommons:license>http://creativecommons.org/licenses/nc/1.0/</creativeCommons:license>
    <item>
      <title>Title</title>
      <description>Description</description>
      <link>https://example.org/entry</link>
      <guid>https://example.org/entry</guid>
      <creativeCommons:license>http://creativecommons.org/licenses/by/2.5/</creativeCommons:license>
    </item>
  </channel>
</rss>
```

## Create feed

```java
// create entry
SyndContent description = new SyndContentImpl();
description.setValue("Description");

CreativeCommons entryLicense = new CreativeCommonsImpl();
entryLicense.setLicenses(new License[] { License.ATTRIBUTION });

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Title");
entry.setDescription(description);
entry.setLink("https://example.org/entry");
entry.getModules().add(entryLicense);

// assemble feed
CreativeCommons feedLicense = new CreativeCommonsImpl();
feedLicense.setLicenses(new License[] { License.NONCOMMERCIAL });

SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("Title");
feed.setDescription("Description");
feed.setLink("https://example.org/feed");
feed.getEntries().add(entry);
feed.getModules().add(feedLicense);

// write feed
String xml = new SyndFeedOutput().outputString(feed);
```

## Read feed

```
Reader reader = new StringReader(xml);
SyndFeed feed = new SyndFeedInput().build(reader);

// extract license from feed
CreativeCommons feedModule = (CreativeCommons) feed.getModule(CreativeCommons.URI);
License[] feedLicenses = feedModule.getLicenses();

// extract license from entry
for (SyndEntry entry : feed.getEntries()) {

    CreativeCommons entryModule = (CreativeCommons) entry.getModule(CreativeCommons.URI);
    License[] entryLicenses = entryModule.getLicenses();

}
```
