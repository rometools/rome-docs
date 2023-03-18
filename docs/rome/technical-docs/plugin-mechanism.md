# Plugin mechanism

ROME has been designed around a plugin mechanism. The support for every feed
type is implemented as a plugin thats included in the distribution.

Parsing feeds, generating feeds, converting feeds from a concrete feed to a
SyndFeed and vice versa, parsing modules and generating modules is done using
plugins.

Plugins for new functionality can be added and default plugins can be easily
replaced with alternate plugins.

## Plugins definition files

The default plugin definition file (`com/rometools/rome/rome.properties`) is
included in the ROME JAR file. This is the first definition file to be processed
and defines the default parsers, generators and converters for feeds and
modules.

After loading the default file, ROME looks for additional `rome.properties` at
the root of all classpath entries and appends the plugin definitions to the
existing ones. Note that if there are several `rome.properties` files in the
different classpath entries all of them are processed. The order of processing
depends on how the `ClassLoader` processes the classpath entries, this is
normally done in the order of appearance - of the entry - in the classpath.

For each type of plugin (parser, generator, converter, etc) a list of available
plugins is built following the read order just described. The plugins classes
are then loaded and instantiated. All plugins have some kind of primary key. In
the case of parsers, generators and converters the primary key is the type of
feed they handle. In the case of modules, the primary key is the module URI. If
a plugin list definition (the aggregation of all plugins of the same time from
all `rome.properties`) contains more than one plugin with the same primary key,
the latter one is the one that will be used (this enables replacing default
plugins with custom ones).

The plugins are read, loaded and managed by the implementation class
`com.rometools.rome.io.impl.PluginManager`. This class is an abstract class and
it is extended to provide support for each type of plugin.

## Parser Plugins

Parser plugins are managed by the `com.rometools.rome.io.impl.FeedParsers` class
(subclass of the `PluginManager`). This plugin manager looks for the
`WireFeedParser.classes` property in all `rome.properties` files. The fully
qualified names of the parser classes must be separated by whitespaces or
commas. For example, the default `rome.properties` file parser plugins
definition is as follows:

```properties
# Feed Parser implementation classes
WireFeedParser.classes=com.rometools.rome.io.impl.RSS090Parser \
                       com.rometools.rome.io.impl.RSS091NetscapeParser \
                       com.rometools.rome.io.impl.RSS091UserlandParser \
                       com.rometools.rome.io.impl.RSS092Parser \
                       com.rometools.rome.io.impl.RSS093Parser \
                       com.rometools.rome.io.impl.RSS094Parser \
                       com.rometools.rome.io.impl.RSS10Parser \
                       com.rometools.rome.io.impl.RSS20wNSParser \
                       com.rometools.rome.io.impl.RSS20Parser \
                       com.rometools.rome.io.impl.Atom03Parser
```

All the classes defined in this property have to implement the
`com.rometools.rome.io.WireFeedParser` interface. Parser instances must be
thread safe. The return value of the `getType()` method is used as the primary
key. If more than one parser returns the same type, the latter one prevails.

## Generator Plugins

Generator plugins are managed by the `com.rometools.rome.io.impl.FeedGenerators`
class (subclass of the `PluginManager`). This plugin manager looks for the
`WireFeedGenerator.classes` property in all `rome.properties` files. The fully
qualified names of the generator classes must be separated by whitespaces or
commas. For example, the default `rome.properties` file generator plugins
definition is as follows:

```properties
# Feed Generator implementation classes
WireFeedGenerator.classes=com.rometools.rome.io.impl.RSS090Generator \
                          com.rometools.rome.io.impl.RSS091NetscapeGenerator \
                          com.rometools.rome.io.impl.RSS091UserlandGenerator \
                          com.rometools.rome.io.impl.RSS092Generator \
                          com.rometools.rome.io.impl.RSS093Generator \
                          com.rometools.rome.io.impl.RSS094Generator \
                          com.rometools.rome.io.impl.RSS10Generator \
                          com.rometools.rome.io.impl.RSS20Generator \
                          com.rometools.rome.io.impl.Atom03Generator
```

All the classes defined in this property have to implement the
`com.rometools.rome.io.WireFeedGenerator` interface. Generator instances must be
thread safe. The return value of the `getType()` method is used as the primary
key. If more than one generator returns the same type, the latter one prevails.

## Converter Plugins

Converter plugins are managed by the `com.rometools.rome.synd.impl.Converters`
class (subclass of the `PluginManager`). This plugin manager looks for the
`Converter.classes` property in all `rome.properties` files. The fully qualified
names of the converter classes must be separated by whitespaces or commas. For
example, the default `rome.properties` file converter plugins definition is as
follows:

