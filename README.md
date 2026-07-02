# Cucumber-JVM Starter: Java with Maven

This is the simplest possible setup for Cucumber-JVM using Java with Maven.
There is nothing fancy like a webapp or browser testing. All this does is to
show you how to set up and run Cucumber!

There is a single feature file with one scenario. The scenario has three steps,
one passing, skipped and undefined. See if you can make them all pass!

To write assertions the project comes with [AssertJ](https://assertj.github.io/doc/#assertj-core-assertions-guide)
included. 

## Get the code

Git:

    git clone https://github.com/cucumber/cucumber-jvm-starter-maven-java.git
    cd cucumber-jvm-starter-maven-java

Or [download a zip](https://github.com/cucumber/cucumber-jvm-starter-maven-java/archive/main.zip) file.

## Run the tests

Open a command window and run:

On macOS/Linux:

```shell
./mvnw test
```

On Windows PowerShell:

```powershell
.\mvnw.cmd test
```

This runs Cucumber features using Cucumber's JUnit Platform Engine. The `Suite`
annotation on the `RunCucumberTest` class tells JUnit to kick off Cucumber.

Example output:

```text
Scenario: a few cukes

  Given I have 42 cukes in my belly
  When I wait 1 hour
  Then my belly should growl

org.opentest4j.TestAbortedException: TODO
```

The `When` step intentionally aborts with `TODO`. This is expected and part of
the exercise. Complete the step definition and rerun the tests until all steps
pass.

## Configuration 

The [Cucumber JUnit Platform Engine](https://github.com/cucumber/cucumber-jvm/tree/main/cucumber-junit-platform-engine) uses configuration parameters to know what features to run,
where the glue code lives, what plugins to use, etc. When using JUnit, these
configuration parameters are provided through the `@ConfigurationParameter`
annotation on your test.

For available parameters see: `io.cucumber.junit.platform.engine.Constants`

## Run a subset of Features or Scenarios

Specify a particular scenario by *line*

    @SelectClasspathResource(value = "com/example/project/belly.feature", line = 3)

In case you have multiple feature files or scenarios to run against repeat the
annotation.

You can also specify what to run by *tag*. 

First add a tag to a scenario:

```feature
Feature: Belly

  @Zucchini
  Scenario: a few cukes
```

Then add an annotation to `RunCucumberTest`.

```java
@IncludeTags("Zucchini")
```

Tags can be selected from the CLI using the `groups` and `excludedGroups` parameters. These take a
[JUnit5 Tag Expression](https://junit.org/junit5/docs/current/user-guide/#running-tests-tag-expressions). 
Note: When using JUnit, the `@` is not part of the tag.

```
mvn verify -DexcludedGroups="Haricots" -Dgroups="Zucchini | Gherkin"
```

### Running a single scenario or feature

Maven does not (yet) support selecting single features or scenarios
with JUnit selectors. As a work around the `cucumber.features` property can be
used. Because this property will cause Cucumber to ignore any other selectors
from JUnit it is prudent to only execute the Cucumber engine.

To select the scenario on line 3 of the `belly.feature` file use:

```
./mvnw test -Dsurefire.includeJUnit5Engines=cucumber -Dcucumber.features=src/test/resources/com/example/project/belly.feature:3 
```

The starter project enables the `pretty` plugin by default. Running the tests
displays the executed steps in the console while also generating the HTML report.
