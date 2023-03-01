# scholarly-articles-subset
Step-by-step instructions along with specification files for creating a subset of the scholarly articles (Q13442814) from Wikidata.

You'll need about 105 Gb of free space for the Wikidata dump and more than 100 Gb for the subset.

## 1. Download the latest Wikidata Dump
```
$ wget https://dumps.wikimedia.org/wikidatawiki/entities/latest-all.json.gz
```

## 2. Download Java 11
```
$ wget https://download.java.net/openjdk/jdk11/ri/openjdk-11+28_linux-x64_bin.tar.gz
$ tar zvxf openjdk-11+28_linux-x64_bin.tar.gz -C ~
$ export JAVA_HOME=$HOME/jdk-11
$ export PATH=$JAVA_HOME/bin/:$PATH
```
Check `$ java -version` to be like this:
```
openjdk version "11" 2018-09-25
OpenJDK Runtime Environment 18.9 (build 11+28)
OpenJDK 64-Bit Server VM 18.9 (build 11+28, mixed mode)
```

## 3. Download and install nvm (for node and npm)
```
$ curl -sL https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh -o install_nvm.sh
$ bash install_nvm.sh
$ source ~/.bashrc
```
You may need to use `$ source ~/.bash_profile` or other bash profile files like `.profile`, `.bashrc`, or `.zshrc` based on the Linux dist you are using.
Then, verify the nvm command:
```
$ command -v nvm
```
It should return `nvm` as the output. Once nvm is working, install node.js version 16:
```
$ nvm install 16
```
Check the node and npm versions:
```
$ node -v
v16.19.1
$ npm -v
8.19.3
```
## 4. Download and install gradle
```
$ wget https://services.gradle.org/distributions/gradle-8.0.1-bin.zip
$ unzip gradle-8.0.1-bin.zip -d ~
$ export GRADLE_HOME=$HOME/gradle-8.0.1
$ export PATH=$GRADLE_HOME/bin/:$PATH
```
Check the `$ gradle -v` be like:
```

------------------------------------------------------------
Gradle 8.0.1
------------------------------------------------------------

Build time:   2023-02-17 20:09:48 UTC
Revision:     68959bf76cef4d28c678f2e2085ee84e8647b77a

Kotlin:       1.8.10
Groovy:       3.0.13
Ant:          Apache Ant(TM) version 1.10.11 compiled on July 10 2021
JVM:          11 (Oracle Corporation 11+28)
OS:           Linux 3.10.0-1160.83.1.el7.x86_64 amd64
```

## 5. Download and install yarn
```
$ wget https://yarnpkg.com/latest.tar.gz
$ mkdir ~/yarn
$ tar zvxf latest.tar.gz -C ~/yarn --strip-components 1
$ export YARN_HOME=$HOME/yarn
$ export PATH=$YARN_HOME/bin:$PATH
$ yarn -v
1.22.19
```

## 6. Download and install WDumper
```
$ git clone https://github.com/bennofs/wdumper.git
$ cd wdumper/
$ ./gradlew install
```
Now check the `wdumper-cli` binary file has existed and (get back to the repo dir):
```
$ ls ./build/install/wdumper/bin/wdumper-cli
build/install/wdumper/bin/wdumper-cli
$ cd ..
```

## 8. Run the subclass adder script
To create a WDUmper specification file that includes all subclasses of scholarly articles (`-iz` is for adding only those subclasses that have at least one instance):
```
$ python3 add_subclasses.py scholarly_article.json ./scholarly_article_subclasses.json -iz
```

## 9. Run WDumper
```
$ wdumper/build/install/wdumper/bin/wdumper-cli ./latest-all.json.gz ./scholarly_article_subclasses.json
```
The output will be `wdump-1.nt.gz`. A standalone Blazegraph would be good to query that.