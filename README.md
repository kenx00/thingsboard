# ThingsBoard

ThingsBoard is an open-source IoT platform for data collection, processing, visualization, and device management.

!!! Please refer official github repository for the full thingsboard. This git version is specific for Visual Studio Code + Ubuntu use and there are several Windows related and black-box-tests related are removed so to optimize for local development.

## Basic Information

The build environment info.

- Intel Core i7-10700
- RAM 64.0 GiB
- SSD 1.0 TB
- Graphic Quadro P620
- Ubuntu 24.04.2 LTS
- Linux 6.8.0-53-generic

Tools

- Visual Studio Code
- Java

```bash
$ java --version
openjdk 17.0.14 2025-01-21
OpenJDK Runtime Environment (build 17.0.14+7-Ubuntu-124.04)
OpenJDK 64-Bit Server VM (build 17.0.14+7-Ubuntu-124.04, mixed mode, sharing)
```

- Maven

```bash
$ mvn --version
Apache Maven 3.9.9 (8e8579a9e76f7d015ee5ec7bfcdc97d260186937)
Maven home: /opt/maven
Java version: 17.0.14, vendor: Ubuntu, runtime: /usr/lib/jvm/java-17-openjdk-amd64
Default locale: en_HK, platform encoding: UTF-8
OS name: "linux", version: "6.8.0-53-generic", arch: "amd64", family: "unix"
```

