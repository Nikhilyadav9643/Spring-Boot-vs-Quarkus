# Performance Comparison Journal: Spring Boot vs Quarkus

## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Environment Setup](#2-environment-setup)
  - [Technologies Used](#technologies-used)
  - [System Information](#system-information)
  - [Spring Boot (JVM and Native)](#spring-boot-jvm-and-native)
  - [Quarkus (JVM and Native)](#quarkus-jvm-and-native)
- [3. Performance Testing with JMeter](#3-performance-testing-with-jmeter)
  - [Test Plan](#test-plan)
  - [Execution](#execution)
- [4. Profiling and Monitoring](#4-profiling-and-monitoring)
- [5. Test Results and Metrics](#5-test-results-and-metrics)
  - [Comparison Tables](#comparison-tables)
- [6. Observations and Analysis](#6-observations-and-analysis)
- [7. Conclusion](#7-conclusion)

## 1. Introduction
This project compares how two popular Java frameworks, Spring Boot and Quarkus, perform when running the same application. The goal is to assess the frameworks' response times, memory and CPU usage, and their ability to handle heavy traffic. Both the regular version (which runs on the Java Virtual Machine) and the native version (which starts up faster) will be tested. Using JMeter to simulate user activity, the project aims to determine which framework is better suited for building fast and efficient applications.

## 2. Environment Setup

### Technologies Used
- **Spring Boot**: Version used for the Spring Boot application.
- **Quarkus**: Version used for the Quarkus application.
- **Docker**: Used for containerization and running the applications. (Docker Version: 27.3.1, build ce12230)
- **JMeter**: Version used for performance testing the applications.
- **MySQL**: Version 5.7.38, used as the database for both applications.
- **Maven**: Apache Maven 3.8.7
- **Java Version**: openjdk version "17.0.12" 2024-07-16

### System Information
- **Operating System**: Ubuntu 24.04.1 LTS (Noble Numbat)
- **CPU**: 4 × Intel® Core™ i5-6300U CPU @ 2.40GHz
- **RAM**: 16 GB DDR4
- **Disk Space**: 512 GB SSD

### Spring Boot (JVM and Native)
- **Spring Boot** is a tool for building Java applications quickly and easily. It comes with built-in features that reduce the need for extensive setup, such as an embedded server to run the application. This allows developers to focus more on writing the main components of their applications instead of worrying about configuration. Spring Boot also supports the creation of fast and responsive applications that can handle numerous requests simultaneously, making it an excellent choice for modern software development.

### Quarkus (JVM and Native)
- **Quarkus** is a framework for Java that simplifies building applications tailored for cloud environments. It focuses on making applications lightweight and fast, enabling quick launches and efficient operation in containers. Quarkus accommodates various programming approaches and offers features that enhance the development process, such as instant updates during coding. Its goal is to improve developer productivity while optimizing the performance of applications designed for modern software infrastructures.

## 3. Performance Testing with JMeter

### Test Plan
1. **Create a Test Plan**:
   - In JMeter, navigate to `File > New` to create a new test plan.

2. **Add a Thread Group**:
   - Right-click `Test Plan > Add > Threads (Users) > Thread Group`.
   - Configure the parameters:
     - **Number of Threads**: Define the number of virtual users (e.g., 50 users).
     - **Ramp-up Period**: Time for all users to start (e.g., 10 seconds).
     - **Loop Count**: Number of times to execute the test (e.g., 10 iterations).

3. **Add HTTP Request (Optional: For Web Testing)**:
   - Right-click `Thread Group > Add > Sampler > HTTP Request`.
   - Configure the following:
     - **Server Name or IP**: Enter the server address or domain (e.g., localhost).
     - **Path**: Enter the target URL path (e.g., /api/test).

4. **Add Listeners for Results**:
   - Right-click `Thread Group > Add > Listener`.
   - Choose from:
     - **View Results Tree**: For detailed information on every request.
     - **Summary Report**: To obtain summary statistics (e.g., throughput, errors).
     - **Graph Results**: To visualize performance trends.

5. **Set Up PerfMon for Monitoring CPU, Memory, Disk, etc.**
   - **Install Server Agent on Target Machine**:
     - Download Server Agent from the JMeter Plugins page.
     - Extract the archive on the machine intended for monitoring (typically the target server where the application is hosted).

   - **Start the Server Agent**:
     - On the target server, navigate to the extracted Server Agent directory and execute the following command:
       - On Linux/Mac:
         ```bash
         ./startAgent.sh
         ```
     - By default, the server agent listens on port 4444.

   - **Add PerfMon Metrics Collector in JMeter**:
     - In JMeter, right-click `Thread Group > Add > Listener > jp@gc - PerfMon Metrics Collector`.
     - In the Metrics Collector window, click `Add row` and configure:
       - **Host**: Enter the IP address or hostname of the target server.
       - **Port**: Default is 4444, which must match the Server Agent’s listening port.
       - **Metric to collect**: Select the metric to monitor (e.g., CPU, Memory, Disk I/O, Network).

   - **Configure Multiple Metrics**:
     - Multiple rows can be added to monitor different metrics simultaneously:
       - **CPU Usage**: Measures CPU consumption on the target server.
       - **Memory Usage**: Monitors memory consumption.

### Note
- This test plan will be used for both applications.

### Execution
- **Save the Test Plan**: Once the test plan is configured, go to `File > Save` to save the test plan.
- **Run the Test**: Click the green Start button on the JMeter toolbar. The test will start executing the defined load (virtual users) and will monitor the target server.
- **Monitor CPU, Memory, and Other Metrics**: The PerfMon Metrics Collector will gather system metrics (e.g., CPU, memory) from the target server.

## 4. Profiling and Monitoring
- Tools used (e.g., VisualVM, top, htop) for resource profiling.
- CPU and memory usage monitoring during JMeter tests for both applications (JVM and native versions).
- Screenshots or graphs showing memory, CPU, and thread utilization can be attached.

## 5. Test Results and Metrics

### Comparison Tables
- Results are tabulated for better visualization of differences.
- Metrics such as:
  - Average Response Time (ms)
  - 95th Percentile Response Time (ms)
  - Throughput (requests/sec)
  - Memory Usage (MB)
  - CPU Utilization (%)
  - Startup Time (s)

## 6. Observations and Analysis
- Key observations from the tests, including which framework performed better under load, resource usage efficiency, and differences between JVM and native versions.
- Discussion of any specific bottlenecks or unexpected behavior (e.g., higher memory usage in one framework).

## 7. Conclusion
- The findings from the comparison are summarized.
- Highlights are made regarding which framework (Spring Boot or Quarkus) performed better overall and under what conditions.
- Any trade-offs between the two frameworks (e.g., startup time, memory usage, native vs. JVM performance) are mentioned.
