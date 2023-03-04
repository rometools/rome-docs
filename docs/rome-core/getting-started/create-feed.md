# Create feed

ROME represents feeds as instances of the _com.rometools.rome.synd.SyndFeed_
interface. The SyndFeed interface and its properties follow the JavaBean
specification. This example shows how to create an RSS 2.0 feed with two
entries.

## Create feed

First a SyndFeed instance has to be created:

```java
SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("Title");
feed.setLink("https://www.example.com");
feed.setDescription("Description");
```

Supported feed types are:

- atom_0.3
- atom_1.0
- rss_0.9
- rss_0.91U
- rss_0.91N
- rss_0.92
- rss_0.93
- rss_0.94
- rss_1.0
- rss_2.0

## Create entries

Then two entries will be created and added to the feed. The first entry uses
plain text while the second entry uses HTML:

```java
List<SyndEntry> entries = new ArrayList<SyndEntry>();

# create first entry
SyndEntry textEntry = new SyndEntryImpl();
textEntry.setTitle("Text entry");
textEntry.setLink("https://www.example.com/first");
textEntry.setPublishedDate(new Date());
textEntry.setDescription("Plain Text");

entries.add(textEntry);

# create second entry
SyndContent htmlContent = new SyndContentImpl();
htmlContent.setType("text/html");
htmlContent.setValue("<p>Second entry</p>");

SyndEntry htmlEntry = new SyndEntryImpl();
htmlEntry.setTitle("HTML entry");
htmlEntry.setLink("https://www.example.com/second");
htmlEntry.setPublishedDate(new Date());
htmlEntry.setDescription(description2);

entries.add(htmlEntry);

# add entries to the feed
feed.setEntries(entries);
```

## Write feed

The feed is now ready to be written out to an XML document.

ROME includes generators that allow producing syndication feed XML documents
from SyndFeed instances. The SyndFeedOutput class handles the generation of the
syndication feed XML documents on any of the supported feed formats (RSS and
Atom). The developer does not need to worry about selecting the right generator
for a syndication feed, the SyndFeedOutput will take care of it by looking at
the information in the SyndFeed bean. All it takes to write the XML document are
the following lines of code:

```java
SyndFeedOutput output = new SyndFeedOutput();
String xml = output.outputString(feed); 
```
!!! note 
    `SyndFeedOutput` supports additional output formats. Please have a look at 
    the Javadoc.