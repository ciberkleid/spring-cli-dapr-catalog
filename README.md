# spring-cli-dapr-catalog

This README explains how to:
1. Install the spring CLI
2. Use the spring CLI to create projects from git repos
3. Use the spring CLI to create projects from catalogs
4. [COMING SOON] Use the spring CLI to add functionality to an existing app

## 1. Install the Spring CLI

#### Get the spring CLI (build from new to get the most recent code):
> Note: Replace `<YOUR_CURRENT_DIR>` with the proper value below.
```
git clone https://github.com/spring-projects-experimental/spring-cli
cd spring-cli
./gradlew clean build -x test
alias spring='java -jar <YOUR_CURRENT_DIR>/build/libs/spring-cli-0.0.1-SNAPSHOT.jar'
cd ..
```

## 2. Use the spring CLI to create projects from git repos

#### Create a new directory and cd into it. Directory name is arbitrary.
```
mkdir my-projects
cd my-projects
```

#### Create a new project from a GitHub repo
The following command will download a copy (not clone) of the source repository and apply some minor refactoring.
Use `spring boot new --help` to see additional arguments you can set.
Note that if you do not set the name, it will use the repository name by default.
```
spring boot new  --name read-app --from "https://github.com/salaboy/dapr-testcontainers?subPath=read-app"  --package-name com.xkcd.dapr
```

#### Explore the copied project
```
tree -a read-app   # or: ls -a read-app
cat read-app/pom.xml
```

## Use the spring CLI to create projects from catalogs

You can add repos to the spring CLI catalog to make it easier to create new projects

To list current spring catalog items and projects, run:
```
spring catalog list
spring project list
```

#### Create catalog metadata and add it to the spring CLI catalog

Create the catalog metadata for the repos you want to add.
See [project-repositories.yml](https://github.com/ciberkleid/spring-cli-dapr-catalog/blob/main/project-repositories.yml)

Register the catalog repository with the spring CLI
```
spring catalog add --name dapr --url "https://github.com/ciberkleid/spring-cli-dapr-catalog" --description "Getting started with Dapr" --tags java-17,boot-3.1
```

List catalogs and projects again.
```
spring catalog list
spring project list
```

#### Create new project from CLI catalog

```
spring boot new pub-sub-subscriber --package-name com.xkcd.dapr --name subscriber-app
spring boot new statestore-and-pub-sub-write --package-name com.xkcd.dapr --name write-app
```

You should now have three local projects:
```
$ ls
read-app	subscriber-app	write-app
```

## [COMING SOON] Use the spring CLI to add functionality to an existing app

The spring CLI enables you to add functionality to an application.
To showcase this functionality for the [sample Dapr application](https://github.com/salaboy/dapr-testcontainers) used here, the code for statestore and pub-sub in the write application needs to be separated so that it is more composable.
This way, one can create a new application from the write-statestore and later *add* pub-sub functionality to the same application using `spring boot add` (see `spring boot add --help` for reference information).

