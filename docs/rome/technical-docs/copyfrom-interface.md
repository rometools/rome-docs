# CopyFrom interface

The `CopyFrom` interface defines functionality similar to the `clone()` method
with the difference that the object itself is the target of the copy operation
and has to be created first. Using `CopyFrom` enables copying data between
different implementations of a common interface without these implementations
having to know about each other.

## Example
```java
public interface Foo extends CopyFrom {

	String getName();

	void setName(String name);

	Set<String> getValues();

	void setValues(Set<String> values);

}
```

```java
public class FooImplA implements Foo {

	private String name;
	private Set<String> values = new HashSet<String>();

	public String getName() {
		return this.name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Set<String> getValues() {
		return this.values;
	}

	public void setValues(Set<String> values) {
		this.values = values;
	}

	public Class<? extends CopyFrom> getInterface() {
		return Foo.class;
	}

	public void copyFrom(CopyFrom obj) {
		Foo other = (Foo) obj;
		this.setName(other.getName());
		this.setValues(new HashSet<String>(other.getValues()));
	}

}
```

```java
public class FooImplB implements Foo {

    private Map<String, Object> data = new HashMap<String, Object>();

    public String getName() {
        return (String) this.data.get("name");
    }

    public void setName(String name) {
        this.data.put("name", name);
    }

	public Set<String> getValues() {
        return (Set<String>) this.data.get("values");
    }

    public void setValues(Set<String> values) {
        this.data.put("values", values);
    }

    public Class<? extends CopyFrom> getInterface() {
        return Foo.class;
    }

    public void copyFrom(CopyFrom obj) {
        Foo other = (Foo) obj;
        this.setName(other.getName());
        this.setValues(new HashSet<String>(other.getValues()));
    }

}
```

For properties implementing the `CopyFrom` interface, the bean must create a
property instance implementing the interface returned by the `getInterface()`
method. This allows the bean doing the `copyFrom(CopyFrom)` to handle
specialized subclasses of property elements properly. This is also applicable to
array and collection properties. The `modules` property of the `SyndFeed` and
`SyndEntry` beans is a use case of this feature where the `copyFrom(CopyFrom)`
invocation must create different bean subclasses for each type of module, the
`getInterface()` helps to find the right implementation.
