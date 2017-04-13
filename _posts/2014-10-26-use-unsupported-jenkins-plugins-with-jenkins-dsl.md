---
title: "Use unsupported Jenkins plugins with Jenkins DSL"
layout: post
permalink: /2014/10/26/use-unsupported-jenkins-plugins-with-jenkins-dsl/
---

In a previous post I wrote about how to [Automate Jenkins][1] with the use of the plugin <a title="Job DSL Plugin" href="https://wiki.jenkins-ci.org/display/JENKINS/Job+DSL+Plugin" target="_blank">Job DSL Plugin</a>. If you didn&#8217;t read it, I highly suggest you do that as it will help you understand better what I&#8217;ll be explaining here.

When you start using the <a title="Job DSL Plugin" href="https://wiki.jenkins-ci.org/display/JENKINS/Job+DSL+Plugin" target="_blank">Job DSL Plugin</a> you&#8217;ll probably sooner or later need to configure your job with a plugin that is not yet supported. And by &#8220;not yet supported&#8221; I mean that there aren&#8217;t (yet) <a title="DSL commands" href="https://github.com/jenkinsci/job-dsl-plugin/wiki/Job-DSL-Commands" target="_blank">DSL commands</a> that will generate a job for that specific plugin. Fortunately they provide you with a way to add them &#8216;manually&#8217; through the <a title="The Configure Block" href="https://github.com/jenkinsci/job-dsl-plugin/wiki/The-Configure-Block" target="_blank">Configure Block</a>.

This part is a bit more complex than using simply the <a title="DSL commands" href="https://github.com/jenkinsci/job-dsl-plugin/wiki/Job-DSL-Commands" target="_blank">DSL commands</a>, because you&#8217;ll have to understand how it works. Now you did notice I wrote &#8220;a bit&#8221; &#8230; that&#8217;s because it seems complex, but in fact it isn&#8217;t. The only thing you need to know is that the plugin will, with the <a title="DSL commands" href="https://github.com/jenkinsci/job-dsl-plugin/wiki/Job-DSL-Commands" target="_blank">DSL commands</a>, generate the *config.xml* of your job containing the full configuration of the job.

To have an idea, this is the *config.xml* of an empty job


```xml
<?xml version='1.0' encoding='UTF-8'?>
<project>
  <actions/>
  <description></description>
  <keepDependencies>false</keepDependencies>
  <properties />
  <scm class="hudson.scm.NullSCM"/>
  <canRoam>true</canRoam>
  <disabled>false</disabled>
  <blockBuildWhenDownstreamBuilding>false</blockBuildWhenDownstreamBuilding>
  <blockBuildWhenUpstreamBuilding>false</blockBuildWhenUpstreamBuilding>
  <triggers/>
  <concurrentBuild>false</concurrentBuild>
  <builders/>
  <publishers/>
  <buildWrappers/>
</project>
```

Let&#8217;s see an example of a basic <a title="DSL command" href="https://github.com/jenkinsci/job-dsl-plugin/wiki/Job-DSL-Commands" target="_blank">DSL command</a> and the corresponding *config.xml*.

```groovy
job {
    name 'Test Job'
    description 'A Test Job'
}
```

```xml
<project></project>
  ...
  <description>A Test Job</description>
  ...
</project>
```

So you see that every <a title="DSL command" href="https://github.com/jenkinsci/job-dsl-plugin/wiki/Job-DSL-Commands" target="_blank">DSL command</a> will generate some part in the *config.xml*.

Knowing this you&#8217;ll understand that we will have to study the *config.xml* of an existing job to see how the &#8220;unsupported&#8221; plugin is configured in the *config.xml*.

Let&#8217;s make it a bit more fun by integrating the <a title="HipChat Plugin" href="https://wiki.jenkins-ci.org/display/JENKINS/HipChat+Plugin" target="_blank">HipChat Plugin</a>. I created a simple job in jenkins and opened the *config.xml* file. (I assume you know how to install and configure the plugin in Jenkins)

