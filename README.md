<!--
Licensed to the Apache Software Foundation (ASF) under one or more
contributor license agreements.  See the NOTICE file distributed with
this work for additional information regarding copyright ownership.
The ASF licenses this file to You under the Apache License, Version 2.0
(the "License"); you may not use this file except in compliance with
the License.  You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

![logo](https://phoenix.apache.org/images/phoenix-logo-small.png)

<b>[Apache Phoenix](http://phoenix.apache.org/)</b> enables OLTP and operational analytics in Hadoop for low latency applications. Visit the Apache Phoenix website <b>[here](http://phoenix.apache.org/)</b>. This is the repo for the Phoenix Query Server (PQS).

Copyright ©2020 [Apache Software Foundation](http://www.apache.org/). All Rights Reserved.

## Introduction

The Phoenix Query Server is an JDBC over HTTP abstraction. The Phoenix Query Server proxies the standard
Phoenix JDBC driver and provides a backwards-compatible wire protocol to invoke that JDBC driver. This is
all done via the Apache Avatica project (sub-project of Apache Calcite).

The reference client implementation for PQS is a "thin" JDBC driver which can communicate with PQS. There
are drivers in other languages which exist in varying levels of maturity including Python, Golang, and .NET.

## Building

This repository will build a tarball which is capable of running the Phoenix Query Server.

By default, this tarball does not contain a Phoenix client jar as it is meant to be agnostic
of Phoenix version (one PQS release can be used against any Phoenix version). Today, PQS builds against
the Phoenix 4.15.0-HBase-1.4 release.

```
$ mvn package
```

### Bundling a Phoenix Client

To build a release of PQS which packages a specific version of Phoenix, specify the `package-phoenix-client` system property
and specify the `phoenix.version` system property to indicate a specific Phoenix version.

PQS will package the same version of Phoenix used for build/test. This version is controlled by the
 `phoenix.version` system property.

```
$ mvn package -Dpackage.phoenix.client -Dphoenix.version=5.1.0-SNAPSHOT
```

### Running integration tests

`mvn package` will run the unit tests while building, but it will not run the integration test suite.

The IT suite is run when executing `mvn install` or `mvn verify`. The Phoenix version specified
with `phoenix.version` is used for running the integration tests.

When specifying `phoenix.version`, also specify the HBase version to be used
for integration testing by activating the corresponding `hbase<minor.major>` profile.

When using a Phoenix 5.x version, activate the `hbase-2.x` profile in addition to the
profile for the minor 2.x version.

```
$ mvn verify -Dpackage.phoenix.client -Dphoenix.version=4.16.0-SNAPSHOT -Phbase-1.3
```
```
$ mvn install -Dpackage.phoenix.client -Dphoenix.version=5.1.0-SNAPSHOT -Phbase-2.1 -Phbase-2.x
```
