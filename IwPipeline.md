# Iw.Hub Jenkins Pipeline - Explanation #

The iw.hub repository aims to have mutliple pipelines to achieve different purposes.

This file aims to explain PipeLine 1.

The aim of the pipeline is to have the following functionality:

When a pull request is created for the following branches [Branches to be defined later] we want Pipeline 1 to run with the following properties:

* Run the Linter : The linter is a shell script wrapped in a python interface that will check if all the C++ source files (.cpp and .hpp) are correclty formatted. If at least 1 file is not properly linted, then the pipeline will fail. For the pipeline to succeed in this step the developer requesting the pull request will have to run the script locally and apply the patch file and reattempt the pull request.
* bazel build (jetpack + x86) : Run two parrallel bazel builds for two different architectures the x86 architechture which is the one that will allow the repo to be run on Intel processors. The Jetpack build is the one that will be eventually deployed on the STR.
*  bazel test : Run all the Unit Tests found on the iw.hub repository.

If the pipeline is successful, the Github UI will show that all checks completed successfully with a URL to the Jenkins Server where one could read all the logs about the pipeline execution.
