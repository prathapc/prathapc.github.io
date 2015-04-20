---
layout: post
title: How to get started with Redis
---

Recently i have used Redis extensively at our startup to deal with couple of complex data structures. After moving to redis we saw sharp load time of web pages when compared to earlier where we were loading from MySql database. 

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

Thats a simple introduction to Redis, we can deal with even complex data structes in real time with amazing fetching speed. We even maintaining all our user sessions in redis which gave drastic performance boost for our enterprise application. I will cover session management in other post of mine.


Bye!