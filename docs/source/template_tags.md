# Template Tags
> Template tags are generating the chat response. 
> These are like web template system similar to JINJA template with following tags Group, Block, Client, Response and Learn Tags

##Group Tags
Group tag are used to group the response to particular topic. Each group can have multiple subgroups group.
If we didn't create block inside the group or group without topic name those will be a generic group.

Sub-group response will be checked and return from the current block, parent block and generic block.

###Syntax
```
{% group topicname %}
{% endgroup %}
```

### Generic Group Sample
```
{% group %}
{% block %}
    {% client %}show (?P<name>.*){% endclient %}
    {% response %}{ %name }{% endresponse %}
{% endblock %}
{% endgroup %}
```

### Named group sample

```
{% group continent %}
    {%  group asia %}
        {% client %}Regex{% endclient %}
        {% response %} Response{% endresponse %}
    {% endgroup %}
    {% group europe %}
        {% client %}Regex{% endclient %}
        {% response %} Response{% endresponse %}
    {% endgroup %}
    ...
{% endgroup %}
```



##Block Tags 
Block holds each client and response details. Every block must have the at-least one client block.

###Syntax
```
{% block %}
{% endblock %}
```
###Sample
```
{% block %}
    {% client %}(what is|(do you remember|tell me) about) (?P<name>.*){% endclient %}
{% endblock %}
```


##Client Tags
Client tag is regex based user input finder. We can have static text or regex to find the inputs.
It also supported python [regex](https://docs.python.org/3/howto/regex.html) to get the input.

###Syntax
```
{% client %}
{% endclient %}
```

### Static text example
```
{% block %}
    {% client %}What is your name{% endclient %}
    {% response %}My name is ChatBot{% endresponse %}
{% endblock %}
```

### Regex text example
```
{% block %}
    {% client %} show (?P<name>.*){% endclient %}
{% endblock %}
```

##Response Tags
Response tag used for user response. 
we can have multiple response, call the function or static response to the user.

###Syntax
```
{% response %}
{% endresponse %}
```

###Static response example
``` 
    {% block %}
    {% client %}I need (.*){% endclient %}
    {% response %}Why do you need %1?{% endresponse %}
    {% response %}Would it really help you to get %1?{% endresponse %}
    {% response %}Are you sure you need %1?{% endresponse %}
    {% endblock %}
```

###Function call example

Here increment_count is a function

```
{% block %}
    {% client %}increment (?P<name>.*){% endclient %}
    {% response %}{% call increment_count: %name %}{% endresponse %}
{% endblock %}
```


##Previous Tags
Previous tag helps to give the proper response or save some input for future response 

###Syntax
```
{% prev %}
{% endprev %}
```

###Sample of saving variable
```
{% block %}
    {% client %}(I am |my name is )?(.*){% endclient %}
    {% prev %}(.*)Can you please tell me your name{% endprev %}
    {% response %}Thank you {name:%2}{% endresponse %}
{% endblock %}
```
###Sample of response
```
{% block %}
    {% client %}(I (am|feel) )?(feeling )?(absolutely )?(.*){% endclient %}
    {% prev %}.*how are you{% endprev %}
    {% response %}{% if {%low %5 %} == fine | {%low %5 %} == good | {%low %5 %} == happy %}  Nice to know that you are %5. What else? {% else %} why you feel %5 {% endif %}{% endresponse %}
{% endblock %}
```

##Learn Tags
Learn tag is dynamic object and description without static object and response.

###Syntax
```
{% learn %}
{% endlearn %}
```
###Sample
```
{% block %}
    {% client %}Remember (?P<object>.*) (?P<description>(is|are) .*){% endclient %}
    {% response %}I will remember that %object %description {% endresponse %}
    {% learn %}
      {% group %}
        {% block %}
            {% client %}(Do you know|tell me) about %object{% endclient %}
            {% response %}%object %description{% endresponse %}
        {% endblock %}
      {% endgroup %}
    {% endlearn %}
{% endblock %}
```

###Chat Sample
```
> remember learn is tag give dynamic response
I will remember that learn is tag give dynamic response
```

```
> tell me about learn
learn is tag give dynamic response
```
