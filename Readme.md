### Spark Scala Project

#### 1.Install Java

    download:
    https://download.oracle.com/otn-pub/java/jdk/8u201-b09/42970487e3af4f5aa5bca3f542482c60/jdk-8u201-linux-x64.tar.gz?AuthParam=1548475597_3e8046d4e642f18fbb991df42ebc8865
    check:
    https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

    csk@csk-ai-revolution:~$ cd Dowloads
    csk@csk-ai-revolution:~/Downloads$ tar -xvf jdk-8u201-linux-x64.tar.gz
    jdk1.8.0_201/
    jdk1.8.0_201/javafx-src.zip
    jdk1.8.0_201/bin/
    csk@csk-ai-revolution:~/Downloads$ sudo mkdir -p /usr/lib/jvm/jdk1.8.0
    [sudo] password for csk:
    csk@csk-ai-revolution:~/Downloads$ sudo mv jdk1.8.0_201/* /usr/lib/jvm/j
    java-1.8.0-openjdk-amd64  jdk1.8.0/
    csk@csk-ai-revolution:~/Downloads$ sudo mv jdk1.8.0_201/* /usr/lib/jvm/jdk1.8.0/
    csk@csk-ai-revolution:~/Downloads$ sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0/bin/java" 1
    update-alternatives: using /usr/lib/jvm/jdk1.8.0/bin/java to provide /usr/bin/java (java) in auto mode
    csk@csk-ai-revolution:~/Downloads$ sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0/bin/javac" 1
    update-alternatives: using /usr/lib/jvm/jdk1.8.0/bin/javac to provide /usr/bin/javac (javac) in auto mode
    csk@csk-ai-revolution:~/Downloads$ sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.8.0/bin/javaws" 1
    update-alternatives: using /usr/lib/jvm/jdk1.8.0/bin/javaws to provide /usr/bin/javaws (javaws) in auto mode

    csk@csk-ai-revolution:~/Downloads$ sudo nano /etc/profile
    JAVA_HOME=/usr/lib/jvm/jdk1.8.0
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
    export JAVA_HOME
    export PATH

#### 2.Install Intellij

    download
    url: https://download-cf.jetbrains.com/idea/ideaIC-2018.3.3.tar.gz

    csk@csk-ai-revolution:~$ cd Downloads
    csk@csk-ai-revolution:~/Downloads$ sudo tar -xvf ideaIC-2018.3.3.tar.gz -C /opt/
    csk@csk-ai-revolution:~/Downloads$ nano idea.desktop
    [Desktop Entry]
    Encoding=UTF-8
    Name=IntelliJ IDEA
    Comment=IntelliJ IDEA
    Exec=/opt/idea-IC-183.5153.38/bin/idea.sh
    Icon=/opt/idea-IC-183.5153.38/bin/idea.png
    Terminal=false
    StartupNotify=true
    Type=Application

    csk@csk-ai-revolution:~/Downloads$ sudo mv idea.desktop /usr/share/applications/

    1. Open Intellij application
    2. go to plugin
    3. search sclaa
    4. Install scala

#### 3.Install sbt

    download
    url :https://dl.bintray.com/sbt/debian/sbt-1.2.8.deb

    csk@csk-ai-revolution:~$ cd Downloads/
    csk@csk-ai-revolution:~/Downloads$ sudo dpkg -i sbt-1.2.8.deb
    Selecting previously unselected package sbt.
    (Reading database ... 219245 files and directories currently installed.)
    Preparing to unpack sbt-1.2.8.deb ...
    Unpacking sbt (1.2.8) ...
    Setting up sbt (1.2.8) ...
    Creating system group: sbt
    Creating system user: sbt in sbt with sbt daemon-user and shell /bin/false
    Processing triggers for man-db (2.7.5-1) ...

#### 4.Create project

    https://hortonworks.com/tutorial/setting-up-a-spark-development-environment-with-scala/

    sbt-1.2.8
    scala-2.11.12

    src/main/scala/Helloscala1.scala

            import org.apache.spark.{SparkConf, SparkContext}
            object Helloscala1 {
              def main(args: Array[String]) {
                //Create a SparkContext to initialize Spark
                val conf = new SparkConf()
                conf.setMaster("local")
                conf.setAppName("Word Count")
                val sc = new SparkContext(conf)

                // Load the text into a Spark RDD, which is a distributed representation of each line of text
                val textFile = sc.textFile("/home/csk/IdeaProjects/Helloscala1/src/main/resources/shakespeare.txt")

                //word count
                val counts = textFile.flatMap(line => line.split(" "))
                  .map(word => (word, 1))
                  .reduceByKey(_ + _)

                counts.foreach(println)
                System.out.println("Total words: " + counts.count());
                counts.saveAsTextFile("/tmp/shakespeareWordCount2")
              }
            }


    build.sbt

        name := "Helloscala1"
        version := "0.1"
        scalaVersion := "2.11.12"
        // https://mvnrepository.com/artifact/org.apache.spark/spark-core
        libraryDependencies += "org.apache.spark" %% "spark-core" % "2.4.0"


#### 5.Build package

    csk@csk-ai-revolution:~/IdeaProjects/Helloscala1$ sbt package
    [info] Loading project definition from /home/csk/IdeaProjects/Helloscala1/project
    [info] Loading settings for project helloscala1 from build.sbt ...
    [info] Set current project to Helloscala1 (in build file:/home/csk/IdeaProjects/Helloscala1/)
    [info] Compiling 1 Scala source to /home/csk/IdeaProjects/Helloscala1/target/scala-2.11/classes ...
    [info] Done compiling.
    [info] Packaging /home/csk/IdeaProjects/Helloscala1/target/scala-2.11/helloscala1_2.11-0.1.jar ...
    [info] Done packaging.
    [success] Total time: 6 s, completed 26 Jan, 2019 11:16:44 PM

#### 6.Spark download:

    download
    url: https://spark.apache.org/downloads.html
    download : https://www.apache.org/dyn/closer.lua/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz

    csk@csk-ai-revolution:~/sparkscala$ tar -xvf spark-2.4.0-bin-hadoop2.7.tgz

#### 7.Deploy project

    csk@csk-ai-revolution:~/sparkscala/spark-2.4.0-bin-hadoop2.6/bin$ ./spark-submit --class Helloscala1  --master local /home/csk/IdeaProjects/Helloscala1/target/scala-2.11/helloscala1_2.11-0.1.jar

    csk@csk-ai-revolution:/tmp$ cd shakespeareWordCount2
    csk@csk-ai-revolution:/tmp/shakespeareWordCount2$ ls
    part-00000  _SUCCESS
    csk@csk-ai-revolution:/tmp/shakespeareWordCount2$ nano part-00000

    Command line args:
    
    csk@csk-ai-revolution:~/IdeaProjects/Helloscala1$ spark-submit --class Helloscala1  --master local /home/csk/IdeaProjects/Helloscala1/target/scala-2.11/helloscala1_2.11-0.1.jar /home/csk/IdeaProjects/Helloscala1/src/main/resources/shakespeare.txt /tmp/shakespeareWordCount3
    csk@csk-ai-revolution:~/IdeaProjects/Helloscala1$ cd /tmp/shakespeareWordCount3
    csk@csk-ai-revolution:/tmp/shakespeareWordCount3$ ls
    part-00000  _SUCCESS
