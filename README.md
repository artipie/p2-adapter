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
| | |-- content.xml / content.jar (e.g. zipped content.xml)
| | |-- artifacts.xml / artifacts.jar (e.g. zipped artifacts.xml)
| |-+ ...
|-+ updates
| | ... similar to the folder `releases`
```  

- `p2.index` -- description of index file is available [here](https://wiki.eclipse.org/Equinox/p2/p2_index)  
- Feature archives -- description of archives which are located in `features` folder [docs](https://help.eclipse.org/latest/topic/org.eclipse.platform.doc.isv/guide/product_def_feature.htm?cp=2_0_21_1)
These files contain the `feature.xml` file inside.

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
    <child location='3.14.10.v20210917-1938'/>
    <child location='4.11.5.v20210930-1251'/>
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
    <child location='3.14.10.v20210917-1938'/>
    <child location='4.11.5.v20210930-1251'/>
  </children>
</repository>
```  

#### Simpe Metadata Repository
File `content.xml`:
```xml
<?xml version='1.0' encoding='UTF-8'?>
<?metadataRepository version='1.2.0'?>
<repository name='' type='org.eclipse.equinox.internal.p2.metadata.repository.LocalMetadataRepository' version='1'>
  <properties size='1'>
    <property name='p2.timestamp' value='1614818273187'/>
  </properties>
  <units size='19'>
    <unit id='org.junit' version='4.13.0.v20200204-1500' singleton='false' generation='2'>
      ...
    </unit>
    ...
  </units>
</repository>
```

#### Simple Artifact Repository
File `artifact.xml` contains information about `.jar` files 
in the subfolders of the current directory. An example of such file:
```xml
<?xml version='1.0' encoding='UTF-8'?>
<?artifactRepository version='1.1.0'?>
<repository name='' type='org.eclipse.equinox.p2.artifact.repository.simpleRepository' version='1'>
  <properties size='2'>
    <property name='p2.timestamp' value='1614818273187'/>
    <property name='publishPackFilesAsSiblings' value='true'/>
  </properties>
  ...
  <artifacts size='17'>
    <artifact classifier='osgi.bundle' id='org.junit.jupiter.api' version='5.7.1.v20210222-1948'>
      <properties size='5'>
        <property name='download.size' value='204027'/>
        <property name='artifact.size' value='204027'/>
        <property name='download.md5' value='f827b34590464f21f09fc18bedb4b8c6'/>
        <property name='download.checksum.md5' value='f827b34590464f21f09fc18bedb4b8c6'/>
        <property name='download.checksum.sha-256' value='3cb3ad7afb0d0d6f30ee1bcc043675b163d81612edd286a28429e5762413d72b'/>
      </properties>
    </artifact>
    ...
  </artifacts>
</repository>
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