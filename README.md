# Brightspot 4.1 template for Platform.sh

This project provides a starter kit for Brightspot projects hosted on Platform.sh. It is primarily an example, although could be used as the starting point for a real project.

There are many ways in which you can deploy and run Java applications on platform.sh, here we propose a simple topology that allows you to have quick and safe deployments. 

The actual project is located in the `app/` directory with `app/pom.xml` describing your Maven build. 
This app is responsible for the build and compile phase (look at the `build` hook in `app/.platform.app.yaml`). Its `deploy` hook takes the built artifacts and deploys them to the second application which runs a standard tomcat instance.

Platform.sh only redeploys applications that have something changed, as such, every time you `git push` only the app gets redeployed, not tomcat.

And as always, you can create new branches which will create a dedicated ephemeral staging for each branch.

## Brightspot specific configuration

### Databases

We run Brightspot on top of MySQL and Solr. Two relationships are defined in the `.platform.app.yaml` file:

```yaml
    database: "mysql:mysql"
    solr: "solr4:solr"
```

You can find the Brightspot services configurations in `tomcat/context.xml`. We use the `com.psddev.dari.db.AggregateDatabase` storage class with two delegates: `sql` and `solr`.

Solr is configured with the Brightspot `solrconfig.xml` and `schema.xml` located in `.platform/solr/`

### Example project

The code added to the `app/` folder is the [hello-world](http://docs.brightspot.com/cms/developers-guide/tutorials/init-index.html) from the documentation. Feel free to remove it and replace with your own source.

Please note that the `app/pom.xml` has the following extra dependencies:

```xml
<dependency>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>

        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>18.0</version>
        </dependency>

        <dependency>
            <groupId>com.google.code.findbugs</groupId>
            <artifactId>jsr305</artifactId>
            <version>1.3.9</version>
        </dependency>

        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpcore</artifactId>
            <version>4.3</version>
        </dependency>

        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.3.1</version>
        </dependency>
```

This is due to the fact that we package the application with `mvn dependency:go-offline`. These dependencies are not automatically detected and need to be included manually.

### Brightspot mode

Brightspot production mode is disabled. You can enable it in `tomcat/context.xml`.
The `isAutoCreateUser` option is enabled. Feel free to change this in the same file.
The editor default path is `cms/`.

## Starting a new project

To start a new project based on this template, follow these 3 simple steps:

1. Clone this repository locally.  You may optionally remove the `origin` remote or remove the `.git` directory and re-init the project if you want a clean history.
 
2. Create a new project through the Platform.sh user interface and select "Import an existing project" when prompted.

3. Run the provided Git commands to add a Platform.sh remote and push the code to the Platform.sh repository.

That's it!  You now have a working "hello world" level project you can build on.

## Accessing the project

The `/` path should be default trigger a 404. This is normal as the example has no default page.
To access the editor, create a user and login on `/cms/`.
The dari debug tools are accessible at `/_debug/` as we are running in debug mode.