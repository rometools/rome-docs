# Bean utilities

ROME contains some utility classes that help us implementing the *equals*,
*hashCode*, *toString* and *clone* methods by using reflection. Although the
classes are public they are not meant to be used outside of ROME and are subject
to change (breaking backward compatibility).

## EqualsBean

The *EqualsBean* class provides a reflection-based implementation of the
*equals* and *hashCode* methods.

```java
public class MyBean {

    // [...]

    public boolean equals(Object obj) {
        return EqualsBean.beanEquals(MyBean.class, this, obj);
    }

    public int hashCode() {
        return EqualsBean.beanHashCode(this);
    }

}
```

## ToStringBean

The *ToStringBean* class provides a reflection-based implementation of the
*toString* method.

```java
public class MyBean implements ToString {

    // [...]

    public String toString() {
        return ToStringBean.toString(MyBean.class, this);
    }

}
```

## CloneableBean

The *CloneableBean* class provides a reflection-based implementation of the
*clone* method.

```java
public class MyBean implements Cloneable {

    // [...]

    public Object clone() {
        return CloneableBean().beanClone(this, ignore);
    }

}
```

It copies all properties whose names are not on the ignore list. This is useful
for cases where the Bean has convenience properties (properties that are
actually references to other properties or properties of properties). For
example the *SyndFeed* and *SyndEntry* beans have such convenience properties.
