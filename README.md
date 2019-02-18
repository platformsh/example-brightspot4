# Tomcat 8.5 template for Platform.sh

This project provides a starter kit for Tomcat projects hosted on Platform.sh. It is primarily an example, although could be used as the starting point for a real project.

There are many ways in which you can deploy and run Java applications on platform.sh, here we propose a simple topology that allows you to have quick and safe deployments. 

The actual project is located in the `app/` directory with `app/pom.xml` describing your Maven build. 
This app is responsible for the build and compile phase (look at the `build` hook in `app/.platform.app.yaml`). Its `deploy` hook takes the built artifcats and deploys them to the second application which runs a standard tomcat instance.

Platform.sh only redeploys applications that have something changed, as such, every time you `git push` only the app gets redeployed, not tomcat.

And as always, you can create new branches which will create a dedicated ephemeral staging for each branch.

## Starting a new project

To start a new project based on this template, follow these 3 simple steps:

1. Clone this repository locally.  You may optionally remove the `origin` remote or remove the `.git` directory and re-init the project if you want a clean history.
 
2. Create a new project through the Platform.sh user interface and select "Import an existing project" when prompted.

3. Run the provided Git commands to add a Platform.sh remote and push the code to the Platform.sh repository.

That's it!  You now have a working "hello world" level project you can build on.

## Using as a reference

You can also use this repository as a reference for your own projects, and borrow whatever code is needed. The most important parts are the two `.platform.app.yaml` files and the `.platform` directory.
