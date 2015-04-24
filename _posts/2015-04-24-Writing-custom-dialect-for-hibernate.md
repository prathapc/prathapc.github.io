---
layout: post
title: Writing our own dialect for hibernate
comments: true
---

We were using MySqlDialect in our project regarding object relational mapping (yes we are using hibernate). So far we dont have requirement to write our own dialect untill we encounter below native query in our project 

{% highlight ruby %}
(select group_concat(concat(cl.label,': ', ccf.custom_field_value, ' ')) from complaint_custom_field ccf inner join custom_label cl on ccf.custom_label_id = cl.id where ccf.complaint_id =c.id ) 
{% endhighlight %}

Out of the box MySqlDialect won't support few functionalities in native sql query - 
{% highlight ruby %}
1) coalesce
2) group_concat
{% endhighlight %}

To overcome this we created our own custom dialect class as below:

{% highlight ruby %}
public class CustomeDialect extends MySQLDialect {
	public CustomeDialect(){
	    registerFunction("group_concat", new StandardSQLFunction("group_concat", Hibernate.STRING));
	    registerFunction("coalesce", new StandardSQLFunction("coalesce", Hibernate.STRING));
	    registerHibernateType(Types.LONGVARCHAR, Hibernate.TEXT.getName());

	}
{% endhighlight %}

and and entry in application context will solve the problem forever:

{% highlight ruby %}
<prop key="hibernate.dialect">com.prathap.dao.CustomeDialect</prop>
{% endhighlight %}

Happy coding!


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