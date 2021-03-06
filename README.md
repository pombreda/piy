# piy


"POM in YAML" is a simple tool to write [Maven](http://maven.apache.org) POM files using [YAML](http://www.yaml.org/). Because many times XML can be too verbose...

## Status

**The tools is still in alpha!** All feedback and bug reports are welcome.

## Installation

From the code:

    python setup.py install

From PyPI:

    easy_install piy

## Usage

Once you have written your pom.yaml file, just execute:

    piy

This will show the generated pom.xml in the standard output. When you'd be happy with the result, you can save it by running:

    piy > pom.xml

As a geneal rule, I strongly recomment that generated files must not be managed by the source control systems (git, hg, svn or whatever). And this case is not an exception; so, please, don't commit the pom.xml file to your project if you want to avoid synchronization issues with other users/developers.

## How does it work

The tools pre-processes the pom.yaml file at the current directory, and generates a proper pom.xml. Then you can use Maven as usual.

Althougt with the same goal, the approach is completelly different to [Maven3 polyglot](http://polyglot.sonatype.org/), which adds to Maven3 the ability to work with pom files written in non-XML notations. 

## Syntax

Basically it's a translation of the current XML tree to YAML.

    project:

        modelVersion: 4.0.0
        groupId: com.foo
        artifactId: bar
        version: 1.0-SNAPSHOT
        packaging: jar

        build:
            plugins:
                plugin:
                    groupId: org.apache.maven.plugins
                    artifactId: maven-compiler-plugin
                    version: 2.5.1                
                    configuration:
                        source: 1.6
                        target: 1.6

        dependencies:
            dependency:
                groupId: com.bar
                artifactId: foo
                version: 1.0-SNAPSHOT

This will be transformed to a normal pom.xml file.

Although not very common in POMs, sometimes attributes are necessary in POM files. Since YAML doesn't support attributes, for adding attributes you'd need to start the name of the node with the underline symbol. See for instance the following example to configure the port with the Jetty plugin using such way:

    project:
        build:
            plugins:
                plugin:
                    groupId: org.mortbay.jetty
                    artifactId: maven-jetty-plugin
                    version: 6.1.10
                    configuration:
                        connectors:
                            connector:
                                _implementation: org.mortbay.jetty.nio.SelectChannelConnector
                                port: 8080

