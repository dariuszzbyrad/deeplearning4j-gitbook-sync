---
title: 'Configuration for Gradle, SBT, and More'
short_title: 'SBT, Gradle, & Others'
description: Configure the build tools for Deeplearning4j.
category: Configuration
weight: 3
---

# SBT, Gradle, & Others

## Configuring your build tool

While we encourage Deeplearning4j, ND4J and DataVec users to employ Maven, it's worthwhile documenting how to configure build files for other tools, like Ivy, Gradle and SBT -- particularly since Google prefers Gradle over Maven for Android projects.

The instructions below apply to all DL4J and ND4J submodules, such as deeplearning4j-api, deeplearning4j-scaleout, and ND4J backends.

## Gradle

You can use Deeplearning4j with Gradle by adding the following to your build.gradle in the dependencies block:

```text
implementation "org.deeplearning4j:deeplearning4j-core:1.0.0-beta6"
```

Add a backend by adding the following:

```text
implementation "org.nd4j:nd4j-native-platform:1.0.0-beta6"
```

You can also swap the standard CPU implementation for [GPUs](deeplearning4j/deeplearning4j-config-gpu-cpu).

## SBT

You can use Deeplearning4j with SBT by adding the following to your build.sbt:

```text
libraryDependencies += "org.deeplearning4j" % "deeplearning4j-core" % "1.0.0-beta6"
```

Add a backend by adding the following:

```text
libraryDependencies += "org.nd4j" % "nd4j-native-platform" % "1.0.0-beta6"
```

You can also swap the standard CPU implementation for [GPUs](deeplearning4j/deeplearning4j-config-gpu-cpu).

## Ivy

You can use Deeplearning4j with ivy by adding the following to your ivy.xml:

```markup
<dependency org="org.deeplearning4j" name="deeplearning4j-core" rev="1.0.0-beta6" conf="build" />
```

Add a backend by adding the following:

```markup
<dependency org="org.nd4j" name="nd4j-native-platform" rev="1.0.0-beta6" conf="build" />
```

You can also swap the standard CPU implementation for [GPUs](deeplearning4j/deeplearning4j-config-gpu-cpu).

## Leinengen

Clojure programmers may want to use [Leiningen](https://github.com/technomancy/leiningen/) or [Boot](http://boot-clj.com/) to work with Maven. A [Leiningen tutorial is here](https://github.com/technomancy/leiningen/blob/master/doc/TUTORIAL.md).

NOTE: You'll still need to download ND4J, DataVec and Deeplearning4j, or doubleclick on the their respective JAR files file downloaded by Maven / Ivy / Gradle, to install them in your Eclipse installation.

