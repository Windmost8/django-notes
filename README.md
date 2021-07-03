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