```xml
<?xml version='1.0' encoding='UTF-8'?>
<project></project>
  <actions/>
  <description></description>
  <keepDependencies>false</keepDependencies>
  <properties>
    <jenkins.plugins.hipchat.HipChatNotifier_-HipChatJobProperty plugin="hipchat@0.1.4"></jenkins.plugins.hipchat.HipChatNotifier_-HipChatJobProperty>
      <room></room></room>
      <startNotification>false</startNotification>
    </jenkins.plugins.hipchat.HipChatNotifier_-HipChatJobProperty>
  </properties>
  <scm class="hudson.scm.NullSCM"/>
  <canRoam>true</canRoam>
  <disabled>false</disabled>
  <blockBuildWhenDownstreamBuilding>false</blockBuildWhenDownstreamBuilding>
  <blockBuildWhenUpstreamBuilding>false</blockBuildWhenUpstreamBuilding>
  <triggers/>
  <concurrentBuild>false</concurrentBuild>
  <builders/>
  <publishers>
    <jenkins.plugins.hipchat.HipChatNotifier plugin="hipchat@0.1.4"></jenkins.plugins.hipchat.HipChatNotifier>
      <jenkinsUrl></jenkinsUrl>http://jenkins/</jenkinsUrl>
      <authToken>ABCDEFGHIJKLMNOPQRSTUVWXYZ</authToken>
      <room>76124</room>
    </jenkins.plugins.hipchat.HipChatNotifier>
  </publishers>
  <buildWrappers/>
</project>
```

> The values in the publisher section are being copied from the jenkins administration. That&#8217;s a bit annoying because it means you&#8217;ll have to expose that in the DSL scripting. At the moment of this writing, I didn&#8217;t find a way to configure that as variables.

Looking at the config.xml, we see that 2 nodes were modified, the *properties* and the *publishers* node. Both are children from the root *project* node. With the Configure Block we can obtain the *XML Node* to manipulate the DOM.

Get hold on the *project* node:

```groovy
job {
  configure { project ->
    // project represents the node <project>
  }
}
```

Now that we can manipulate the project node, let&#8217;s add the properties node:

```groovy
job {
  configure { project ->
      
    project / 'properties' << 'jenkins.plugins.hipchat.HipChatNotifier_-HipChatJobProperty' {
      room ''
      startNotification false
    }

  }
}
```

What we did here is tell the parser to append (<<) the block &#8216;jenkins.plugins.hipchat.HipChatNotifier_-HipChatJobProperty&#8217; to the node project/properties. And finally in the block we simply enumerate the parameters as *key*[space]*value* as you can see them in the config.xml.

**Hint 1:** Do not specify the plugin version *plugin=&#8221;hipchat@0.1.4&#8243;* otherwise it doesn&#8217;t work.  
**Hint 2:** I append the properties (and below the publishers), because there will/can be others configured through other DSL blocks.

Let&#8217;s do the same now for the publishers part:

```groovy
job {
  configure { project ->
      
    project / 'publishers' << 'jenkins.plugins.hipchat.HipChatNotifier' {
      jenkinsUrl 'http://jenkins/'
      authToken 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
      room '76124'
    }
  }
}
```

As with the properties, we tell the parser to append (<<) &#8216;jenkins.plugins.hipchat.HipChatNotifier&#8217; (without the plugin version) and enumerate the parameters.

Following is the full DSL for adding HipChat Plugin support:

```groovy
job {
  name "Job with HipChat"
  
  configure { project ->
      
    project / 'properties' << 'jenkins.plugins.hipchat.HipChatNotifier_-HipChatJobProperty' {
      room ''
      startNotification false
    }

    project / 'publishers' << 'jenkins.plugins.hipchat.HipChatNotifier' {
      jenkinsUrl 'http://jenkins/'
      authToken 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
      room '76124'
    }
  }
}
```

Once you grasp the Configure block, you&#8217;ll be able to generate any job you want. The example below uses the configure block to add a missing functionality in an existing predefined GIT DSL:

```groovy
job {
  scm {
    git {
      remote { 
        url("")
      }
      branch("refs/heads/${branch}")
      configure { node -> //the GitSCM node is passed in
        
        // Add the CleanBeforeCheckout functionality
        node / 'extensions' << 'hudson.plugins.git.extensions.impl.CleanBeforeCheckout'  {
        }
        
        // Add the BitbucketWeb
        node / browser (class: 'hudson.plugins.git.browser.BitbucketWeb') {
          url 'https://bitbucket.org/my-account/my-project/'
        }
      }
    }
  }
}
```

A handy tool to play with (or test) the generation of your DSL is http://job-dsl.herokuapp.com/. It will prevent your from constantly running your DSL and open the config.xml from your jenkins to see if the xml is generated correctly!

Although the Configure block is really awesome, it doesn&#8217;t beat the predefined DSL commands, so if you have the time I suggest to contribute to the project by making it a predefined DSL :) https://github.com/jenkinsci/job-dsl-plugin/blob/master/CONTRIBUTING.md

If you have some other great Configure Block example, share them in the comments :)

 [1]: http://www.devexp.eu/2014/09/23/automate-jenkins/ "Automate Jenkins"