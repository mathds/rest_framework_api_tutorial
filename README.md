# Django Rest Framework Tutorial

# Setting up a new environnement

We create a new virtual environnement

```python
virtualenv env
source env/bin/activate
```

# Getting started

Install rest framework :

```python
pip install djangorestframework
```


New Django project : 

```python
django-admin.py startproject tutorial
cd tutorial
```


New app, simple Web API : 

```python
python manage.py startapp snippets
```

Add the snippets app and the rest_framework to INSTALLED_APPS in tutorial/settings.py :

```python
INSTALLED_APPS = (
    ...
    'rest_framework',
    'snippets.apps.SnippetsConfig',
)
```

## Creating a model to work with

Snippet model used to store code snippets : add snippet class in snippets/model.py.


Create an initial migration for our snippet model, and sync the database for the first time :

```python
python manage.py makemigrations snippets
python manage.py migrate
```

## Creating a Serializer class

First thing to do in our API : provide a way of serializing and deserializing the snippet instances into representations such as json.
We create a file in the snippets directory named serializers.py.  


Then create a few snippets :

```python
snippet = Snippet(code='foo = "bar"\n')
snippet.save()
``` 

Then we serialize them : 

```python
serializer = SnippetSerializer(snippet)  # In Python datatype 
content = JSONRenderer().render(serializer.data)  # In Json
```

And deserialization : 

```python
stream = BytesIO(content)
data = JSONParser().parse(stream)
```

Restore the native datatypes into a fully populated object instance : 

```python
serializer = SnippetSerializer(data=data)
serializer.is_valid()
serializer.validated_data
serializer.save()
```


Serialize querysets instead of model instances, simply add a many=True flag : 

```python
serializer = SnippetSerializer(Snippet.objects.all(), many=True)
serializer.data
```

## Writing regular Django views using our Serializer



