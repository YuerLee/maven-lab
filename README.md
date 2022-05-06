# Maven Lab

## 建立專案

1. 建立 parent project
    ```shell
    $ mvn archetype:generate \
        -DgroupId="com.cbxsoftware.cbx" \
        -DartifactId="parent-project"
    ```
1. 刪除 parent project 的 `src/`
1. 指定 packaging 為 `pom`
    ```xml
    <!--  parent-project/pom.xml -->
    <packaging>pom</packaging>
    ```
1. 建立子專案
    - web: webapp 架構（packaging 會是 `war`）
        ```shell
        # parent-project/
        $ mvn archetype:generate \
            -DarchetypeGroupId="org.apache.maven.archetypes" \
            -DarchetypeArtifactId="maven-archetype-webapp" \
            -DarchetypeVersion="1.4" \
            -DgroupId="com.cbxsoftware.cbx" \
            -DartifactId="web"
        ```
    - core, service: 一般架構（Packaging 會是預設的 `jar`）
        ```shell
        $ mvn archetype:generate \
            -DgroupId="com.cbxsoftware.cbx" \
            -DartifactId="core"
        $ mvn archetype:generate \
            -DgroupId="com.cbxsoftware.cbx" \
            -DartifactId="service"
        ```

## 依賴設定

![](https://i.imgur.com/ne6HFh5.png)

1. 所有子專案都要依賴 `junit` 及 `org.slf4j` ，設定於上層
    ```xml
    <!-- parent-project/pom.xml -->
    <dependencies>
        <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.11</version>
          <scope>test</scope>
        </dependency>

        <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-api</artifactId>
          <version>1.7.36</version>
        </dependency>
    </dependencies>
    ```
1. 只有 core 要用到 `commons-lang3`，設定於該層
    ```xml
    <!-- parent-project/core/pom.xml -->
    <dependencies>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.12.0</version>
        </dependency>
    </dependencies>
    ```
1. web 內要用到 core & service ，設定於該層
    ```xml
    <!-- parent-project/web/pom.xml -->
    <dependencies>
        <dependency>
            <groupId>com.cbxsoftware.cbx</groupId>
            <artifactId>core</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.cbxsoftware.cbx</groupId>
            <artifactId>service</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
    ```

## 安裝依賴

於 parent project 執行 clean install

```shell=
# parent-project/
$ mvn clean install
```

![](https://i.imgur.com/HzB3ShM.png)
