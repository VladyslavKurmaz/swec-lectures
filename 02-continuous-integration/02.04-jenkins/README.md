## Jenkins
* Install
  ```
  tln install default-jdk:jenkins
  ```
* Open page in browser **http://\<host-ip-address\>:8080**
* Complete instalation, using provided instructions (Install suggested plugins)
* Install additional plugins
  ```
  cd ~/projects/swe-course/swec-lectures/02-continuous-integration/02.04-jenkins
  ./install-plugins.sh
  ```

* Apply fix(es) for **"ALPN callback dropped: SPDY and HTTP/2 are disabled. Is alpn-boot on the boot class path?"**
  * Check your Java version 
    ```bash
    > java -version
    ```
    
    > openjdk version "1.8.0_191"
    > OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-0ubuntu0.16.04.1-b12)
    > OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
    
  * Find corresponding alpn boot library
  
    ```
    https://www.eclipse.org/jetty/documentation/9.4.x/alpn-chapter.html#alpn-versions
    ```
    | OpenJDK version | ALPN version |
    | --- | --- |
    | 1.8.0u191 | 8.1.13.v20181017 |
    
  * Copy link to the corresponding ALPN jar from 'ALPN version' folder
    ```
    http://central.maven.org/maven2/org/mortbay/jetty/alpn/alpn-boot/
    ```
    > http://central.maven.org/maven2/org/mortbay/jetty/alpn/alpn-boot/8.1.13.v20181017/alpn-boot-8.1.13.v20181017.jar
    
  * Goto JVM folder
    ```
    > cd /usr/lib/jvm
    ```
  * Download jar file
    ```
    > wget http://central.maven.org/maven2/org/mortbay/jetty/alpn/alpn-boot/8.1.13.v20181017/alpn-boot-8.1.13.v20181017.jar
    ```
  * Goto folder
    ```
    > cd /etc/default
    ```
  * Open for editing **jenkins** file
  * Modify property **JAVA_ARGS** variable
    from
    ```
    JAVA_ARGS="-Djava.awt.headless=true"
    ```
    too
    ```
    JAVA_ARGS="-Djava.awt.headless=true -Xbootclasspath/p:/usr/lib/jvm/alpn-boot-8.1.13.v20181017.jar -Dhudson.DNSMultiCast.disabled=true"
    ```
  * Restart Jenkins
    ```
    systemctl stop jenkins && systemctl start jenkins
    ```


### Configure plugins
Goto Manage **Manage Jenkins/Configure System**
* Configure "SonarQube servers" instance, name **SonarQube**
  ![](https://raw.githubusercontent.com/swe-course/swec-content/master/imgs/jenkins-sonar.png)
* "GitHub" instance + Github access credentials using created token
  ![](https://raw.githubusercontent.com/swe-course/swec-content/master/imgs/jenkins-github.png)
* "GitHub Pull Request Builder"
  ![](https://raw.githubusercontent.com/swe-course/swec-content/master/imgs/jenkins-ghprb.png)

### Configure tools
Goto Manage **Manage Jenkins/Global Tool Configuration**
* Configure "SonarQube Scanner", name - **SonarQube Scanner**
  ![](https://raw.githubusercontent.com/swe-course/swec-content/master/imgs/jenkins-tools-sonar-scanner.png)