```properties
# Feed Conversor implementation classes
Converter.classes=com.rometools.rome.feed.synd.impl.ConverterForAtom03 \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS090 \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS091Netscape \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS091Userland \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS092 \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS093 \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS094 \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS10 \
                  com.rometools.rome.feed.synd.impl.ConverterForRSS20
```

All the classes defined in this property have to implement the
`com.rometools.rome.synd.Converter` interface. Converter instances must be
thread safe. The return value of the `getType()` method is used as the primary
key. If more than one converter returns the same type, the latter one prevails.

## Module Plugins

There are 2 types of module plugins, module parser plugins and module generator
plugins. They use a same pattern feed parsers and generators use.

The main difference is that support for module plugins has to be wired in the
feed parser and generator plugins. The default feed parser and generator plugins
supporting module plugins are: RSS 1.0, RSS 2.0 and Atom 0.3.

It is important to understand that this wiring is for modules support. Once a
feed parser or generator has modules support, new modules can be used just by
adding them to right property in the `rome.properties` file. No code changes are
required.

Module parsers and generators are defined at feed and item level. This allow
selective handling of modules, for example handling Syndication module at feed
level only.

Module parser plugins are managed by the
`com.rometools.rome.io.impl.ModuleParsers` class (subclass of the
`PluginManager`). This plugin manager looks for the `.feed.ModuleParser.classes`
and the `.item.ModuleParser.classes` properties in all `rome.properties` files
must be the type defined by the parser (ie: rss_1.0, atom_0.3). The fully
qualified names of the module parser classes must be separated by whitespaces or
commas. For example, the default `rome.properties` file modules parser plugins
definition is as follows:

```properties
# Parsers for Atom 0.3 feed modules
atom_0.3.feed.ModuleParser.classes=com.rometools.rome.io.impl.SyModuleParser \
                                   com.rometools.rome.io.impl.DCModuleParser

# Parsers for Atom 0.3 entry modules
atom_0.3.item.ModuleParser.classes=com.rometools.rome.io.impl.DCModuleParser

# Parsers for RSS 1.0 feed modules
rss_1.0.feed.ModuleParser.classes=com.rometools.rome.io.impl.SyModuleParser \
                                  com.rometools.rome.io.impl.DCModuleParser

# Parsers for RSS 1.0 item modules
rss_1.0.item.ModuleParser.classes=com.rometools.rome.io.impl.DCModuleParser

# Parsers for RSS 2.0 feed modules
rss_2.0.feed.ModuleParser.classes=

# Parsers for RSS 2.0 item modules
rss_2.0.item.ModuleParser.classes=
```

All the classes defined in this property have to implement the
`com.rometools.rome.io.ModuleParser` interface. ModuleParser instances must be
thread safe. The return value of the `getNamesapceUri()` method is used as the
primary key. If more than one module parser returns the same URI, the latter one
prevails.

Module generator plugins are managed by the
`com.rometools.rome.io.impl.GeneratorParsers` class (subclass of the
`PluginManager`). This plugin manager looks for the
`.feed.ModuleGenerator.classes` and the `.item.ModuleGenerator.classes`
properties in all `rome.properties` files. must be the type defined by the
generator (ie: rss_1.0, atom_0.3). The fully qualified names of the module
generator classes must be separated by whitespaces or commas. For example, the
default `rome.properties` file modules generator plugins definition is as
follows:

```properties
# Generators for Atom 0.3 feed modules
atom_0.3.feed.ModuleGenerator.classes=com.rometools.rome.io.impl.SyModuleGenerator \
                                      com.rometools.rome.io.impl.DCModuleGenerator

# Generators for Atom 0.3 entry modules
atom_0.3.item.ModuleGenerator.classes=com.rometools.rome.io.impl.DCModuleGenerator

# Generators for RSS 1.0 feed modules
rss_1.0.feed.ModuleGenerator.classes=com.rometools.rome.io.impl.SyModuleGenerator \
                                     com.rometools.rome.io.impl.DCModuleGenerator

# Generators for RSS_1.0 entry modules
rss_1.0.item.ModuleGenerator.classes=com.rometools.rome.io.impl.DCModuleGenerator

# Generators for RSS 2.0 feed modules
rss_2.0.feed.ModuleGenerator.classes=

# Generators for RSS_2.0 entry modules
rss_2.0.item.ModuleGenerator.classes=
```

All classes defined in this property have to implement the
`com.rometools.rome.io.ModuleGenerator` interface. ModuleGenerator instances
must be thread safe. The return value of the `getNamespaceUri()` method is used
as the primary key. If more than one module generator returns the same URI, the
latter one prevails.
