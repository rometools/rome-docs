# Podlove Simple Chapters

This is a ROME plug in that implements the [Podlove Simple Chapters](https://podlove.org/simple-chapters/) RSS extension.

## Sample

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:psc="http://podlove.org/simple-chapters" version="2.0">
  <channel>
    <title>Lorem Ipsum</title>
    <link>http://example.org</link>
    <description>Lorem Ipsum</description>
    <item>
      <title>Lorem Ipsum</title>
      <link>http://example.org/episode1</link>
      <description>Lorem Ipsum</description>
      <guid isPermaLink="false">http://example.org/episode1</guid>
      <psc:chapters version="1.2">
        <psc:chapter start="00:00:00.000" title="Lorem Ipsum" href="http://example.org" image="http://example.org/cover" />
      </psc:chapters>
    </item>
  </channel>
</rss>
```

## Create feed

```java
SyndContent description = new SyndContentImpl();
description.setValue("Lorem Ipsum");

PodloveSimpleChapterModule psc = new PodloveSimpleChapterModuleImpl();
SimpleChapter sc = new SimpleChapter();
sc.setStart("00:00:00.000");
sc.setTitle("Lorem Ipsum");
sc.setHref("http://example.org");
sc.setImage("http://example.org/cover");
psc.getChapters().add(sc);

SimpleChapter sc2 = new SimpleChapter();
sc2.setStart("00:00:01.000");
sc2.setTitle("Lorem Ipsum 1");
sc2.setHref("http://example1.org");
sc2.setImage("http://example1.org/cover");
psc.getChapters().add(sc2);

SyndEntry entry = new SyndEntryImpl();
entry.setTitle("Lorem Ipsum");
entry.setDescription(description);
entry.setLink("http://example.org");
entry.getModules().add(psc);

SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("Lorem Ipsum");
feed.setDescription("test-description");
feed.setLink("https://example.org");
feed.getEntries().add(entry);

System.out.println(new SyndFeedOutput().outputString(feed));
```

## Read feed

```java
SyndFeedInput input = new SyndFeedInput();
SyndFeed syndfeed = input.build(new XmlReader(feed.toURL()));

for (SyndEntry syndEntry : syndfeed.getEntries()) {
    Module module = syndEntry.getModule(PodloveSimpleChapterModule.URI);
    PodloveSimpleChapterModule chapterModule = ((PodloveSimpleChapterModule) module);

    List<SimpleChapter> chapters = chapterModule.getChapters();
    for (SimpleChapter c : chapters) {
        System.out.println( c.toString );
    }
}
```
