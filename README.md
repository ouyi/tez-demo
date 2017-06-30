# What is this about

Apache Tez is a generalization and successor of the Hadoop MapReduce
implementation. Apache Pig and Hive have already switched to use Tez as the
default execution engine.

The tez-helloword project provides code example for a very simple native Tez
application, i.e., WordCount, the `Hello, world!` application for Hadoop.

It is based on the [WordCount code of the Tez examples](https://github.com/apache/tez/blob/master/tez-examples/src/main/java/org/apache/tez/examples/WordCount.java).
The additional values are:
- Simplified code
- Unit tests
- Gradle build scripts with minimal dependencies, and 
- Instructions for running the application on Hadoop inside a Docker container

The following steps require JDK and a working Docker installation.

# How to build

- Clone this project
- Build the artifact with Gradle
    ```
    ./gradlew build
    ```

# How to run

- Start the Docker container
    ```
    docker run -P -it ouyi/hadoop-docker:install-tez /etc/bootstrap.sh -bash 
    ```
- Transfer the jar file and the test data into the container, whose name assumed to be `my_container` in the following
    ```
    docker cp tez-helloworld/build/libs/tez-helloworld.jar my_container:/root/
    docker cp tez-helloword/src/test/resources/input.txt my_container:/root/
    ```
- Upload the files to HDFS
    ```
    hadoop fs -copyFromLocal -f tez-helloworld.jar /apps/tez/
    hadoop fs -copyFromLocal -f input.txt /tmp
    ```
- Start the application
    ```
    yarn jar tez-helloworld.jar io.github.ouyi.tez.HelloWorld /tmp/input.txt /tmp/output
    ```
- Verify the results
    ```
    hadoop fs -cat /tmp/output/{*}
    ```