- Protoc (Ubuntu apt included is old version - use this one https://github.com/protocolbuffers/protobuf/releases/download/v25.5/protoc-25.5-linux-x86_64.zip)

```bash
$ protoc --version
libprotoc 25.5
$ which protoc
/usr/local/protoc/bin/protoc
```

- fvm (for flutter application development use - need 3.24.4)

```bash
$ fvm --version
3.2.1
$ fvm list
Cache directory:  /home/XXX/fvm/versions
Directory Size: 1.76 GB

┌─────────┬─────────┬─────────────────┬──────────────┬──────────────┬────────┬───────┐
│ Version │ Channel │ Flutter Version │ Dart Version │ Release Date │ Global │ Local │
├─────────┼─────────┼─────────────────┼──────────────┼──────────────┼────────┼───────┤
│ stable  │ stable  │ 3.29.0          │ 3.7.0        │ Feb 12, 2025 │ ●      │       │
├─────────┼─────────┼─────────────────┼──────────────┼──────────────┼────────┼───────┤
│ 3.24.4  │ stable  │ 3.24.4          │ 3.5.4        │ Oct 24, 2024 │        │       │
└─────────┴─────────┴─────────────────┴──────────────┴──────────────┴────────┴───────┘
```

- flutter (for flutter application development use - install also Android Studio, Intellij Idea is not necessary)

```bash
$ fvm flutter doctor -v

┌─────────────────────────────────────────────────────────┐
│ A new version of Flutter is available!                  │
│                                                         │
│ To update to the latest version, run "flutter upgrade". │
└─────────────────────────────────────────────────────────┘
[✓] Flutter (Channel stable, 3.24.4, on Ubuntu 24.04.2 LTS 6.8.0-53-generic, locale en_HK.UTF-8)
    • Flutter version 3.24.4 on channel stable at /home/XXX/fvm/versions/3.24.4
    • Upstream repository https://github.com/flutter/flutter.git
    • Framework revision 603104015d (4 months ago), 2024-10-24 08:01:25 -0700
    • Engine revision db49896cf2
    • Dart version 3.5.4
    • DevTools version 2.37.3

[✓] Android toolchain - develop for Android devices (Android SDK version 35.0.1)
    • Android SDK at /home/XXX/Applications/Android/Sdk
    • Platform android-35, build-tools 35.0.1
    • Java binary at: /usr/lib/jvm/java-17-openjdk-amd64/bin/java
    • Java version OpenJDK Runtime Environment (build 17.0.14+7-Ubuntu-124.04)
    • All Android licenses accepted.

[✓] Chrome - develop for the web
    • Chrome at google-chrome

[✓] Linux toolchain - develop for Linux desktop
    • Ubuntu clang version 18.1.3 (1ubuntu1)
    • cmake version 3.28.3
    • ninja version 1.11.1
    • pkg-config version 1.8.1

[✓] Android Studio (version 2024.2)
    • Android Studio at /opt/android-studio
    • Flutter plugin version 83.0.3
    • Dart plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/6351-dart
    • Java version OpenJDK Runtime Environment (build 21.0.5+-12932927-b750.29)

[✓] IntelliJ IDEA Ultimate Edition (version 2024.1)
    • IntelliJ at /opt/ideaIU
    • Flutter plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/9212-flutter
    • Dart plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/6351-dart

[✓] VS Code (version 1.97.2)
    • VS Code at /usr/share/code
    • Flutter extension version 3.104.0

[✓] Connected device (2 available)
    • Linux (desktop) • linux  • linux-x64      • Ubuntu 24.04.2 LTS 6.8.0-53-generic
    • Chrome (web)    • chrome • web-javascript • Google Chrome 133.0.6943.126

[✓] Network resources
    • All expected network resources are available.

• No issues found!
```

- docker

```bash
$ docker --version
Docker version 27.3.1, build ce12230
```

- docker for Postgres (sample)

```bash
services:
  postgres:
    container_name: postgres
    image: postgres:16.6
    restart: always
    environment:
      POSTGRES_USER: XXX
      POSTGRES_PASSWORD: XXX
      POSTGRES_DB: tbdb
      TZ: Asia/Hong_Kong
    network_mode: 'host'
    volumes:
      - vPostgres:/var/lib/postgresql/data

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:8.14
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: XXX@XXX
      PGADMIN_DEFAULT_PASSWORD: XXX
      PGADMIN_LISTEN_PORT: 5050
      TZ: Asia/Hong_Kong
    depends_on:
      - postgres
    network_mode: 'host'
    volumes:
      - vPgadmin:/var/lib/pgadmin

volumes:
  vPostgres:
    external: true
  vPgadmin:
    external: true
```

- Environment variable for Postgres

```bash
vi /etc/environment
# TB DB Configuration
export DATABASE_TS_TYPE=sql
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/tbdb
export SPRING_DATASOURCE_USERNAME=tbdbopr1
export SPRING_DATASOURCE_PASSWORD=5G46RRANBPZS9GUB

# TB Specify partitioning size for timestamp key-value storage. Allowed values: DAYS, MONTHS, YEARS, INDEFINITE.
export SQL_POSTGRES_TS_KV_PARTITIONING=MONTHS
```

- bashrc

```bash
export MAVEN_OPTS="-Xmx16G -XX:+UseG1GC -XX:+UseStringDeduplication"
export NODE_OPTIONS="--max-old-space-size=8192"
export PATH="$PATH:$HOME/.pub-cache/bin"
export PATH="$PATH:/usr/lib/dart/bin"
export PATH=$PATH:/home/XXX/Applications/Android/Sdk/platform-tools
export PATH=$PATH:/usr/local/protoc/bin
```

## Installation

- First clone this repository with Visual Studio Code (vsc)
- Then wait for the vsc to auto first build
- There will have x2 erros left (java and js packaging)
  - These java/js modules are for packaging use (vsc currently unable to retrieve related properties and pass from Maven to Gradle)
  - This doesn't affect the coding and debugging, as the java project still can be run and debug
  - For final packaging, use command line to execute the build.sh instead (through command line the properties can be passed successfully)
- Start the docker Postgres first
- Run and Debug
- Open the browser to check http://localhost:8080 if can display the main page or return text error saying no static resources
  - If no static resources, in vsc terminal execute under project root

```bash
Ctrl-Shift-B manually trigger java (build): Build Workspace <= this one will trigger protoc to generate those proto java
./build_proto.sh
./build.sh
```

- Above build.sh step may repeat few times so to ensure all success

```bash
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for Thingsboard 4.0.0-SNAPSHOT:
[INFO]
[INFO] Thingsboard ........................................ SUCCESS [ 16.509 s]
[INFO] Thingsboard Server Commons ......................... SUCCESS [  1.011 s]
[INFO] Thingsboard Server Common Data ..................... SUCCESS [ 11.994 s]
[INFO] Thingsboard Server Common Utils .................... SUCCESS [  2.055 s]
[INFO] Netty MQTT Client .................................. SUCCESS [  3.509 s]
[INFO] Thingsboard Server Common Messages ................. SUCCESS [  3.457 s]
[INFO] Thingsboard Server Common Protobuf and gRPC structures SUCCESS [ 20.305 s]
[INFO] Thingsboard Actor system ........................... SUCCESS [  2.370 s]
[INFO] Thingsboard Server Stats ........................... SUCCESS [  0.907 s]
[INFO] Thingsboard Server Common Cluster API .............. SUCCESS [  0.603 s]
[INFO] Thingsboard Server Queue components ................ SUCCESS [  4.824 s]
[INFO] Thingsboard Server Common Cache .................... SUCCESS [  2.820 s]
[INFO] Thingsboard Server Commons ......................... SUCCESS [  0.369 s]
[INFO] Thingsboard Server Common Transport components ..... SUCCESS [  3.628 s]
[INFO] Thingsboard MQTT Transport Common .................. SUCCESS [  9.542 s]
[INFO] Thingsboard HTTP Transport Common .................. SUCCESS [  3.222 s]
[INFO] Thingsboard CoAP server ............................ SUCCESS [  3.325 s]
[INFO] Thingsboard CoAP Transport Common .................. SUCCESS [  7.111 s]
[INFO] Thingsboard LwM2M Transport Common ................. SUCCESS [  7.962 s]
[INFO] Thingsboard SNMP Transport Common .................. SUCCESS [  3.840 s]
[INFO] Thingsboard Server Common DAO API .................. SUCCESS [  2.538 s]
[INFO] Thingsboard Server Remote Edge wrapper ............. SUCCESS [ 14.365 s]
[INFO] Thingsboard Server Version Control API ............. SUCCESS [  2.020 s]
[INFO] Thingsboard Script Invoke Commons .................. SUCCESS [  0.055 s]
[INFO] Thingsboard Server Script invoke API ............... SUCCESS [  4.042 s]
[INFO] Thingsboard Extensions ............................. SUCCESS [  0.257 s]
[INFO] Thingsboard Rule Engine API ........................ SUCCESS [  1.985 s]
[INFO] Thingsboard Server JS Client for remote JS execution SUCCESS [  1.293 s]
[INFO] Thingsboard Server DAO Layer ....................... SUCCESS [ 28.772 s]
[INFO] Thingsboard Rule Engine Components ................. SUCCESS [ 11.891 s]
[INFO] Thingsboard Server Transport Modules ............... SUCCESS [  0.083 s]
[INFO] Thingsboard HTTP Transport Service ................. SUCCESS [01:12 min]
[INFO] Thingsboard MQTT Transport Service ................. SUCCESS [01:06 min]
[INFO] Thingsboard CoAP Transport Service ................. SUCCESS [01:05 min]
[INFO] Thingsboard LwM2m Transport Service ................ SUCCESS [01:07 min]
[INFO] Thingsboard SNMP Transport Service ................. SUCCESS [01:12 min]
[INFO] ThingsBoard Server UI .............................. SUCCESS [02:08 min]
[INFO] Thingsboard Server Tools ........................... SUCCESS [  1.276 s]
[INFO] Thingsboard Rest Client ............................ SUCCESS [  2.231 s]
[INFO] ThingsBoard Server Application ..................... SUCCESS [ 32.685 s]
[INFO] ThingsBoard Microservices .......................... SUCCESS [  1.502 s]
[INFO] ThingsBoard Docker Images .......................... SUCCESS [  0.231 s]
[INFO] ThingsBoard Web UI Microservice .................... SUCCESS [ 15.336 s]
[INFO] ThingsBoard Version Control Executor ............... SUCCESS [01:17 min]
[INFO] ThingsBoard Version Control Executor Microservice .. SUCCESS [  0.100 s]
[INFO] ThingsBoard Node Microservice ...................... SUCCESS [  0.119 s]
[INFO] ThingsBoard Transport Microservices ................ SUCCESS [  0.028 s]
[INFO] ThingsBoard MQTT Transport Microservice ............ SUCCESS [  0.121 s]
[INFO] ThingsBoard HTTP Transport Microservice ............ SUCCESS [  0.785 s]
[INFO] ThingsBoard COAP Transport Microservice ............ SUCCESS [  0.112 s]
[INFO] ThingsBoard LWM2M Transport Microservice ........... SUCCESS [  0.384 s]
[INFO] ThingsBoard SNMP Transport Microservice ............ SUCCESS [  0.110 s]
[INFO] ThingsBoard JavaScript Executor Microservice ....... SUCCESS [02:00 min]
[INFO] ThingsBoard Monitoring Service ..................... SUCCESS [01:44 min]
[INFO] ThingsBoard Monitoring Microservice ................ SUCCESS [  0.078 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  02:58 min (Wall Clock)
[INFO] Finished at: 2025-02-23T22:26:23+08:00
[INFO] ------------------------------------------------------------------------
```

- Finally trigger vsc clear workspace

```bash
Ctrl-Shift-P manually trigger Clear Java Language Server Workspace
```

- If next time the vsc still have many proto related error, manual trigger Ctrl-Shift-B java (build) again
