## Loraine Lab release for IGB 

This Loraine Lab branch builds artifact htsdjk-igb for re-use as an OSGi
bundle in the IGB project.

Note: this release removes SRA parsing code, which depends on an
artifact from NCBI. IGB does not parse SRA files, so we don't need to
include this additional artifact in IGB. 

In addition, we remove references to scala. 

See commit history for details.

To build and deploy:

* Clone repo and run `gradle jar` to build the artifact
* Upload to https://nexus.bioviz.org to make artifact available to IGB developers (Dr. Loraine will probably need to do this)

To develop new versions:

* Check upstream repository (samtools/htsjdk) for new release tags.
* Synchronize this fork with upstream repository (Dr. Loraine or other project maintainter can do this.)
* Fork this repository.
* Branch from the same commit that is tagged with the new upstream release; call it igb-{version}. 
* Update the gradle files accordingly. 
* Cherry-pick commits from the previous igb-{version} branch on our fork. 
* Build the bundle and test with IGB code base. For testing, you will need to change the version the top-level POM.xml for the htsjdk-igb artifact in the IGB code base.

**Note**: Our branch will not pass the tests because we are only using functionality required in IGB. Code for reading SRA files is not included, for example.



* * * 

[![Coverage Status](https://codecov.io/gh/samtools/htsjdk/branch/master/graph/badge.svg)](https://codecov.io/gh/samtools/htsjdk)
[![Build Status](https://travis-ci.org/samtools/htsjdk.svg?branch=master)](https://travis-ci.org/samtools/htsjdk)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.samtools/htsjdk/badge.svg)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.github.samtools%22%20AND%20a%3A%22htsjdk%22)
[![License](http://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/samtools/htsjdk)
[![Language](http://img.shields.io/badge/language-java-brightgreen.svg)](https://www.java.com/)
[![Join the chat at https://gitter.im/samtools/htsjdk](https://badges.gitter.im/samtools/htsjdk.svg)](https://gitter.im/samtools/htsjdk?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Status of downstream projects automatically built on top of the current htsjdk master branch. See [gatk-jenkins](https://gatk-jenkins.broadinstitute.org/view/HTSJDK%20Release%20Tests/) for detailed logs. Failure may indicate problems  in htsjdk, but may also be due to expected incompatibilities between versions, or unrelated failures in downstream projects.
- [Picard](https://github.com/broadinstitute/picard):  [![Build Status](https://gatk-jenkins.broadinstitute.org/buildStatus/icon?job=picard-on-htsjdk-master)](https://gatk-jenkins.broadinstitute.org/job/picard-on-htsjdk-master/)
- [GATK 4](https://github.com/broadinstitute/gatk): [![Build Status](https://gatk-jenkins.broadinstitute.org/buildStatus/icon?job=gatk-on-htsjdk-master)](https://gatk-jenkins.broadinstitute.org/job/gatk-on-htsjdk-master/)

## A Java API for high-throughput sequencing data (HTS) formats.  

HTSJDK is an implementation of a unified Java library for accessing
common file formats, such as [SAM][1] and [VCF][2], used for high-throughput
sequencing data.  There are also an number of useful utilities for 
manipulating HTS data.

> **NOTE: _HTSJDK does not currently support the latest Variant Call Format Specification (VCFv4.3 and BCFv2.2)._**

### Documentation & Getting Help

API documentation for all versions of HTSJDK since `1.128` are available through [javadoc.io](http://www.javadoc.io/doc/com.github.samtools/htsjdk).

If you believe you have found a bug or have an issue with the library please a) search the open and recently closed issues to ensure it has not already been reported, then b) log an issue.

The project has a [gitter chat room](https://gitter.im/samtools/htsjdk) if you would like to chat with the developers and others involved in the project.

To receive announcements of releases and other significant project news please subscribe to the [htsjdk-announce](https://groups.google.com/forum/#!forum/htsjdk-announce) google group.

### Building HTSJDK

HTSJDK is now built using [gradle](http://gradle.org/).

A wrapper script (`gradlew`) is included which will download the appropriate version of gradle on the first invocation.

Example gradle usage from the htsjdk root directory:
 - compile and build a jar 
 ```
 ./gradlew
 ```
 or
 ```
 ./gradlew jar
 ```
 The jar will be in build/libs/htsjdk-\<version\>.jar where version is based on the current git commit.

 - run tests, a specific test class, or run a test and wait for the debugger to connect
 ```
 ./gradlew test

 ./gradlew test -Dtest.single=TestClassName

 ./gradlew test --tests htsjdk.variant.variantcontext.AlleleUnitTest
 ./gradlew test --tests "*AlleleUnitTest"

 ./gradlew test --tests "*AlleleUnitTest" --debug-jvm
 ```

- run tests and collect coverage information (report will be in `build/reports/jacoco/test/html/index.html`)
```
./gradlew jacocoTestReport
```

 - clean the project directory
 ```
 ./gradlew clean
 ```

 - build a monolithic jar that includes all of htsjdk's dependencies
 ```
 ./gradlew shadowJar
 ```
 
 - create a snapshot and install it into your local maven repository
 ```
 ./gradlew install
 ```

 - for an exhaustive list of all available targets
 ```
 ./gradlew tasks
 ```

### Create an HTSJDK project in IntelliJ
To create a project in IntelliJ IDE for htsjdk do the following:

1. Select fom the menu: `File -> New -> Project from Existing Sources`
2. In the resulting dialog, chose `Import from existing model`, select `Gradle` and `Next`
3. Choose the `default gradle wrapper` and `Finish`.

From time to time if dependencies change in htsjdk you may need to refresh the project from the `View -> Gradle` menu.

### Licensing Information

Not all sub-packages of htsjdk are subject to the same license, so a license notice is included in each source file or sub-package as appropriate. 
Please check the relevant license notice whenever you start working with a part of htsjdk that you have not previously worked with to avoid any surprises. 
Broadly speaking the majority of the code is covered under the MIT license with the following notable exceptions:

* Much of the CRAM code is under the Apache License, Version 2
* Core `tribble` code (underlying VCF reading/writing amongst other things) is under LGPL
* Code supporting the reading/writing of SRA format is uncopyrighted & public domain

### Java Minimum Version Support Policy

We will support all Java SE versions supported by Oracle until at least six months after Oracle's Public Updates period has ended ([see this link](http://www.oracle.com/technetwork/java/eol-135779.html)).

Java SE Major Release | End of Java SE Oracle Public Updates | Proposed End of Support in HTSJDK | Actual End of Support in HTSJDK
---- | ---- | ---- | ----
6 | Feb 2013 | Aug 2013 | Oct 2015
7 | Apr 2015 | Oct 2015 | Oct 2015
8 | Jul 2018 | Jul 2018 | TBD


HTSJDK is migrating to semantic versioning (http://semver.org/). We will eventually adhere to it strictly and bump our major version whenever there are breaking changes to our API, but until we more clearly define what constitutes our official API, clients should assume that every release potentially contains at least minor changes to public methods.

[1]: http://samtools.sourceforge.net
[2]: http://vcftools.sourceforge.net/specs.html

