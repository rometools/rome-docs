# CopyFrom interface

The *CopyFrom* interface defines functionality similar to the *clone* method
with the difference that the object itself is target of the copy operation and
has to be created first. Using *CopyFrom* enables copying data between different
implementations of a common interface without these implementation having to
know about each other.

## Example
```java
public interface Foo extends CopyFrom {

	void setName(String name);

	String getName();

	void setValues(Set<String> values);

	Set<String> getValues();

}

public class FooImplA implements Foo {

	private String name;
	private Set<String> values;

	public void setName(String name) {
		this.name = name;
	}

	public String getName() {
		return this.name;
	}

	public void setValues(Set<String> values) {
		this.values = values;
	}

	public Set<String> getValues() {
		return this.values;
	}

	public Class<? extends CopyFrom> getInterface() {
		return Foo.class;
	}

	public void copyFrom(CopyFrom obj) {
		Foo other = (Foo) obj;
		setName(other.getName());
		setValues(new HashSet<String>(other.getValues()));
	}

}

public class FooImplB implements Foo {

    private Map<String, Object> data = new HashMap<String, Object>();

    public void setName(String name) {
        this.data.put("name", name);
    }

    public String getName() {
        return (String) this.data.get("name");
    }

    public void setValues(Set<String> values) {
        this.data.put("values", values);
    }

	public Set<String> getValues() {
        return (Set<String>) this.data.get("values");
    }

    public Class<? extends CopyFrom> getInterface() {
        return Foo.class;
    }

    public void copyFrom(CopyFrom obj) {
        Foo other = (Foo) obj;
        setName(other.getName());
        setValues(new HashSet<String>(other.getValues()));
    }

}
```

For properties implementing the CopyFrom interface, the bean must create a
property instance implementing the interface returned by the getInterface()
method. This allows the bean doing the copyFrom() to handle specialized
subclasses of property elements propertly. This is also applicable to array and
collection properties. The 'modules' property of the SyndFeed and SyndEntry
beans is a use case of this feature where the copyFrom() invocation must create
different beans subclasses for each type of module, the getInteface() helps to
find the right implementation.
