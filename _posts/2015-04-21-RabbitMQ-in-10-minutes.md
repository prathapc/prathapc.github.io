---
layout: post
title: Up and running with RabbitMQ in 10 minutes
comments: true
---

RabbitMQ is robust messaging queue for your applications. You may be thinking of data delivery, non-blocking operations or push notifications. Or you want to use publish / subscribe, asynchronous processing, or work queues. All these are patterns, and they form part of messaging.

RabbitMQ is a messaging broker - an intermediary for messaging. It gives your applications a common platform to send and receive messages, and your messages a safe place to live until received.

These tutorials cover the basics of creating messaging applications using RabbitMQ. You need to have the RabbitMQ server installed to go through the tutorials ‒ please see the [installation guide.](http://www.rabbitmq.com/download.html) In this post i will be using Spring amqp template to push messages into queue with ease from wherever we want.

You have to provide below beans in your apring application context to get rabbit template.
	{% highlight ruby %}
	<beans xmlns:rabbit="http://www.springframework.org/schema/rabbit" >

<bean id="connectionFactory" class="org.springframework.amqp.rabbit.connection.CachingConnectionFactory">
	<constructor-arg value="${rabbitMQ.address}"></constructor-arg>
	<property name="username" value="${rabbitMQ.username}" />
	<property name="password" value="${rabbitMQ.password}" />
</bean>

<rabbit:direct-exchange name="exchange" auto-delete="false" durable="true" id="exchange" >
	<rabbit:bindings>
		<rabbit:binding key="yourQueueName" queue="yourQueueName"></rabbit:binding>
	</rabbit:bindings>
</rabbit:direct-exchange>

<bean id="yourQueueName" class="org.springframework.amqp.core.Queue">
	<constructor-arg value="yourQueueName"></constructor-arg>
	<constructor-arg value="true"></constructor-arg>
	<constructor-arg value="false"></constructor-arg>
	<constructor-arg value="false"></constructor-arg>
</bean>

<bean id="amqpRabbitTemplate" class="org.springframework.amqp.rabbit.core.RabbitTemplate">
	<constructor-arg ref="connectionFactory"></constructor-arg>
	<property name="exchange" value="exchange"></property>
	<property name="routingKey" value="yourQueueName"></property>
	<property name="queue" value="yourQueueName"></property>
</bean>
{% endhighlight %}

Now you can inject your Rabbit template in your component class to send messages to queue as below:

{% highlight ruby %}  
public class Example {

  @Autowired
  private RabbitTemplate amqpControlTableDBTemplate ;
	
  public void sendMessageToQueue(JSONObject jsonObject) {
    
    amqpControlTableDBTemplate.convertAndSend(jsonObject.toString());
    
  }
}
{% endhighlight %} 

Now if you login to your dashboard on browser you can see your queue create with a message init, and you can get/publish your message on browser as well. Now lets move to consumer part where we can consume the message from queue.

{% highlight ruby %}  
ConnectionFactory factory = new ConnectionFactory();
factory.setHost("localhost");
Connection connection = factory.newConnection();
Channel channel = connection.createChannel();
channel.basicQos(1);
channel.queueDeclare(QUEUE_NAME, true, false, false, null);
QueueingConsumer consumer = new QueueingConsumer(channel);
channel.basicConsume(QUEUE_NAME, consumer);
while(true) {
QueueingConsumer.Delivery delivery = consumer.nextDelivery();
int n = channel.queueDeclarePassive(QUEUE_NAME).getMessageCount();
byte[] bs = delivery.getBody();
channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
{% endhighlight %} 

That's all you have to consume your message and to acknowledge. You can see my [github repo](https://github.com/prathapc/RabbitMQ) for different types of consumer queues like named queue, worker queue and pub/sub based queue etc.

Happy Queuing!


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