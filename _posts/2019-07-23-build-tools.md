---
layout: post
author: jayahariv
title: Build tools
---
# Java
dependency management and a build automation tools for Java.

## Apache Ant
In the beginning, Make was the only build automation tool, beyond homegrown solutions. Make has been around since 1976 and as such, it was used for building Java applications in the early Java years.

However, a lot of conventions from C programs didn’t fit in the Java ecosystem, so in time Ant was released as a better alternative.

Apache Ant is a Java library and command-line tool whose mission is to drive processes described in build files as targets and extension points dependent upon each other.

Ant supplies a number of built-in tasks allowing to compile, assemble, test and run Java applications. Ant can also be used effectively to build non Java applications, for instance C or C++ applications.

Ant build files are written in XML, and by convention, they’re called build.xml.

Different phases of a build process are called “targets”.

- Another Neat Tools

## Apache Maven
Maven continues to use XML files just like Ant but in a much more manageable way.

While Ant gives the flexibility and requires everything to be written from scratch, Maven relies on conventions and provides predefined commands (goals).

Maven’s configuration file, containing build and dependency management instructions, is by convention called pom.xml.

Additionally, Maven also prescribes strict project structure, while Ant provides flexibility there as well.

Maven can be considered a plugin execution framework, since all work is done by plugins.

## Gradle
was built upon the concepts of Ant and Maven.

This was adopted by Gradle, which is using a DSL based on Groovy. This led to smaller configuration files with less clutter since the language was specifically designed to solve specific domain problems.

Gradle’s configuration file is by convention called build.gradle.

Plugins add all useful features.

Gradle gave its build steps name “tasks”, as opposed to Ant’s “targets” or Maven’s “phases”.
