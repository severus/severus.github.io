---
layout: default
title: Packaging programs in JAR files
parent: Java
grand_parent: Hacking
nav_order: 1
---

# Packaging Java programs in JAR files

## Hello, world!

Let's create a simple “Hello, world!” program in Java and package it as a JAR file.

```java
// HelloWorld.java
class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Hello, world!");
  }
}
```

Next we need to compile that code into a Java class file:

```
$ javac HelloWorld.java
```

After running that command a new file should appear in the current directory:

```
$ ls
HelloWorld.class  HelloWorld.java
```

We can run this program with the following command:

```
$ java HelloWorld
Hello, world!
```

Java programs are commonly distributed as JAR files, because there could be more files and assets other than a main class. To pack our program as a JAR file run the following command:

```
$ jar cfe app.jar HelloWorld HelloWorld.class
```

The above command will create a JAR file `app.jar` using `HelloWorld` class as an entry point and add `HelloWorld.class` to the JAR file:

```
$ ls
app.jar  HelloWorld.class  HelloWorld.java
```

Now it's possible to run our packaged program using the following:

```
$ java -jar app.jar
Hello, world!
```

## Adding dependencies

In this section we are going to create a program wich imports another Java packages. Finally, we are going to pack that program alongside its dependencies into a JAR file.

For this example we'll be using [IPinfo API](https://ipinfo.io/) and its [Java library](https://github.com/ipinfo/java). You first need to [signup](https://ipinfo.io/signup) and get a free token to use IPinfo API.

Let's create the environment variable to store the token value (replace `your-token-here` with your actual token):

```
$ export IPINFO_TOKEN="your-token-here"
```

We'll use that environment variable later in our Java program:

```java
import io.ipinfo.api.IPInfo;
import io.ipinfo.api.model.IPResponse;

class HelloWorld {
  public static void main(String... args) {
    if (args.length == 0) {
      System.err.println("No arguments passed!");
      return;
    }

    String token = System.getenv("IPINFO_TOKEN");
    IPInfo ipInfo = IPInfo.builder().setToken(token).build();

    for (String arg: args) {
      try {
        IPResponse response = ipInfo.lookupIP(arg);

        // Print out the hostname
        System.out.println(arg + ": " + response.getHostname());
      } catch (Exception e) {
        System.err.println(e.getMessage());
      }
    }
  }
}
```

Our program does the following:

1. It reads IPinfo API token from the `IPINFO_TOKEN` environment variable
2. It loops through the program arguments
3. It treats every argument as IP address and tries to get the name of the host for that particular IP address.
4. Finally, it prints the host name.

The program we created depends on IPinfo library, which in turn depends on other Java libraries. We are going to download all the dependencies in order to build and pack our program.

Let's create a new directory for all the packages our program depends upon:

```
$ mkdir lib
$ cd lib
```

### Direct dependencies

Our program depends on IPinfo Java library. It's possible to download it from Maven:

1. Go to [IPinfo API page](https://mvnrepository.com/artifact/io.ipinfo/ipinfo-api) at Maven Repository and pick the latest version (1.1 at the time of this document)
2. On the version page download the JAR file from **files** section and save it to the `lib` directory, we created previously. You can also use cURL to download that file:
    ```
    $ curl -O https://repo1.maven.org/maven2/io/ipinfo/ipinfo-api/1.1/ipinfo-api-1.1.jar
    ```

### Dependencies's dependencies

Since IPinfo API package depends on other packages, we need to download them too. Go to the packages' pages from the **Compile Dependencies** section of the [IPinfo API page](https://mvnrepository.com/artifact/io.ipinfo/ipinfo-api) and download the appropriate JARs with their dependencies to `lib` directory:

```
$ curl -O https://repo1.maven.org/maven2/com/google/code/gson/gson/2.8.6/gson-2.8.6.jar
$ curl -O https://repo1.maven.org/maven2/com/squareup/okhttp3/okhttp/4.9.0/okhttp-4.9.0.jar
$ curl -O https://repo1.maven.org/maven2/com/squareup/okio/okio/2.9.0/okio-2.9.0.jar
$ curl -O https://repo1.maven.org/maven2/org/jetbrains/kotlin/kotlin-stdlib/1.4.10/kotlin-stdlib-1.4.10.jar
$ curl -O https://repo1.maven.org/maven2/org/jetbrains/kotlin/kotlin-stdlib-common/1.4.10/kotlin-stdlib-common-1.4.10.jar
```

After that your `lib` directory should look like the following:

```
$ ls
gson-2.8.6.jar  ipinfo-api-1.1.jar  kotlin-stdlib-1.4.10.jar  kotlin-stdlib-common-1.4.10.jar  okhttp-4.9.0.jar  okio-2.9.0.jar
```

We've done with our dependencies. Let's get back to the upper directory (assuming, we're in `lib` directory):

```
$ cd ..
```

### Class path

To compile Java program with dependencies we need to provide the `-classpath` parameter to indicate where to look for the dependencies classes:

```
$ javac -classpath 'lib/*' HelloWorld.java
```

For our packaged application we also need to provide class path. To do so at first we need to create a `manifest.txt` file with `Class-Path` directive:

```
// manifest.txt
Class-Path: lib/kotlin-stdlib-1.4.10.jar lib/ipinfo-api-1.1.jar lib/okio-2.9.0.jar lib/gson-2.8.6.jar lib/kotlin-stdlib-common-1.4.10.jar lib/okhttp-4.9.0.jar
```

We can do the same using this short shell script:

```bash
echo "Class-Path: $(find lib -name '*.jar' -printf '%p ')" > manifest.txt
```

### Packaging as a JAR file

We are ready to package out Java application. To create a JAR file run the following:

```
$ jar cvfem app.jar HelloWorld manifest.txt HelloWorld.class lib
```

The above command will create a JAR file `app.jar` using `HelloWorld` class as an entry point, `manifest.txt` as a JAR manifest, and add `HelloWorld.class` and `lib` directory to the JAR file.

Now we car run our packaged Java program. Please check if you created the `IPINFO_TOKEN` environment variable and run the following:

```
$ java -jar app.jar 8.8.8.8 208.67.222.222 1.1.1.1
```

You should see the following output:

```
8.8.8.8: dns.google
208.67.222.222: resolver1.opendns.com
1.1.1.1: one.one.one.one
```
