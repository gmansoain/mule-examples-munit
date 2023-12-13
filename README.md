<h1>Mule Examples MUnit</h1>

## Description
This app is an example of how to use the MUnit Maven Plugin

## POM File
Here's what we need to include in this project to configure the MUnit maven plugin. Notice that:
- runtimeProduct is set to MULE_EE to specify Mule Enterprise version. This version is required to run coverage for your MUnit tests.
- For this you need access to the Mule Enterprise Nexus repository. Make sure you've got a valid username and password. These credentials are provided in the maven settings.xml file
- The formats tag specifies the different formats MUnit can generate for the report:
    - HTML
    - Console - We'll see the result in the output of the github actions step
    - JSON
    - SONAR (not included in this project)
 - The Coverage tag allows us to specify
    - failBuild - true/false If set to true it won't build the jar in the specified coverage is not passed
    - You can specify three levels
       - Application
       - Resource
       - Flow

```sh
<plugin>
  <groupId>com.mulesoft.munit.tools</groupId>
  <artifactId>munit-maven-plugin</artifactId>
  <version>${munit.version}</version>
  <executions>
    <execution>
      <id>test</id>
      <phase>test</phase>
      <goals>
        <goal>test</goal>
        <goal>coverage-report</goal>
      </goals>
    </execution>
  </executions>
  <configuration>
    <runtimeProduct>MULE_EE</runtimeProduct>
    <redirectTestOutputToFile>true</redirectTestOutputToFile>
    <coverage>
      <runCoverage>true</runCoverage>
      <failBuild>false</failBuild>
      <requiredApplicationCoverage>75</requiredApplicationCoverage>
      <requiredResourceCoverage>50</requiredResourceCoverage>
      <requiredFlowCoverage>50</requiredFlowCoverage>

      <formats>
        <format>console</format>
        <format>html</format>
        <format>json</format>
      </formats>
    </coverage>
  </configuration>
</plugin>
```

## Maven settings.xml file
In order to use Coverage for our MUnits it is required to use valid credentials for the Mule Nexus Enterprise Repo. Credentials are provided adding a server node to the settings.xml file
In this project we use the placeholders NEXUX_USERNAME and NEXUS_PASSWORD. Values are provided as Secrets of this repo that will be used by the CI github actions workflow
```
<server>
  <id>MuleRepository</id>
  <username>${env.NEXUS_USERNAME}</username>
  <password>${env.NEXUS_PASSWORD}</password>
</server>
```

## Useful Links


## Author

👤 **Gonzalo Marcos**

* Website: [@\MyBlog](https://mulegon.blogspot.com/)
* Twitter: [@\_ShareGon](https://twitter.com/\_ShareGon)
* Github: [@gmansoain](https://github.com/gmansoain)
* LinkedIn: [@gmansoain](https://linkedin.com/in/gmansoain)
