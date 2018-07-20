Vizor Infraworld Cornerstone
============================

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Build Status](https://travis-ci.org/vizor-games/infraworld-cornerstone.svg?branch=master)](https://travis-ci.org/vizor-games/infraworld-cornerstone)


Cornerstone is a fast and robust converter utility, used to generate UE4-friendly code to provide gRPC functionality
into your game. The generated code can be used for both C++ and Blueprints, and thus should be placed into your project's Source or Plugin's source folder to be able
to be compiled and executed. You shouldn't modify the generated code (at least within normal, not debug conditions). Instead if you want some
behavior change - you should look into the [Options](#options) section.

It has to be used with the [Infraworld Runtime](https://github.com/vizor-games/infraworld-runtime) plugin for Unreal Engine 4.

Building
========

You need a JDK 8+ installation (we recommend [Java SE Development Kit 8u172](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html),
but it was tested to be built and ran alongside with Java9 or Java10 as well).

Being written 100% in Java 8, Cornerstone can be built using Maven ([See how to intstall Maven for Windows](https://maven.apache.org/guides/getting-started/windows-prerequisites.html),
but I definitely recommend you to use [chocolatey](https://chocolatey.org) for package management).

To build the converter executable and run it's tests, just run:
>`mvn package`

Then you can find built jar as
`target/cornerstone.jar`. Since it is compiled as an executable jar, you may run it by just using:

>`java -jar target/cornerstone.jar`

Usage
=====

Since Cornerstone is a quite complex program, it uses command line arguments and/or configuration YML files to decide
what to do. Cornerstone has to be built with it's [base configuration file](src/main/resources/config.yml) to define
some base options.

Two possible ways to re-define options are:
- To copy the [base configuration file](src/main/resources/config.yml) and place at it the same level as compiled jar. Then
feel free to modify any option as you want. This saves you from providing every single option as command line option
each time you run the converter.
- To use command line arguments (run the jar with `--help` option to see the list of all available options). Note that
command line options are 'prime' options and they can override options, taken from the config. One more time:

> **Command line option** > **Override config option** > **Base config option**

List of available options:
* `src_path` A path, where *.proto source files are.
* `dst_path` A path, where generated C++ classes should be placed.
* `module_name` Name of the module (Required to compute an API, like `MYMODULE_API` for **MyModule** module name).
Left it blank if you don't want to use API. If not left blank, it mustn't contain any spaces, this is checked in run-time.
* `precompiled_header` An Unreal Header Tool requires each *.cpp file to include a precompiled header or PCH,
here you may define, which header is had to be used. In case you're using precompiled headers - provide a valid relative or absolute path to
the precompiled header.
* `wrappers_path` Name of the folder, where files generated by proto compiler (protoc) should be placed.
* `company_name` Company name will be used to declare category, where RPC methods will be placed.
* `log_level` Override log level if you want to. May be ignored, then log4j2.xml will be used.
* `no_fork` Set to true to force the converter to work in the single thread. False by default.

Additional options (only available from CLI):
* `--help` Prints help message and lists all available commands
* `--credits` Outputs the creators of Cornerstone

Limitations
===========

Despite the fact, that both [proto2](https://developers.google.com/protocol-buffers/docs/proto3) and
[proto3](https://developers.google.com/protocol-buffers/docs/proto2) syntaxes are supported, there's several limitations applied to your code:

- No support for [oneof](https://developers.google.com/protocol-buffers/docs/proto3#oneof) fields yet.
- No support for [import](https://developers.google.com/protocol-buffers/docs/proto3#other) statement yet.
- No support for [group](https://developers.google.com/protocol-buffers/docs/proto#nested) statement yet.
- No support for nested containers, e.g. you can't delcare a `map<map<int32, string>, string>`, because it isn't supported in UE4,
note that you **can** use `bytes` (which is actually a container type) for map's key and/or value type, `bytes` field can be `repeated`.

Debugging
=========

Since Cornerstone is an open source software, you may want to debug it or add some extra functionality.
Since it is distributed as Maven project, you can import it as a Maven solution for Eclipse, Intellij IDEA or any of
your favorite Java IDE.

Contribution
============

Please feel free to report known bugs, propose new features and improve tests using Github's pull request system.
Thank you very much for contributing into free software.
