# ROME Modules

!!! warning "TODO"
    This page needs to be revised

The ROME Modules project is an effort to roll up contributed module support into
a single distribution for users.

## Current modules

| Module | Description |
| --- | --- |
| [A9 OpenSearch](a9-opensearch.md) | For working with Amazon/OpenSearch.org results. |
| [Content](content.md) | For using content:encoded tags. |
| [Creative Commons](creative-commons.md) | A unified module for working with the RDF and RSS/Atom Creative Commons License information. |
| [GeoRSS](georss.md) | For adding location information to RSS/Atom. |
| [Google Base](google-base.md) | For working with the Google Base types and custom tagsets. |
| [iPhoto Photocasting](iphoto-photocasting.md) | For working with Apple iPhoto Photocasts |
| [iTunes Podcasting](itunes-podcasting.md) | Apples extensions for listing in the iTunes podcast directory. |
| [Microsoft Simple List Extensions](microsoft-simple-list-extensions.md) | for sorting and grouping entries. |
| [Microsoft Simple Sharing Extensions](microsoft-simple-sharing-extensions.md) | for synchronizing across bi-directional RSS and [OPML](index.md) feeds. |
| [Slash](slash.md) | Used by Slash-based blogs. |
| [Yahoo! MediaRSS](yahoo-mediarss.md) | For working with Yahoo! MediaRSS feeds (and Flickr Photostreams) |
| [Yahoo! Weather](yahoo-weather.md) | For use with the Yahoo Weather API |

## General Guidelines

This is intended to serve as a guide for contributions as well as a hint for
users working with the modules.

- Modules are packaged in com.rometools.rome.feed.module.\[Module Name\].
- Modules contain a com.rometools.rome.feed.module.\[Module Name\].Module.URI
  reference for retrieval from ROME Synd* classes.
- Modules contain a com.rometools.rome.feed.module.\[Module Name\].ModuleImpl
  that is a concrete implementation.
- Modules contain a com.rometools.rome.feed.module.\[Module Name\].types package
  that holds any other datatypes needed for the module. Many of these are simple
  immutable types to simplify clone() and copyFrom()
- Modules contain a com.rometools.rome.feed.module.\[Module Name\].io package
  with parsers and generators.
