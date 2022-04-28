---
layout: default
---


## [The World of Jenkins: See Video](https://www.youtube.com/watch?v=LFDrDnKPOTg)

[//]: #  There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.


## What is Jenkins? The CI server explained

  Jenkins offers a simple way to set up a continuous integration or continuous delivery (CI/CD) environment for almost any combination of languages and source code repositories using pipelines, as well as automating other routine development tasks. While Jenkins doesn’t eliminate the need to create scripts for individual steps, it does give you a faster and more robust way to integrate your entire chain of build, test, and deployment tools than you can easily build yourself.

“Don’t break the nightly build!” is a cardinal rule in software development shops that post a freshly built daily product version every morning for their testers. Before Jenkins, the best a developer could do to avoid breaking the nightly build was to build and test carefully and successfully on a local machine before committing the code. But that meant testing one’s changes in isolation, without everyone else’s daily commits. There was no firm guarantee that the nightly build would survive one’s commit

## Jenkins automation
Today Jenkins is the leading open-source automation server with some 1,600 plug-ins to support the automation of all kinds of development tasks. The problem Kawaguchi was originally trying to solve, continuous integration and continuous delivery of Java code (i.e. building projects, running tests, doing static code analysis, and deploying) is only one of many processes that people automate with Jenkins. Those 1,600 plug-ins span five areas: platforms, UI, administration, source code management, and, most frequently, build management.

## How Jenkins works
Jenkins is distributed as a WAR archive and as installer packages for the major operating systems, as a Homebrew package, as a Docker image, and as source code. The source code is mostly Java, with a few Groovy, Ruby, and Antlr files.

You can run the Jenkins WAR standalone or as a servlet in a Java application server such as Tomcat. In either case, it produces a web user interface and accepts calls to its REST API.

When you run Jenkins for the first time, it creates an administrative user with a long random password, which you can paste into its initial webpage to unlock the installation.

![ ](https://hackr.io/blog/media/architecture-of-jenkins-min.png)

## Jenkins pipelines
Once you have Jenkins configured, it’s time to create some projects that Jenkins can build for you. While you can use the web UI to create scripts, the current best practice is to create a pipeline script, named Jenkinsfile, and check it into your repository. The screenshot below shows the configuration web form for a multibranch pipeline.

![ ](https://images.idgesg.net/images/article/2017/12/jenkins-multibranch-pipeline-100743391-large.jpg?auto=webp&quality=85,70)


As you can see, branch sources for this kind of pipeline in my basic Jenkins installation can be Git or Subversion repositories, including GitHub. If you need other kinds of repositories or different online repository services, it’s just a matter of adding the appropriate plug-ins and rebooting Jenkins. I tried, but couldn’t think of a source code management system (SCM) that doesn’t already have a Jenkins plug-in listed.

Jenkins pipelines can be declarative or scripted. A declarative pipeline, the simpler of the two, uses Groovy-compatible syntax—and if you want, you can start the file with #!groovy to point your code editor in the right direction. A declarative pipeline starts with a pipeline block, defines an agent, and defines stages that include executable steps, as in the three-stage example below.

![](code.png)

pipeline is the mandatory outer block to invoke the Jenkins pipeline plugin. agent defines where you want to run the pipeline. any says to use any available agent to run the pipeline or stage. A more specific agent might declare a container to use, for example:

agent {
    docker {
        image ‘maven:3-alpine’
        label ‘my-defined-label’
        args  ‘-v /tmp:/tmp’
    }
}
stages contain a sequence of one or more stage directives. In the example above, the three stages are Build, Test, and Deploy.

steps do the actual work. In the example above the steps just printed messages. A more useful build step might look like the following:

pipeline {
    agent any

    stages {
        stage(‘Build’) {
            steps {
                sh ‘make’
                archiveArtifacts artifacts: ‘**/target/*.jar’, fingerprint: true
            }
        }
    }
}
Here we are invoking make from a shell, and then archiving any produced JAR files to the Jenkins archive.

The post section defines actions that will be run at the end of the pipeline run or stage. You can use a number of post-condition blocks within the post section: always, changed, failure, success, unstable, and aborted.

For example, the Jenkinsfile below always runs JUnit after the Test stage, but only sends an email if the pipeline fails.

pipeline {
    agent any
    stages {
        stage(‘Test’) {
            steps {
                sh ‘make check’
            }
        }
    }
    post {
        always {
            junit ‘**/target/*.xml’
        }
        failure {
            mail to: team@example.com, subject: ‘The Pipeline failed :(‘
        }
    }
}
The declarative pipeline can express most of what you need to define pipelines, and is much easier to learn than the scripted pipeline syntax, which is a Groovy-based DSL. The scripted pipeline is in fact a full-blown programming environment.

### I hope this was useful as IoT get deeper and deeper in our lifes.


```
Thank you readers, and wait for next week blog
For Contact e-mail me at ramirez368@hotmail.com

```
