The project uses Gradle's [Gretty plugin][gretty] to run the web application:
```
git clone git@github.com:haba713/depcheck-gretty-issue.git
cd depcheck-gretty-issue/
./gradlew appRun
```
```
curl -s http://localhost:8080/

hello world
```

There are only two API dependencies in the runtime classpath:
```
./gradlew -q dependencies --configuration runtimeClasspath       
...
runtimeClasspath - Runtime classpath of source set 'main'.
+--- jakarta.servlet:jakarta.servlet-api:5.0.0
\--- jakarta.websocket:jakarta.websocket-api:2.0.0
```

The generated war contains no jars at all:
```
./gradlew war && unzip -t build/libs/depcheck-gretty-issue-1.0-SNAPSHOT.war 
...
Archive:  build/libs/depcheck-gretty-issue-1.0-SNAPSHOT.war
    testing: META-INF/                OK
    testing: META-INF/MANIFEST.MF     OK
    testing: index.jsp                OK
No errors detected in compressed data of build/libs/depcheck-gretty-issue-1.0-SNAPSHOT.war.
```

Despite the above, [dependencyCheckAnalyze][dca] reports vulnerabilities:
```
./gradlew dependencyCheckAnalyze                          
...
One or more dependencies were identified with known vulnerabilities in depcheck-gretty-issue:

apache-jsp-10.0.14.jar (pkg:maven/org.mortbay.jasper/apache-jsp@10.0.14, cpe:2.3:a:apache:tomcat:10.0.14:*:*:*:*:*:*:*) : CVE-2022-29885, CVE-2022-42252, CVE-2022-23181, CVE-2022-34305, CVE-2021-43980
bcprov-jdk18on-1.77.jar (pkg:maven/org.bouncycastle/bcprov-jdk18on@1.77, cpe:2.3:a:bouncycastle:bouncy-castle-crypto-package:1.77:*:*:*:*:*:*:*, cpe:2.3:a:bouncycastle:bouncy_castle_crypto_package:1.77:*:*:*:*:*:*:*, cpe:2.3:a:bouncycastle:bouncy_castle_for_java:1.77:*:*:*:*:*:*:*, cpe:2.3:a:bouncycastle:legion-of-the-bouncy-castle:1.77:*:*:*:*:*:*:*, cpe:2.3:a:bouncycastle:legion-of-the-bouncy-castle-java-crytography-api:1.77:*:*:*:*:*:*:*, cpe:2.3:a:bouncycastle:the_bouncy_castle_crypto_package_for_java:1.77:*:*:*:*:*:*:*) : CVE-2024-34447, CVE-2024-29857, CVE-2024-30171, CVE-2024-30172
commons-configuration-1.10.jar (pkg:maven/commons-configuration/commons-configuration@1.10, cpe:2.3:a:apache:commons_configuration:1.10:*:*:*:*:*:*:*) : CVE-2024-29131, CVE-2024-29133
springloaded-1.2.8.RELEASE.jar (pkg:maven/org.springframework/springloaded@1.2.8.RELEASE, cpe:2.3:a:pivotal_software:spring_framework:1.2.8:release:*:*:*:*:*:*, cpe:2.3:a:springsource:spring_framework:1.2.8:release:*:*:*:*:*:*, cpe:2.3:a:vmware:spring_framework:1.2.8:release:*:*:*:*:*:*) : CVE-2018-1270, CVE-2022-22965, CVE-2011-2730, CVE-2016-9878, CVE-2018-11040, CVE-2013-4152, CVE-2013-7315, CVE-2014-0054, CVE-2018-1257, CVE-2020-5421, CVE-2022-22950, CVE-2023-20861, CVE-2018-11039, CVE-2022-22968, CVE-2022-22970

See the dependency-check report for more details.
```

Why's that? Is there a fix or workaround for this?

[gretty]: https://gretty-gradle-plugin.github.io/gretty-doc/
[dca]: https://jeremylong.github.io/DependencyCheck/dependency-check-gradle/configuration.html