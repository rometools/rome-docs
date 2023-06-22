# Atom Threading Extensions

This plugin is for use with Atom 1.0 elements inside RSS feeds

If you have an RSS feed that uses Atom extensions like link, author or
contributor, with Rome you can parse these elements

```xml
<rss xmlns:atom="http://www.w3.org/2005/Atom" version="2.0">
    <channel>
        <title>Lorem Ipsum</title>
        <link>http://example.org</link>
        <description>Lorem Ipsum</description>
        <atom:link href="http://test.com" type="application/rss+xml" rel="self" />
        <atom:link href="http://test.com/alt" type="application/rss+xml" rel="alternate" />
        <atom:author>
            <atom:name>Lorem Ipsum</atom:name>
            <atom:email>test@example.org</atom:email>
            <atom:uri>http://example.org</atom:uri>
        </atom:author>
        <atom:contributor>
            <atom:name>Lorem Ipsum</atom:name>
            <atom:email>test@example.org</atom:email>
            <atom:uri>http://example.org</atom:uri>
        </atom:contributor>
        <item>
            <title>Lorem Ipsum</title>
            <link>http://example.org/episode1</link>
            <description><![CDATA[Lorem Ipsum]]></description>
            <atom:author>
                <atom:name>Lorem Ipsum</atom:name>
                <atom:email>test@example.org</atom:email>
                <atom:uri>http://example.org</atom:uri>
            </atom:author>
            <atom:contributor>
                <atom:name>Lorem Ipsum</atom:name>
                <atom:email>test@example.org</atom:email>
                <atom:uri>http://example.org</atom:uri>
            </atom:contributor>
        </item>
    </channel>
</rss>
```

## Sample Usage

```java
final SyndFeed feed = new SyndFeedInput().build(
    new XmlReader(new File("/foo_rss_atom.xml"))));
feed.getModule(AtomLinkModule.URI);
```

### Read author's email in each entry

```java
final AtomLinkModule feedAtomModule = (AtomLinkModule) feed.getModule(AtomLinkModule.URI);
for (SyndPerson author : feedAtomModule.getAuthors()) {
    LOG.debug(author.getEmail());
}
```
