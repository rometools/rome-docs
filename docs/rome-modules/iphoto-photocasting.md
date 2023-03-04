# iPhoto Photocasting

!!! warning "TODO"
    This page needs to be revised

This plugin is for use with iPhoto Photocast listings.

This module will read and write photocast feeds "properly". Be advised, however,
that an iPhoto photocast feed will not pass a FeedValidator test as they are not
properly namespaced. If you are wanting to publish, rather than read, consider
using the [MediaRSS (modules)](yahoo-mediarss.md) plugin instead. iPhoto will
also read [MediaRSS (modules)](yahoo-mediarss.md) (Flickr Photostream) feeds as
well.

## Sample Usage

```java
SyndFeed feed = input.build(  new File( "/foo.rss" ) ) );
List entries = feed.getEntries();
for( int i =0; i < entries.size() ; i++ ){
    System.out.println( ((SyndEntry)entries.get(i)).getModule( PhotocastModule.URI ) );
}

// or to create a photocast module:

SyndFeed myFeed = new SyndFeedImpl();
myFeed.getModules().add( new PhotocastModuleImpl() );
// you need this as a placeholder so the version gets in the feed.

SyndEntry myEntry = new SyndEntryImpl();
PhotocastModule pm = new PhotocastModuleImpl();
pm.setUrl( new URL("http://foo.com/img.jpg" ) );
pm.setThumnail( new URL("http://foo.com/img-small.jpg" ) );
pm.setCropDate( new Date() );
pm.setPhotoDate( new Date() );
pm.setMetaData( new PhotoDate(), "Some comments that I think always get ignored." );
myEntry.getModules().add( pm );
```
