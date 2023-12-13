<h1>Mule Examples MUnit</h1>

## Description
This app is an example of how to use the MUnit Maven Plugin

## GitHub Repo Setup
- Two Environments:
    - Production
    - Development
- Variables
  
|Variable|Scope|Description|
|--------|-----|-----------|
|ENVIRONMENT|Environment|Anypoint Environment name|
|APP_ENV|Environment|Sufix for the env property to identify environement in the mule app|
|API_ID|Environment|API Manager Instance|
|REPLICAS|Environment|Number of Replicas to be deployed in Cloudhub 2.0 for the app in the specified environment|
|CORES|Environment|Quantity of cores for each replica in Cloudhub 2.0|
|TARGET|Repository|Private Space in Cloudhub 2.0|
|API_LAYER|Repository|Tag for the API led connectivity layer of the app (Experience/Process/System)|

- Secrets

|Secret|Scope|Description|
|--------|-----|-----------|
|ANYPOINT_PLATFORM_CLIENT_ID|Environment|Client ID of Anypoint Environment for API Manager |
|ANYPOINT_PLATFORM_CLIENT_SECRET|Environment|Client Secret of Anypoint Environement for API Manager|
|SECURE_KEY|Environment|Key for encryption of properties|
|MULE_ORG_ID|Repository|Anypoint Master Org ID|
|MULE_BG_ID|Repository|Anypoint Business Group ID where the app is deployed|
|MULE_CLIENT_ID|Repository|Connected APP client ID to Deploy to Cloudhub|
|MULE_CLIENT_SECRET|Repository|Connected APP client SECRET to Deploy to Cloudhub|
|NEXUS_USERNAME|Repository|Username for Mule Enterprise Nexus Repository - Needed to Run MUnit Coverage|
|NEXUS_PASSWORD|Repository|Password for Mule Enterprise Nexus Repository - Needed to Run MUnit Coverage|


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

```xml
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
```xml
<server>
  <id>MuleRepository</id>
  <username>${env.NEXUS_USERNAME}</username>
  <password>${env.NEXUS_PASSWORD}</password>
</server>
```

## Useful Links
MuleSoft Docs:
- https://docs.mulesoft.com/munit/latest/munit-maven-plugin
- [Set Up Coverage](https://docs.mulesoft.com/munit/latest/munit-maven-plugin#set-up-coverage)
- [Run Tests Using the plugin](https://docs.mulesoft.com/munit/latest/munit-maven-plugin#run-tests-using-the-plugin)
- [How to specify Mule Enterprise Version](https://docs.mulesoft.com/munit/latest/munit-maven-plugin#specify-runtime-product)
- [MUnit Maven Plugin Configuration Reference](https://docs.mulesoft.com/munit/latest/munit-maven-plugin#munit-maven-plugin-configuration-reference)
- [Setup Maven Settings file to use the Mule Enterprise Nexus Repo](https://help.mulesoft.com/s/article/How-to-use-Enterprise-Maven-Repository-credentials-with-settings-xml-and-pom-xml-example)
- [Maven Configuration for Coverage](https://docs.mulesoft.com/munit/latest/coverage-maven-concept)

Blogs and Articles:
- [CI/CD Pipeline with Mulesoft and GitHub Actions - Part 3: MUnit Testing](https://www.prostdev.com/post/part-3-ci-cd-pipeline-with-mulesoft-and-github-actions-munit-testing)
- [CI/CD Pipeline with Mulesoft and GitHub Actions - Part 4: Adding MUnit Coverage](https://www.prostdev.com/post/part-4-ci-cd-pipeline-with-mulesoft-and-github-actions-munit-minimum-coverage-percentage)

## Author

👤 **Gonzalo Marcos**

* Website: [@\MyBlog](https://mulegon.blogspot.com/)
* Twitter: [@\_ShareGon](https://twitter.com/\_ShareGon)
* Github: [@gmansoain](https://github.com/gmansoain)
* LinkedIn: [@gmansoain](https://linkedin.com/in/gmansoain)
