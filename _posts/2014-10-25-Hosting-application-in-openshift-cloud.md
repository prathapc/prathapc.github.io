---
layout: post
title: Step by step guide to host application in openshift cloud
comments: true
---
OpenShift Online is Red Hat’s public cloud application development and hosting platform that automates the provisioning, management and scaling of applications so that you can focus on writing the code for your business, startup, or big idea.

Today we are going to host a basic app ([like this](http://akosha-tracker1.rhcloud.com/)) in openshift cloud starting with installation. Follow below steps to setup environment in your mac.

First of all create an Openshift account at [https://developers.openshift.com/](https://developers.openshift.com/) and open your termical to run below commands:

{% highlight ruby %}
step 1: cd ~
step 2: sudo gem install rhc
step 3: rhc setup
step 4: Enter your openshift account details
step 5: generate a token now - yes
step 6: upload ssh key to openshift server - yes
{% endhighlight %} 

You can ssh your application and see the application log from terminal window by getting below url from openshift app dashboard:
{% highlight ruby %}
ssh 53d5492d500446be9e0002bf@fitness-tracker1.rhcloud.com 
rhc tail app_name
{% endhighlight %} 

If you are using windows please follow this [link](https://developers.openshift.com/en/getting-started-windows.html) to setup the same.
 
Now the environment is ready to develop your application. Lets install jboss tools eclipse plugin to develop and host our billion dollar app.

{% highlight ruby %}
Step 1: Search for jBoss tools in eclipse market place & click install
Step 2: Now select JBoss OpenShift Tools plugin and confirm
Step 3: Restart eclipse
Step 4: Create new openshift application
Step 5: Give path to clone your repo so that OpenShift will also setup a private git repository for us and clone the repository to the local machine and select cartridges like MySQL, PostgreSQL, MongoDB. Next, OpenShift will propagate the DNS to the outside world. 
{% endhighlight %}

Finally, the project is imported as a Maven project in the Eclipse workspace.

Now you can see your new application running under your domain created. You can develop your own application on top of it and push your changes to remorte repo to reflect changes in the app.

{% if page.comments %}
<div id="disqus_thread"></div>
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES * * */
    var disqus_shortname = 'wwwprathapchowdarycom';
    
    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
        dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
{% endif %}
