---
layout: post
title: How to get started with Redis
comments: true
---

Recently i have used Redis extensively at our startup to deal with couple of complex data structures. After moving to redis we saw clear difference in load time of web pages when compared to earlier where we were loading from MySql database. 

I would like to present a simple demo on how to get started with Redis in minutes for beginners.

First of all, Redis is an open source, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets. Redis is written in c.

There are several redis clients available to start with supporting different languages. Here i will be using spring redis template.

{% highlight ruby %}
<bean id="jedisConnFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory" p:use-pool="true" 
p:host-name="${jedis.hostName}" p:port="6379"/>

<bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate" p:connection-factory-ref="jedisConnFactory"/>
{% endhighlight %}

Now we can autowire "redisTemplate" to play with redis as below:

{% highlight ruby %}  
public class Example {

  @Autowired
  private StringRedisTemplate redisTemplate;

  public void addToRedis(String key, String value) {
    redisTemplate.opsForList().leftPush(key, value);
  }
}
{% endhighlight %} 

Similar to list you can perform deal with different data structres like strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.

{% highlight ruby %} 
redisTemplate.opsForSet().add("key", "value1"); //add to set
redisTemplate.opsForHash.entries(String.valueOf("key")); //get
redisTemplate.opsForHash().putAll("key", "value"); //put
{% endhighlight %}

Thats a simple introduction to Redis, we can deal with even complex data structes in real time with amazing fetching speed. We are maintaining all our user sessions as well in redis which gave drastic performance boost for our enterprise application. I will cover session management in other post of mine.


Happy Caching!


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
