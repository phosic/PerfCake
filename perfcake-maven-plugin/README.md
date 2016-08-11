Maven plugin for PerfCake
=========================
Maven plugin which allows to run [PerfCake](http://www.perfcake.org) scenario within Maven build. This makes perfomance test
automation more easy and encourage you to run performance tests on a regular basis, e.g. in within your favourite CI server.
Running performance test on a regular basis allows you to spot performance drops very soon and thus make much more easy to
identify, which commit has caused performance regression.

Configuration
---
Currently plugin has only one goal `scenario-run`, which runs specified PerfCake scenario. By default, this goal is executed
in `integration-test` phase (i.e. assumes, that you deploy/start your application in `pre-integration-test` phase and 
shut it down in `post-integration-test` phase). 

The only mandatory parameter is `<scenario>`, which specifies the name of the scenario to be run. 
Optionally, you can also specify `scenarios-dir`, `messages-dir` and `plugins-dir`, which specify paths to
directory containing scenarios, messages and plugins, respectively. If you don't setup these parameters, plugin assumes,
that appropriate directories (`scenarios`, `messages`, `plugins`) are in `src/test/resources/perfcake`. This can be switched to
`src/main/resources/perfcake` by configuring `use-test-resources` to `false`. If any of these directories does not
exists, plugin will use `src/test/resources` (or `src/main/resources`) as a fallback value for missing parameter.

You can also specify PerfCake version which should be used for scenario execution. It can be set up by adding
`<perfcake-version>` element into plugin configuration.
Appropriate PerfCake jar file is obtained via Maven, so you can use any PerfCake version which is available in your Maven repository.
If the version is not specified or appropriate artifact cannot be resolved, PerfCake on the class path is used - if not changed by 
other set up, there should be PerfCake which this plugin depends on.

PerfCake logging level can be configured with `<log-level>` tag and additional properties file can be specified by `<properties-file>`.

Example configuration
---

```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.perfcake.maven</groupId>
        <artifactId>perfcake-maven-plugin</artifactId>
        <version>0.1-SNAPSHOT</version>
        <configuration>
          <scenario>my_perfcake_scenario</scenario>
          <perfcake-version>7.0</perfcake-version>
        </configuration>
        <executions>
          <execution>
            <id>perfcake-scenario-run</id>
            <goals>
              <goal>scenario-run</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```