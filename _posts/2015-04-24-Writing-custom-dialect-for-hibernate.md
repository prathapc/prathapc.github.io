---
layout: post
title: Writing our own diablect for hibernate
---

We were using MySqlDialect for in our project regarding object relational mapping (yes we are using hibernate). So far we dont have requirement to write our own dialect untill we encounter below native query in our project 

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

