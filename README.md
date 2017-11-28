# Instructions

This project generates from the `istio/api` the Java classes using the Google Protobuf `protoc` converter. The tool can be installed
locally using a brew command `brew install protobuf`

If Golang is already installed on you machine, then get the Google Protobuf project as it is used to convert different Google types
such as `ExtensionRegistry, ExtensionRegistryLite, MEssageOrBuilder, ...`

```bash
go get github.com/google/protobuf
```

Next, clone the istio api project which contains the definition of the mixer. broker and proxy types

```bash
cd /path/to/go/workspace/src/istio.io
git clone https://github.com/istio/api
```

Create locally a new project to host the Java classes generated

```
mkdir -p java_model/src/main/java
cat <<EOF > java_model/pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.istio</groupId>
    <artifactId>java-model</artifactId>
    <version>0.3-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.google.protobuf</groupId>
            <artifactId>protobuf-java</artifactId>
            <version>3.5.0</version>
        </dependency>
    </dependencies>
    
</project>
EOF
```

Move to the api github project and run the protoc tool to generate the java classes

```bash
cd api

protoc --java_out=../java_model/src/main/java \
	   -I=. \
       proxy/v1/config/http_fault.proto \
       proxy/v1/config/l4_fault.proto \
       proxy/v1/config/route_rule.proto \
       proxy/v1/config/dest_policy.proto \
       proxy/v1/config/egress_rule.proto \
       proxy/v1/config/ingress_rule.proto \
       proxy/v1/config/proxy_mesh.proto   
```
    
 