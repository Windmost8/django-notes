# django-notes

# Models
## Creating Models
+ models.Model is class, by django.all model classes  inhereit models.model. W
+ When we create the models, they becomes tables in the database. 
+ Every field is a coloumn. 
+ every object is a row. 



## Model Fields
+ For any model we create, django adds a field called ID, and this ID is the primary key. Therfore its unique and never repeated, and its used whenever for creating relationships between two models. ex... 
Product.objects.get(pk=1)
Category.objects.get(id=1)

+ If a model wants to have a field from another model, then a foreign key is needed to connect them. The primary is the one that 

+ default = when a new object is created, and this is not assigned a value, default is what will be assigned. ex
...
description = models.CharField(max_length=250, blank=True, default='generic description')

+ null = True means that a field can be empty. ex
...
category = models.ForeignKey("Category", on_delete=models.SET_NULL, null=True, default=1)

+ . CASCADE, RESTRICT, SET_NULL. cascade would delete all associated items with the foreign key, restrict would stop you from deleting a foreign key if there WAS something associated with it, and set null removes the whole thing.

# Query & Database
+ Product.objects.all() returns all items inside Product
Product.objects.get(name="billy") returns Product item with the name billy. Note: get returns 1 result, if you want more than one, use filter()
p = Product.objects.get(name="billy") and then p.save() will save it to the database. Note. a typo will not result in an error, but you will not get your desired result.

CHECK DJANGO DOCUMENTATION https://docs.djangoproject.com/en/3.2/topics/db/queries/

# ManyToMany
+ You can "add" via the same line, or do it seperately.
o1.products.add
o1.products.add(Product.objects.get(name="åsa"), Product.objects.get(name="bob marley"))

+ You cannot do a .attribute to a Class/Model, but rather to an object. So for example 
c = Category() and then c.name
You cannot do Category.name

+ Realize closely what is the object, and what is the Class. For your own sake, maybe name objects more clearer. 

+ User is an inbuilt django Model, that is not shown but is there. You can check the fields in the admin page. 

+ https://docs.djangoproject.com/en/3.2/topics/db/queries/#saving-foreignkey-and-manytomanyfield-fields

+ .create() saves auatomatically instead of having to do .save()

+ Foreignkeys and manttomany fields cannot be assinged values, you must either .get() or .create(). The rest can, on the same line too.

# Query

+ Product.objects.create(name="sameh", price=4, category=Category.objects.create(name="pop"))

+ There are two ways to filter, one standard, and one shorthand. Both achieve the same thing in this example;
Product.objects.filter(category=Category.objects.get(name="Reggae")) - normal
Product.objects.filter(category__name="Reggae") - shorthand

+ You can use a greater than or lesser than operator in filter like this
Product.objects.filter(price__gte=1000)
Product.objects.filter(price__lte=10)
NOTE: that the position of the operator in the code doesnt neccessarily mean anything. Product.objects.filter(category__name="Reggae"), in this example, name is a field, not an operator.

+ Here are a list of operators useable in filtering after __
```
exact
iexact
contains
icontains
in
gt
gte
lt
lte
startswith
istartswith
endswith
iendswith
range

date
year
iso_year
month
day
week
week_day
iso_week_day
quarter
time
hour
minute
second

isnull
regex
iregex
```
+ Here is an exmaple of filter chaining, or with exclude;
```Product.objects.filter(category__name="Rock", price__gt=170).exclude(description="generic description")```

+ You can also filter by words, using "contains". An example would be
```Product.objects.filter(description__contains="people")```

+ You can also index like so;
```Product.objects.all()[:3]``` Note this is the first 3, not 2. It starts with 1. It is also not possible to negative index, so getting the last one could be tricky.

+ You can filter by jumping through fields like this. This takes the products field from the order model, then the category from Product, and name.
```Order.objects.filter(products__category__name="Rock")```

+ In order to delete, you simple use .delete()
ex;

```
p = Product.objects.last()
p.delete()

or

Product.objects.get(name="iron maiden").delete()
```

# Aggregation

+  Sum, average, min, and max. Ignore the in and out, its just from the shell. This needs imports, Import Max, Avg etc..
```Product.objects.all().aggregate(Max("price"))
Out[22]: {'price__max': Decimal('2000')}

In [23]: Product.objects.all().aggregate(Min("price"))
Out[23]: {'price__min': Decimal('1')}

In [24]: Product.objects.all().aggregate(Avg("price"))
Out[24]: {'price__avg': Decimal('309.75')}

In [25]: Product.objects.all().aggregate(Sum("price"))
Out[25]: {'price__sum': Decimal('2478')}
```

+ If you want to calculate the total sum of an order for example. First you index the order and then bring all the products in that order to aggreagte, so;
```
o1 = Order.objects.get(id=1)
then
o1.products.all().aggregate(Sum("price"))
```

# General objects
+ How to make a class, and objects creating lists, and joining them in the end to create a string.
```
class Animal:
    def __init__(self, name, sound):
        self.name = name
        self.sound = sound 

    def makeSound(self):
        print(self.name, self.sound)

a1 = Animal('dog', 'bark')
a2 = Animal('bird', 'tweet')
a2.sound = 'chirp'
a3 = Animal("cat", "meow")
animalList = [a1, a2, a3]
nameList = [animal.name for animal in animalList]
print(", ".join(nameList))
```

# Views & Templates
+ 
```
{% if latest_question_list %}
    <ul>
    {% for q in latest_question_list %}
        <li><a href="/polls?mickey={{ q.id }}">{{ q.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <h1>there are NO questions</h1>
{% endif %}
<h6>browser is {{ userAgent }}</h6>
<h6>host is {{ request.headers.Host }}</h6>
{% if chosenQuestion %}
    <h6>params are {{ chosenQuestion }}</h6>
{% endif %}
```
```
def index(request):
```
