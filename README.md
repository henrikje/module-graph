[![Pre Merge Checks](https://github.com/iurysza/module-graph/workflows/Pre%20Merge%20Checks/badge.svg)](https://github.com/iurysza/module-graph/actions?query=workflow%3A%22Pre+Merge+Checks%22) [![License](https://img.shields.io/github/license/cortinico/kotlin-android-template.svg)](LICENSE) ![Language](https://img.shields.io/github/languages/top/cortinico/kotlin-android-template?color=blue&logo=kotlin)

## Introducing the Gradle Module Graph Plugin! 🌟

This plugin generates a [Mermaid](https://github.com/mermaid-js/mermaid) graph for your Gradle
project, providing a visual representation of your project's module relationships. By illustrating
the connections between modules, it makes it easier to grasp your project's structure and
interdependencies.

> A diagram about the current system is only useful if it's generated. If it is produced by hand it
> documents the author's belief, not the system. Still, important, but not an input for decision
> making. Development is primarily decision making. Enable it through custom
> tools. [source](https://twitter.com/girba/status/1628326848256962561)

You can read more about the background story of this
plugin [here](https://iurysouza.dev/automating-project-architecture-graphs/).

## Features

- Generate a Mermaid dependency graph of the modules in your Gradle project
- Automatically append the generated graph to your project's README file

## Getting Started

You'll just need to add it to your project's **root** `build.gradle` or `build.gradle.kts` file.

<details>
  <summary><b>build.gradle (Groovy DSL)</b></summary>

### Using the [plugins DSL](https://docs.gradle.org/current/userguide/plugins.html#sec:plugins_block)

```groovy
plugins {
    id "dev.iurysouza.modulegraph" version "0.3.0"
}
```

### Using [legacy plugin application](https://docs.gradle.org/current/userguide/plugins.html#sec:old_plugin_application)

```groovy
buildscript {
    repositories {
        maven {
            url "https://plugins.gradle.org/m2/"
        }
    }
    dependencies {
        classpath "dev.iurysouza:modulegraph:0.3.0"
    }
}

apply plugin: "dev.iurysouza.modulegraph"
```

### Configuring the plugin

```groovy
  moduleGraphConfig {
    readmePath = "README.md"
    heading = '### Dependency Diagram'
    theme = Theme.NEUTRAL // optional
    orientation = Orientation.LEFT_TO_RIGHT // optional
    linkText = LinkText.CONFIGURATION // optional
    createReadmeIfMissing = true // optional
}
```

</details>

<p></p>

<details open>
<summary><b>build.gradle.kts (Kotlin DSL)</b></summary>

<p></p>

#### Using the [plugins DSL](https://docs.gradle.org/current/userguide/plugins.html#sec:plugins_block)

```kotlin
plugins {
    id("dev.iurysouza.modulegraph") version "0.3.0"
}
```

#### Using [legacy plugin application](https://docs.gradle.org/current/userguide/plugins.html#sec:old_plugin_application)

```kotlin
buildscript {
    repositories {
        maven {
            url = uri("https://plugins.gradle.org/m2/")
        }
    }
    dependencies {
        classpath("dev.iurysouza:modulegraph:0.3.0")
    }
}

apply(plugin = "dev.iurysouza:modulegraph")
```

### Configuring the plugin

```kotlin
moduleGraphConfig {
    readmePath.set("$projectDir/README.md")
    heading.set("### Dependency Diagram")
    theme.set(Theme.NEUTRAL) // optional
    orientation.set(Orientation.LEFT_TO_RIGHT) //optional
    linkText.set(LinkText.CONFIGURATION) // optional
    createReadmeIfMissing.set(true) // optional
}
```

</details>

## Configuration

To configure the Gradle Module Dependency Graph Plugin, you can set the following options:

- **readmePath**: The path of the file where the dependency graph will be appended.
- **heading**: The heading where the dependency graph will be appended.

Optional settings:

- **theme**: The [mermaid theme](https://mermaid.js.org/config/theming.html) to be used for styling
  the graph. Default is `NEUTRAL`.
- **orientation**:
  The [orientation](https://mermaid.js.org/syntax/flowchart.html#flowchart-orientation) that the
  flowchart will have. Default is `LEFT_TO_RIGHT`.
- **linkText**:
  Whether to add information as text on links in graph. Available values:
  - `NONE`: No text added. (Default.)
  - `CONFIGURATION`: The name of the configuration which the dependency belongs to (e.g. "implementation", "compileOnly", "jsMain").
- **createReadmeIfMissing**:
  Create the specified `readmePath` if it does not exist. Default is `false`.

## Usage

Make sure you have a heading in your `README` with the same format as the one you set in the configuration, if
not, the plugin will append it with the graph to the end of the file.

After that, just run the following command:

```sh
./gradlew createModuleGraph
```

Now, just look for the generated graph in your project's README file.

### Example Diagram

You can expect the plugin to generate this kind of diagram after running the plugin:

```mermaid
%%{
  init: {
    'theme': 'neutral'
  }
}%%

graph LR
  subgraph app
    main
    playground
  end
  subgraph core
    common
    design-system
    footballdata
    reddit
  end
  subgraph features
    match-day
    match-thread
  end
  main --> match-thread
  main --> match-day
  main --> footballdata
  main --> reddit
  main --> design-system
  main --> common
  playground --> match-thread
  playground --> match-day
  playground --> footballdata
  playground --> reddit
  playground --> design-system
  playground --> common
  footballdata --> common
  reddit --> common
  match-day --> common
  match-day --> footballdata
  match-day --> design-system
  match-day --> reddit
  match-thread --> common
  match-thread --> footballdata
  match-thread --> design-system
  match-thread --> reddit
```

## Contributing 🤝

Feel free to open an issue or submit a pull request for any bugs/improvements.

## License 📄

This template is licensed under the MIT License - see the [License](License) file for details.
