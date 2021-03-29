# Iw.Hub Jenkins Pipeline - Explanation #

The iw.hub repository aims to have mutliple pipelines to achieve different purposes.

This file aims to explain PipeLine 1.

The aim of the pipeline is to have the following functionality:

When a pull request is created for the following branches [Branches to be defined later] we want Pipeline 1 to run with the following properties:

* Run the Linter : The linter is a shell script wrapped in a python interface that will check if all the C++ source files (.cpp and .hpp) are correclty formatted. If at least 1 file is not properly linted, then the pipeline will fail. For the pipeline to succeed in this step the developer requesting the pull request will have to run the script locally and apply the patch file and reattempt the pull request.
* bazel build (jetpack + x86) : Run two parrallel bazel builds for two different architectures the x86 architechture which is the one that will allow the repo to be run on Intel processors. The Jetpack build is the one that will be eventually deployed on the STR.
*  bazel test : Run all the Unit Tests found on the iw.hub repository.

If the pipeline is successful, the Github UI will show that all checks completed successfully with a URL to the Jenkins Server where one could read all the logs about the pipeline execution.

## Pipeline Syntax ## 
This paragraph aims to explain a Jenkins pipeline from a technical point of view.

A Jenkins pipeline is normally defined in a text file which describes the differnet steps of the pipeline. As defined before a CI pipeline is simply an automated succession of tasks.

For projects with a single pipeline the convention is to name that file `JenkinsFile` but that won't be the case in this project due to the presence of many pipelines.

#### A Jenkinsfile can be written using two types of syntax - Declarative and Scripted. ####

According to the official Jenkins Documentation:

```console
Declarative and Scripted Pipelines are constructed fundamentally differently. Declarative Pipeline is a more recent feature of Jenkins Pipeline which:

• Provides richer syntactical features over Scripted Pipeline syntax.

• Is designed to make writing and reading Pipeline code easier.
```

###### Declarative Pipeline ######

Declarative pipeline is a relatively new feature that supports the pipeline as code concept. It makes the pipeline code easier to read and write. This code is written in a Jenkinsfile which can be checked into a source control management system such as Git.

**Structure and syntax of the Declarative pipeline:**
The Agent is where the whole pipeline runs. Example, Docker. The Agent has following parameters:

• any – Which mean the whole pipeline will run on any available agent.

• none – Which mean all the stages under the block will have to declared with agent separately.

• label –  this is just a label for the Jenkins environment

• docker –  this is to run the pipeline in Docker environment.

The Declarative pipeline code will looks like this:

```Jenkins
pipeline {
  agent { label 'node-1' }
  stages {
    stage('Source') {
      steps {
        git 'https://github.com/digitalvarys/jenkins-tutorials.git''
      }
    }
    stage('Compile') {
      tools {
        gradle 'gradle4'
      }
      steps {
        sh 'gradle clean compileJava test'
      }
    }
  }
}
```

###### Scripted Pipeline ######
 The scripted pipeline is a traditional way of writing the code. In this pipeline, the Jenkinsfile is written on the Jenkins UI instance.

 **Structure and syntax of the scripted pipeline:**
 Node Block:

 Node is the part of the Jenkins architecture where Node or agent node will run the part of the workload of the jobs and master node will handle the configuration of the job. So this will be defined in the first place as

```console
node {
}
```
Stage Block:

Stage block can be a single stage or multiple as the task goes. And it may have common stages like

Cloning the code from SCM

Building the project

Running the Unit Test cases

Deploying the code

Other functional and performance tests.

So the stages can be written as mentioned below:

```code
stage {
}
```
So, Together, the scripted pipeline can be written as mentioned below.
```code
node ('node-1') {
  stage('Source') {
    git 'https://github.com/digitalvarys/jenkins-tutorials.git''
  }
  stage('Compile') {
    def gradle_home = tool 'gradle4'
    sh "'${gradle_home}/bin/gradle' clean compileJava test"
  }
}
```

Declarative Pipeline encourages a declarative programming model, whereas Scripted Pipelines follow a more imperative programming model. 

Declarative type imposes limitations to the user with a more strict and pre-defined structure, which would be ideal  for simpler continuous delivery pipelines. 

Scripted type has very few limitations that to with respect to structure and syntax that tend to be defined by Groovy,thus making it ideal for users with more complex requirements. 