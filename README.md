# spring-cli-dapr-catalog

This README provides instructions on :

1. Installing the Spring CLI
2. Installing the Dapr Catalog
3. Creating a new project

Additional topics covered are: 

4. Adding code to an existing project
5. Creating your own Project Catalog
6. Creating a new project from a URL


## 1. Installing the Spring CLI

#### To install the Spring CLI, follow these steps to build and get the most recent code.
```shell
git clone https://github.com/spring-projects-experimental/spring-cli $HOME/temp/spring-cli
CURRENT_DIR=$PWD && cd $HOME/temp/spring-cli
./gradlew clean build -x test
alias spring='java -jar $HOME/temp/spring-cli/build/libs/spring-cli-0.0.1-SNAPSHOT.jar'
cd $CURRENT_DIR
```

## 2. Installing the Dapr Catalog

#### Create a new directory and cd into it. Directory name is arbitrary.
```shell
mkdir my-projects
cd my-projects
```

#### Add the Dapr Catalog

The Spring CLI maintains a list of project catalogs that you can use to create new projects.

Executing the command `spring project-catalog list-available` gives:

```shell
┌────┬─────────────┬─────────────────────────────────────────────────────┬───────────────────────────┐
│Name│Description  │URL                                                  │Tags                       │
├────┼─────────────┼─────────────────────────────────────────────────────┼───────────────────────────┤
│dapr│Dapr Catalog.│https://github.com/ciberkleid/spring-cli-dapr-catalog│[java-17, boot-3.1.x, dapr]│
└────┴─────────────┴─────────────────────────────────────────────────────┴───────────────────────────┘
```

Execute the command `spring catalog add dapr`:

```shell
$ spring project-catalog add dapr
Project Catalog 'dapr' added.
```

Running the command `spring project list` will now show the `read`, `write`, and `subscribe` projects.

## 3. Creating a new project

To create a new Dapr project, run the following command:

```shell
$ spring boot new write-app write
Getting project from https://github.com/ciberkleid/dapr-testcontainers?subPath=write-app
Created project in directory 'write-app'
```

This command will create a new directory named `write-app` and copy the contents from the source code repository listed in the message.

Explore the project to verify that the content has been copied, not cloned (there is no .git directory).
```shell
tree -a write-app
cat write-app/pom.xml
```

Additionally, you have the option to specify a package name using `--package-name`.
The application will then be refactored to utilize the provided package name.
Furthermore, there are other available options such as `--artifact-id` and `--description`.
For more detailed information, refer to the help documentation by executing `spring boot new --help`.

#### Create additional new projects
You can create additional new projects. For example:
```shell
$  spring boot new read-app read --package-name com.xkcd.dapr
Getting project from https://github.com/ciberkleid/dapr-testcontainers?subPath=read-app
Refactoring package to com.xkcd.dapr
Created project in directory 'read-app'
```

## 4. Adding code an existing project

Alternatively, you can add code to your existing project using the `spring boot add` command.
It adds code by merging the new code base into your current code base.

Starting from the beginning with `spring boot new`


```shell
$ spring boot new dapr-app read --package-name com.xkcd.dapr
Getting project from https://github.com/ciberkleid/dapr-testcontainers?subPath=read-app
Refactoring package to com.xkcd.dapr
Created project in directory 'dapr-app'

$ cd dapr-app

$ spring boot add write
Getting project with URL https://github.com/ciberkleid/dapr-testcontainers?subPath=write-app
Merging dependency org.testcontainers:junit-jupiter
Merging dependency io.rest-assured:rest-assured
Refactoring code base that is to be merged to package name com.xkcd.dapr
Merging Spring Boot application.yaml file...

Done!
```

Now examine the contents of the `dapr-app` directory

```shell
tree
```
The results from the `tree` command should show the contents of the two projects have been merged, as follows:

```text
.
├── Dockerfile
├── java-app.yaml
├── mvnw
├── mvnw.cmd
├── pom.xml
├── README.md
├── README-write.md
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

## 5. Create your own catalog

To add a custom catalog, create the catalog metadata for the projects you want to add.
For this example, you can use the configuration file: [project-repositories.yml](https://github.com/ciberkleid/spring-cli-dapr-catalog/blob/main/project-repositories.yml)

Register the catalog with the spring CLI
```shell
spring catalog add --name my-dapr --url "https://github.com/ciberkleid/spring-cli-dapr-catalog" --description "Getting started with Dapr" --tags java-17,boot-3.1
```

You should change the URL in the above example to your own location as this catalog was already used in this example.

## 6. Create a new project from a URL

You can also specify a URL instead of a name from a registerd Project Catalog.
Let's use the prefix `dapr-` to distinguish these projects from the ones you created from URLs in step 2.
```shell
spring boot new dapr-write-app --url https://github.com/ciberkleid/dapr-testcontainers?subPath=write-app --package-name com.xkcd.dapr 
spring boot new dapr-read-app --url https://github.com/ciberkleid/dapr-testcontainers?subPath=read-app --package-name com.xkcd.dapr
spring boot new dapr-subscriber-app --url https://github.com/ciberkleid/dapr-testcontainers?subPath=subscriber-app --package-name com.xkcd.dapr
```

You should have three new local projects:
```text
$ ls | grep dapr
dapr-read-app
dapr-subscriber-app
dapr-write-app
```


