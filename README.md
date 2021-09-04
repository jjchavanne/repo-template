# repo-template

Used as a template for creating new repositories. Click the create repo button and name the new repository the same as your application. We want to consistently use the same naming everywhere for clarity.
Where to start
Everything below is only here for convenience. It is mostly focused around building images on Compute Engine, so if you have no need for them, or have good reason to do things differently, just update or delete them.
*.json are Hashicorp Packer files used for defining what and where to build build/ is for code and files considered for how to build artifacts like images or packages. Typically Ansible code deploy/ is for code and files considered for deploying infrastructure to Google Cloud. Typically Terraform code
build.auto.pkrvars.json: Contains build settings for when creating Compute Engine Images; and there are some values
which will need to be modified. A key which needs explaining is destination project, which is relevant only for
production deployments and should be set to the production project that the application image needs copying to.

deploy/default.auto.tfvars: Contains default deployment settings that will be used across different environments. I.e. The application name shouldn't be different deploy/prod.tfvars: And the other related files dev.tfvars and test.tfvars, these contain variables that differ across
environment. I.e. Number of instances to run in an instance group. dev or at least test may not require the same amount deploy/vm_monitoring.tf: Has instances of reusable modules that needs the version setting. Unfortunately, the current
version of Terraform doesn't allow for variable interpolation in the source attribute, so look for SET_VERSION in that file and update it to the version of the module you want to use. E.g. v2.5.0 on how Jenkins should build the image and
deploy/Jenkinsfile: Also, build/Jenkinsfile contain a set of instructions
deploy the application. They are generally opinionated around the Compute Engine based code which this is all tailored towards. All the same, there is some useful instructions in them that can be modified to requirements. The standard is to keep build and deployment code in separated directories. Some things need to be updated in both files:
• environment projectGitUrl needs to be set to the Git URL for your project. This will hopefully be removed at deploy/igm.tf: In this file, the Instance Group Manager has been configured with a basic auto-healing HTTP
later date.
healthcheck. You should build your app to have a healthcheck. This will either need updating to meet what your
healthcheck looks like. If you don't have one (you should), remove this configuration.
Jenkins project
In the correct top-level group folder, in Jenkins, create a dedicate application folder named the same as your application. In this folder we want two multi-branch pipeline jobs, one called build and one called deploy. To create these, click New Item in the application folder, name the job build or deploy accordingly and in the Copy From field enter Templates/Build Template or Templates/Deploy Template (do both). Lastly, change Repository HTTPS URL to your new repo's URL.
deploy template
.build template
Your README
Below this line is the beginning of a README for your new repo. Everything above can be deleted but if you're following the templated examples, you may want to copy some of it first and add it to a section in the final README
[YOUR APP NAME HERE]
OVERVIEW
Summary of what the application is for. Consider this a TL;DR
DESCRIPTION
Detailed description of the application, its purpose, why we're using it, how we're using it, key concepts
CONFIGURATION
Where to find any configuration for the application and any key specifics
CI/CD
How the application is built and deployed and any Git (or other) workflows developers and users need to be aware of.
CONTRIBUTING
If you wish to contribute to this codebase, please read CONTRIBUTING.md
