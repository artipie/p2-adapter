<img src="https://www.artipie.com/logo.svg" width="64px" height="64px"/>

[![EO principles respected here](https://www.elegantobjects.org/badge.svg)](https://www.elegantobjects.org)
[![We recommend IntelliJ IDEA](https://www.elegantobjects.org/intellij-idea.svg)](https://www.jetbrains.com/idea/)

[![Javadoc](http://www.javadoc.io/badge/com.artipie/p2-adapter.svg)](http://www.javadoc.io/doc/com.artipie/p2-adapter)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/com.artipie/p2-adapter/blob/master/LICENSE.txt)
[![codecov](https://codecov.io/gh/artipie/p2-adapter/branch/master/graph/badge.svg)](https://codecov.io/gh/artipie/p2-adapter)
[![Hits-of-Code](https://hitsofcode.com/github/artipie/p2-adapter)](https://hitsofcode.com/view/github/artipie/p2-adapter)
[![Maven Central](https://img.shields.io/maven-central/v/com.artipie/p2-adapter.svg)](https://maven-badges.herokuapp.com/maven-central/com.artipie/p2-adapter)
[![PDD status](http://www.0pdd.com/svg?name=artipie/p2-adapter)](http://www.0pdd.com/p?name=artipie/p2-adapter)

This Java library turns your binary storage (files, S3 objects, anything)
into a P2 repository. You may add it to your binary storage and it
will become a fully-functional P2 repository. Source documentation 
is located [here](https://wiki.eclipse.org/Equinox/p2). API overview is available
[here](https://help.eclipse.org/latest/topic/org.eclipse.platform.doc.isv/guide/p2_api_overview.htm?cp=2_0_20_0).

## P2 Repository structure
P2 repository [has](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.platform.doc.isv%2Fguide%2Fp2_composite_repositories.htm)
the following structure:
```
(repository root) 
| 
|-- p2.index
|-- compositeContent.xml
|-- compositeArtifacts.xml
|-+ releases
| |-- p2.index
| |-- compositeContent.xml
| |-- compositeArtifacts.xml
| |-+ 3.14.10.v20210917-1938
| | |-+ features
| | | |-- *.jar
| | |-+ plugins
| | | |-- **
| | |-- p2.index
| | |-- content.xml / content.jar
| | |-- artifacts.xml / artifacts.jar
| |-+ ...
|-+ updates
| | ... similar to the folder `releases`
```  

`p2.index` -- description of index file is available [here](https://wiki.eclipse.org/Equinox/p2/p2_index)  
Feature archives -- description of archives which are located in `features` folder [docs](https://help.eclipse.org/latest/topic/org.eclipse.platform.doc.isv/guide/product_def_feature.htm?cp=2_0_21_1)

## [Sample metadata](https://wiki.eclipse.org/Equinox/p2/Composite_Repositories_(new))  

#### Composite Metadata Repository
File `compositeContent.xml`:
```xml
<?xml version='1.0' encoding='UTF-8'?>
<?compositeMetadataRepository version='1.0.0'?>
<repository name='&quot;Eclipse Project Test Site&quot;'
    type='org.eclipse.equinox.internal.p2.metadata.repository.CompositeMetadataRepository' version='1.0.0'>
  <properties size='1'>
    <property name='p2.timestamp' value='1243822502499'/>
  </properties>
  <children size='2'>
    <child location='childOne'/>
    <child location='childTwo'/>
  </children>
</repository>
```

#### Composite Artifact Repository
File: `compositeArtifacts.xml`:
```xml
<?xml version='1.0' encoding='UTF-8'?>
<?compositeArtifactRepository version='1.0.0'?>
<repository name='&quot;Eclipse Project Test Site&quot;'
    type='org.eclipse.equinox.internal.p2.artifact.repository.CompositeArtifactRepository' version='1.0.0'>
  <properties size='1'>
    <property name='p2.timestamp' value='1243822502440'/>
  </properties>
  <children size='2'>
    <child location='childOne'/>
    <child location='childTwo'/>
  </children>
</repository
```

## How to use Artipie P2 SDK

The next dependency is required:

```xml
<dependency>
  <groupId>com.artipie</groupId>
  <artifactId>p2-adapter</artifactId>
  <version>[...]</version>
</dependency>
```