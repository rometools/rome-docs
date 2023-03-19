# OpenSearch

This module adds support for
[OpenSearch](https://en.wikipedia.org/wiki/OpenSearch) developed by Amazon.com
subsidiary A9.

## Sample Usage

```java
OSQuery query = new OSQuery();
query.setRole("superset");
query.setSearchTerms("Java Syndication");
query.setStartPage(1);

Link link = new Link();
link.setType("application/opensearchdescription+xml");
link.setHref("https://www.example.org/opensearch-description.xml");

OpenSearchModule osm = new OpenSearchModuleImpl();
osm.setStartIndex(1);
osm.setTotalResults(1024);
osm.setItemsPerPage(50);
osm.addQuery(query);
osm.setLink(link);

// assemble feed
SyndFeed feed = new SyndFeedImpl();
feed.setFeedType("rss_2.0");
feed.setTitle("OpenSearch");
feed.setDescription("OpenSearch example");
feed.setLink("https://www.example.org/feed");
feed.getModules().add(osm);
```
