# Installation

## Install from Maven Central

All versions since 1.5.0 are available on Maven Central under the following
coordinates:

```xml
<dependency>
    <groupId>com.rometools</groupId>
    <artifactId>rome</artifactId>
    <version>${rome.version}</version>
</dependency>
```

## Install from source

To build the library from source you just have to install JDK 6 (or later),
clone the source from [GitHub](https://github.com/rometools/rome) and install
all modules using the provided
[Maven Wrapper](https://maven.apache.org/wrapper/).

### Linux

```shell
git clone https://github.com/rometools/rome.git
cd rome
./mvnw install
```

### Windows

```shell
git clone https://github.com/rometools/rome.git
cd rome
mvnw install
```
