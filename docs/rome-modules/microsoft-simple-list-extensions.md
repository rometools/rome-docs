# Microsoft Simple List Extensions

!!! warning "TODO"
    This page needs to be revised

This plugin is for use with feeds ith Microsoft Simple List Extensions.

## Sample Usage

```java
SimpleListExtension sle = (SimpleListExtension) feed.getModule( SimpleListExtension.URI );
System.out.println( sle );
Group[] groups = sle.getGroupFields();
System.out.println( groups[0].getLabel() );

//You can use the SleUtility class to do sorting and grouping:

List sortedEntries = SleUtility.sort( feed.getEntries(),  sle.getSortFields()[1], true );
SyndEntry entry = (SyndEntry) sortedEntries.get( 0 );

//You can also Group or Sort and Group

List sortedAndGroupedEntries = SleUtility.sortAndGroup( feed.getEntries, sle.getGroupFields(), sle.getSortFields()[0], false );

// If you change, for instance, module values on a feed and want to reinitialize it for
// grouping and sorting...

SleUtility.initializeForSorting( feed );

// Be aware, this is a VERY heavy operation and should not be used frequently.
```
