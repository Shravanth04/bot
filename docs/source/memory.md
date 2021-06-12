# Memory

> Memory store the information/value for any user specific attributes, for example storing users names, age, other details or any other information user want the bot to remember.
>
> Helps in setting up remainders and knowledge-base. This can be done in template response tag or python call back functions through session objects.

SET MEMORY
------------------------------------------------------------------------------------------------------------------------

## Set memory with-in template tag

Setting memory with-in template should be done between `{% response %}` and `{% endresponse %}` tags.

### Set memory in copy mode

setting up memory in copy mode stores value for given variable name for current user and puts the value as part of the
text in place of set memory tag or can be used for any other purpose like with-in condition or callbacks.

#### Syntax

```
{ vairable_name : value }
```

#### Example

(I) constant value set string value `Arun` in variable `Name`.

```
{ Name : Arun }
```

(II) dynamic value (set from client match group)
set `full_name` entity from client message match group as `Name`.

```
{ Name : %full_name }
```

or set first matching group from client message as `Name`

```
{ Name : %1 }
```

(III) dynamic variable name with constant value set `object` entity from client message group as variable name with
constant value `Hot`.

```
{ %object: Hot }
```   

(IV) dynamic variable name and value

set `object` entity from client message group as variable name and `property` entity from client message group.

```
{ %object : %property }
```

##### Full block example

```
{% group %}
    {% block %}
        {% client %}remember (?P<name>.*) is (?P<value>.*){% endclient %}
        {% response %}I will remember %name is { %name : %value }{% endresponse %}
    {% endblock %}
{% endgroup %}
```

Chat Sample

```
> remember sun is red hot star in our solar system
I will remember sun is red hot star in our solar system
```

In the above example in memory for current user session variable `sun` will be set with value
`is red hot star in our solar system`.

## Set memory in think mode

setting up memory in copy mode stores value for given variable name for current user and returns nothing. Hence, when it
is used with-in response tag it'll be replaced by empty string

#### Syntax

```
{! vairable_name : value }
```

##### Full block example

```
{% group %}
    {% block %}
        {% client %}My age is (?P<age>\d+){% endclient %}
        {% response %}
            I will remember your age {! age: %age }
        { % endresponse %}
    {% endblock %}
{% endgroup %}
```

Chat Sample

```
> My age is 29
I will remember your age
```

Age will be store for the current user in memory and not included in the response, as you can see from the above
example.

**Set memory in python**
   Session is dictionary kind of storage. Keeps the attributes and values for each user session. 
   we can add value to it similar way of dictionary.

#### Syntax

`session[vairable_name] = value`

GET MEMORY
---------------------------------------------------------------------------------------------------------------------

## Get memory value with-in template tag

### Full block sample

```
{% block %}
    {% client %}Do you (know|remember) my name{% endclient %}
    {% response %}{% if {name} %}Yes I do {name}{% else %}No,{% chat what is my name %}{% endif %}{% endresponse %}
{% endblock %}
```

Chat Sample

```
> do you name my name? 
> Yes I do Arun
```

## Set and Get memory value in python session 

#### Syntax set memory
```
 session.memory[variable_name] = value
```

#### Syntax set memory

```
session.memory.get(variable_name)
```

### Full block sample
Increment memory sample [python mode](https://github.com/ahmadfaizalbh/Chatbot/blob/master/examples/get_set_memory_example.py#L5-L13)
```
{% block %}
    {% client %}increment (?P<name>.*){% endclient %}
    {% response %}{% call increment_count: %name %}{% endresponse %}
{% endblock %}
```

Chat sample

```
> increment mango

count 1
```

```
> increment mango

count 2
```

```
> show mango  
2
```




