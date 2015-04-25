---
layout: post
title: Deploying an app in google app engine
comments: true
---

Google App Engine lets you build and run applications on Google’s infrastructure. App Engine applications are easy to create, easy to maintain, and easy to scale as your traffic and data storage needs change. With App Engine, there are no servers for you to maintain. You simply upload your application and it’s ready to go.

In this tutorial, i will show how i deployed a twitter4j app ([you can see my app here](http://fresh-fusion-704.appspot.com/))

To create a project you need app or project ID to deploy your app to production app engine. create new project [here](https://console.developers.google.com/)

Change to a directory where you want to build your project and invoke Maven as follows:

{% highlight ruby %}
mvn archetype:generate -Dappengine-version=1.9.19 -Dapplication-id=your-app-id -Dfilter=com.google.appengine.archetypes:
{% endhighlight %}


once app engine app skeleton and appropriate jars downloaded change direcotry to newly created app folder and issue below command:
{% highlight ruby %}
mvn clean install
{% endhighlight %}

Now basic app was created with a proper folder structure. Create an index.jsp at src/main/webapp location and some content to it.

Now lets push these changes to our production app as below:
{% highlight ruby %}
mvn clean install
mvn appengine:devserver
{% endhighlight %}


Ok, all good and application is running successfully at localhost:8080. Its time to upload our app to appengine
{% highlight ruby %}
mvn appengine:update
{% endhighlight %}

Hurray!! your app is live at your_app_id.appspot.com

You can link your git repo to app engine, so that you can simply push your changes to git repo which inturn updates in production environment.

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