---
title: Automate Jenkins
author: Van de Voorde Toni
excerpt: Improve your internal development processes by using Jenkins together with the Job DSL Plugin
layout: post
permalink: /2014/09/23/automate-jenkins/
dsq_thread_id:
  - 3828997782
categories:
  - DEVelopment EXPerience
  - General
tags:
  - automate
  - continuous
  - development
  - integration
  - jenkins
  - test
---
<img class="alignright" style="border: 0;" src="http://www.praqma.com/sites/default/files/img/cool-jenkins2x3.png" alt="jenkins is cool" width="214" height="147" />

Jenkins is a powerful <a title="continuous integration" href="http://en.wikipedia.org/wiki/Continuous_integration" target="_blank">continuous integration</a> server which has been around for some time now. I&#8217;ve been personally using it for years and it never let me down.

However, there will come a time where adding/updating/removing jobs will have an impact on your internal processes. Take for example feature branches. Logically you will (and should) test them, so you will start making new jobs for each feature, and once they are done, you will remove them. Sure using the duplicate helps a lot, but it&#8217;s yet another (manual) thing on the todo list of a developer. Or it could even be that you don&#8217;t allow (junior) developers to administrate the jenkins, and thus making it the job of the &#8220;jenkins manager&#8221;.

If you are facing such situation then you will embrace the <a title="Job DSL Plugin" href="https://wiki.jenkins-ci.org/display/JENKINS/Job+DSL+Plugin" target="_blank">Job DSL Plugin</a>.

The <a title="Job DSL Plugin" href="https://wiki.jenkins-ci.org/display/JENKINS/Job+DSL+Plugin" target="_blank">Job DSL Plugin</a> allows you to generate jobs through some simple Groovy DSL scripting. I&#8217;m not going to explain here how everything works, because the <a title="wiki" href="https://github.com/jenkinsci/job-dsl-plugin/wiki" target="_blank">wiki of the plugin</a> does a very good job at that, but instead you&#8217;ll find some DSL scripts which I&#8217;m currently using on a project. I do however suggest reading the <a title="wiki" href="https://github.com/jenkinsci/job-dsl-plugin/wiki" target="_blank">wiki</a> first in order to fully grasp the meaning of the following examples.

**Generate jobs based on subversion branches**

This following DSL will create a job based on the branches it finds on a subversion repository.

<pre class="brush: groovy; title: ; notranslate" title="">svnCommand = "svn list --xml svn://url_path/branches"
def proc = svnCommand.execute()
proc.waitFor()
def xmlOutput = proc.in.text

def lists = new XmlSlurper().parseText(xmlOutput)

def listOfBranches = lists.list.entry.name

println "Start making jobs ..."

// iterate through branches
listOfBranches.each(){
  def branchName = it.text()

  println "- found branch '${branchName}'"

  job {
    name("branch-${branchName}")
    scm {
        svn("svn://url_path/branches/${branchName}")
    }
    triggers {
      scm('H/5 * * * *')
    }
    steps {
      maven("-U clean verify","pom.xml")
    }
  }
}
</pre>

**Generate jobs based on a static list**

If you have several libraries which need to be configured exactly in the same way, you could also make use of a static list.

<pre class="brush: groovy; title: ; notranslate" title="">def systems = ["linux","windows","osx"];

// Configure the jobs for each system
systems.each() {

def system = it

job(type: Maven) {
  name("mylibrary-${system}")
    scm {
      git {
        remote {
          url("git@bitbucket.org:some_account/mylibrary-${system}.git")
        }
      }
    }
    goals("-U clean deploy")
  }
};
</pre>

As you can see you can achieve some very powerful automations with the <a title="Job DSL Plugin" href="https://wiki.jenkins-ci.org/display/JENKINS/Job+DSL+Plugin" target="_blank">Job DSL Plugin</a>.

They already support a lot of plugins but in case the one you use is not (yet) supported it is always possible to configure it through <a title="Configure Blocks" href="https://github.com/jenkinsci/job-dsl-plugin/wiki/The-Configure-Block" target="_blank">Configure Blocks</a>. I had to do that for the <a title="HipChat Plugin" href="https://wiki.jenkins-ci.org/display/JENKINS/HipChat+Plugin" target="_blank">HipChat Plugin</a> which I will explain in detail in a following blog post.

Hope this convinced you to stop creating, editing and removing jobs manually and start doing all that automatically 😉