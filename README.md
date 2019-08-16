### nullaway
---
https://github.com/uber/NullAway

```java
plugins {
  id "net.ltgt.errorprone" version "0.6"
}

dependencies {
  annotaionProcessor "com.uber.nullaway:nullaway:0.7.5"
  
  compileOnly "com.google.code.findugs:jsr305:3.0.2"
  
  errorprone "com.google.errorprone:error_prone_core:2.3.2"
  errorproneJavac "com.google.errorprone:javac:9+181-r4173-1"
}

import net.ltgt.gradle.errorprone.CheckSerity

tasks.withType(JavaCompile) {
  if (!name.toLowerCase().contains("test")) {
    options.errorprone {
      check("NullAway", CheckSeverity.ERROR)
      option("NullAway:AnnotatedPackages", "com.uber")
    }
  }
}

buildscript {
  repositories {
    gradlePluginPortal()
  }
  dependencies {
    classpath "net.ltgt.gradle:gradle-errorprone-plugin:0.6"
  }
}

apply plugin: 'net.ltgt.errorprone'


dependencies {
  annotationProcessor "com.uber.nullaway:nullaway:0.7.5"
}

static void log(Object x) {
  System.out.println(x.toString());
}
static void foo() {
  log(null);
}

static void log(@Nullable Object x) {
  System.out.println(x.toString());
}

warning: [NullAway] dereferenced expression x is @Nullable
  System.out.println(x.toString());
  
static void log(@Nullable Object x) {
  if (x != null) {
    System.out.println(x.toString());
  }
}
```

```sh
cp sample/src/main/java/com/uber/mylib/MyClass.jaa.buggy sample/src/main/java/com/uber/mylib/MyClass.java
./gradlew build
```

```
```
