# A9 OpenSearch

!!! warning "TODO"
    This page needs to be revised

This plugin is for use with [OpenSearch.org](http://www.opensearch.org/)
results.

## Sample Usage

```java
SyndFeed feed = new SyndFeedImpl();
feed.setFeedType(feedType);

// Add the opensearch module, you would get information like totalResults from the
// return results of your search
List mods = feed.getModules();
OpenSearchModule osm = new OpenSearchModuleImpl();
osm.setItemsPerPage(1);
osm.setStartIndex(1);
osm.setTotalResults(1024);
osm.setItemsPerPage(50);

OSQuery query = new OSQuery();
query.setRole("superset");
query.setSearchTerms("Java Syndication");
query.setStartPage(1);
osm.addQuery(query);

Link link = new Link();
link.setHref("http://www.bargainstriker.com/opensearch-description.xml");
link.setType("application/opensearchdescription+xml");
osm.setLink(link);

mods.add(osm);

feed.setModules(mods);
// end add module
```
