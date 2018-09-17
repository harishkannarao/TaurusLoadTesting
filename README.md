# Taurus Load Testing
This repository is to load testing http endpoint using blazemeter taurus using Apache JMeter executor. It also demonstration auto generation of html reports in local machine using JMeter without using the blazemeter online reporting services. Also it explains steps to generate a unified html report if the load test is run from multiple machines to simulate distributed load testing.

### Required Software and Tools
* Java Version: Oracle Java 1.8.0_31 (Execute **_java -version_** in command line after installation)
* Docker Version: Docker version 17.03.1-ce, build c6d412e (Execute **_docker --version_** in command line after installation)

### To run load tests
The following command runs the load tests with sanity profile targetting the development environment. The following command can be easily parameterised to run with different load profile against a specified environment.

    docker run --rm -v `pwd`:/TaurusLoadTesting blazemeter/taurus /TaurusLoadTesting/profile/sanity.yml /TaurusLoadTesting/common/scenarios.yml /TaurusLoadTesting/variable/development.yml

The html report and other files are located under the `build` directory

### To generate unified html nodes
Let us assume that you had run the load test from multiple nodes using the above docker command to simulate clustered mode of load testing. This is supported by many cloud providers to run temprovary virtual machines based on docker commands, which can be used to run load tests in parallel and it can be scaled horizontally.
From individual nodes, you can collect the jmeter_stats.jtl file into a local directory.

Let the files be placed under `build` directory as `jmeter_stats_node1.jtl`, `jmeter_stats_node2.jtl` and `jmeter_status_node3.jtl`. The the following commands allows us to generate a unified html report by combining all three jtl files.

    awk 'FNR==1 && NR!=1{next;}{print}' build/*/jmeter_stats*.jtl > build/jmeter_status_full.jtl

    /opt/apache-jmeter/bin/jmeter -g build/jmeter_status_full.jtl -o build/jmeterFullHtmlReport