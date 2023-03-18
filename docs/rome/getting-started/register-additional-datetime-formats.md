# Register additional datetime formats

Date and time elements seem to be error prone. RSS feeds use RFC822 datetime
formats and Atom feeds use W3C datetime formats. Both RFC822 and W3C support
quite a few flavors for specifying date and time. Some feeds use the wrong
standard (such as some RSS feeds using W3C dates). Other feeds have an ENTER
before or after the date. And other feeds use arbitrary formats altogether.

To help cope with this ROME makes a few tricks:

- it trims datetime elements before parsing them
- it tries both RFC822 and W3C formats in all feeds
- it uses custom datetime formats (if provided) when the previous steps fail

Additional datetime formats can be specified as `datetime.extra.masks` in a
`rome.properties` file that has to be placed in the root of the classpath. To
register more than one mask, the masks have to be seperated with '|' characters.
The syntax for the masks is the one used by the `java.text.SimpleDateFormat`
class assuming the US locale by default. If datetime masks are specified in more
than one `rome.properties` file in the classpath, they will be aggregated.

## Example
```properties
datetime.extra.masks=yyyy-MM-dd'T'HH:mm:ss.SSSz|yyyy-MM-dd't'HH:mmz
```