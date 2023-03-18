# ROME Fetcher

!!! warning
    ROME Fetcher is deprecated and will be removed with version 2.0.0


The ROME Fetcher allows the retrieval of feeds via HTTP supporting
[HTTP conditional gets](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers)
and gzip compressed feeds. It enables users to write aggregators that follow the
[Atom aggregator behaviour recommendations (Archived)](https://web.archive.org/web/20060712072932/http://diveintomark.org/archives/2003/07/21/atom_aggregator_behavior_http_level).

## Installation

All versions since 1.5.0 are available on Maven Central under the following
coordinates:

```xml
<dependency>
    <groupId>com.rometools</groupId>
    <artifactId>rome-fetcher</artifactId>
    <version>${rome.version}</version>
</dependency>
```

If you want to use the `HttpClientFeedFetcher` you also need to add the
following dependency:

```xml
<dependency>
    <groupId>commons-httpclient</groupId>
    <artifactId>commons-httpclient</artifactId>
    <version>3.1</version>
</dependency>
```

## Example

The basic usage of ROME Fetcher is as follows:

```java
// create cache
FeedFetcherCache cache = HashMapFeedInfoCache.getInstance();

// create fetcher
FeedFetcher fetcher = new HttpURLFeedFetcher(cache);

// fetch feed
SyndFeed feed = fetcher.retrieveFeed(new URL("https://example.org/feed"));

// print feed to stdout
System.out.println(feed);
```

Any subsequent fetches of the feed will now only retrieve the feed if it has
changed.

ROME Fetcher can also be used without a cache if required. Simply create it
using the default constructor:

```java
FeedFetcher feedFetcher = new HttpURLFeedFetcher();
```

### HttpClientFeedFetcher

To use the Jakarta Commons HTTP Client based implementation, simply switch the
FeedFetcher implementation (also usable without cache):

```java
FeedFetcher fetcher = new HttpClientFeedFetcher(cache);
```

### Preserve WireFeed

To preserve the WireFeed you have to set the `preserveWireFeed` property to
`true`:

```java
feedFetcher.setPreserveWireFeed(true);
```
