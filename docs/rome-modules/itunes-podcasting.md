# iTunes Podcasting

!!! warning "TODO"
    This page needs to be revised

This plugin is for use with iTunes Music Service podcast listings.

### Sample Usage

```java
SyndFeedInput input = new SyndFeedInput();
SyndFeed syndfeed = input.build(new XmlReader(feed.toURL()));

Module module = syndfeed.getModule("http://www.itunes.com/dtds/podcast-1.0.dtd");
FeedInformation feedInfo = (FeedInformation) module;

System.out.println( feedInfo.getImage() );
System.out.println( feedInfo.getCategory() );

// Or to create a feed..

ArrayList modules = new ArrayList();
EntryInformation e = new EntryInformationImpl();
e.setDuration( new Duration( 10000 ) );
modules.add( e );
syndEntry.setModules( modules );
```
