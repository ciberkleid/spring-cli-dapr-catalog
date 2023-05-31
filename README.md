# spring-cli-dapr-catalog

This README explains how to:
1. Install the spring CLI
2. Create/merge projects from git URLs
3. Register a catalog and create/merge projects from the catalog

## 1. Install the Spring CLI

#### Get the spring CLI (build from new to get the most recent code):
```shell
git clone https://github.com/spring-projects-experimental/spring-cli $HOME/temp/spring-cli
CURRENT_DIR=$PWD && cd $HOME/temp/spring-cli
./gradlew clean build -x test
alias spring='java -jar $HOME/temp/spring-cli/build/libs/spring-cli-0.0.1-SNAPSHOT.jar'
cd $CURRENT_DIR
```

## 2. Create/merge projects from git URLs

#### Create a new directory and cd into it. Directory name is arbitrary.
```shell
mkdir my-projects
cd my-projects
```

#### Create a new project
The following command will download a copy (not clone) of the source repository and apply some minor refactoring.
Use `spring boot new --help` to see additional arguments you can set.
Note that if you do not set a name for the new app, the CLI will use the source repository name by default.
```shell
spring boot new  --name write-app --from "https://github.com/ciberkleid/dapr-testcontainers?subPath=write-app"  --package-name com.xkcd.dapr
```

Explore the project to verify that the content has been copied, not cloned (there is no .git directory).
```shell
tree -a write-app   # or: ls -a write-app
cat write-app/pom.xml
```

#### Create additional new projects
You can create additional new projects. For example:
```shell
spring boot new  --name read-app --from "https://github.com/ciberkleid/dapr-testcontainers?subPath=read-app"  --package-name com.xkcd.dapr
ls
```

#### Merge projects

Alternatively, you can merge projects using the `spring boot add` command.
```shell
spring boot new  --name read-and-write-app --from "https://github.com/ciberkleid/dapr-testcontainers?subPath=write-app"  --package-name com.xkcd.dapr
cd read-and-write-app
spring boot add --from "https://github.com/ciberkleid/dapr-testcontainers?subPath=read-app"
tree
cd ..
```

The results from the `tree` command should show the contents of the two projects have been merged, as follows:
```text
.
├── Dockerfile
├── README-dapr-testcontainers.md
├── README.md
├── java-app.yaml
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── xkcd
    │   │           └── dapr
    │   │               ├── Application.java
    │   │               ├── read
    │   │               │   ├── MyValues.java
    │   │               │   └── ReadController.java
    │   │               └── write
    │   │                   ├── MyValues.java
    │   │                   └── WriteController.java
    │   └── resources
    │       └── application.properties
    └── test
        ├── java
        │   └── com
        │       └── xkcd
        │           └── dapr
        │               ├── read
        │               │   ├── CommonContainers.java
        │               │   ├── ReadControllerTests.java
        │               │   └── TestApp.java
        │               └── write
        │                   ├── CommonContainers.java
        │                   ├── TestApp.java
        │                   └── WriteControllerTests.java
        └── resources
            ├── application.properties
            ├── components
            │   ├── pubsub.yaml
            │   └── statestore.yaml
            └── logback-test.xml
```

## 3. Register a catalog and create/merge projects from the catalog

To make it easier to find/copy/merge source projects, you can register them in a spring CLI catalog.

First, take a look at the default/current catalog(s):
```shell
spring catalog list
```

Take a look at the projects in the default/current projects:
```shell
spring project list
```

#### Create catalog metadata and add it to the spring CLI catalog

To add a custom catalog, create the catalog metadata for the projects you want to add.
For this example, you can use the configuration file: [project-repositories.yml](https://github.com/ciberkleid/spring-cli-dapr-catalog/blob/main/project-repositories.yml)

Register the catalog with the spring CLI
```shell
spring catalog add --name dapr --url "https://github.com/ciberkleid/spring-cli-dapr-catalog" --description "Getting started with Dapr" --tags java-17,boot-3.1
```

List catalogs and projects again.
Notice that the `dapr` catalog is now listed, along with three new projects:
```shell
spring catalog list
spring project list
```

#### Create new projects

Now you can more easily create new projects, without the need to remember the URL for the source code.
Let's use the prefix `dapr-` to distinguish these projects from the ones you created from URLs in step 2.
```shell
spring boot new write --package-name com.xkcd.dapr --name dapr-write-app
spring boot new read --package-name com.xkcd.dapr --name dapr-read-app
spring boot new subscribe --package-name com.xkcd.dapr --name dapr-subscriber-app
```

You should have three new local projects:
```text
$ ls | grep dapr
dapr-read-app
dapr-subscriber-app
dapr-write-app
```

### Merge projects

Merge projects using the `spring boot add` command.
```shell
spring boot new write --package-name com.xkcd.dapr --name dapr-write-and-subscribe-app
cd dapr-write-and-subscribe-app
spring boot add subscribe
tree
cd ..
```

You should see the contents of the two projects have been merged.

