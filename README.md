# django_abc

## what it does
With Django alone, you cannot directly call a subclass method on objects retrieved using a base class's manager. With django_abc, you can easily call the subclass instance's method without manually casting. This follows the expected polymorphic behavior as with classic object oriented programming.

## installation

`pip install django_abc`

## using the decorators

When coding Django Models of which you want to use inheritance, use the approach of Django's "[multi-table inheritance](https://docs.djangoproject.com/en/2.0/topics/db/models/#multi-table-inheritance)" combined with this library.

```
from django.db import models
from django_abc import base, implementor

@base
class Piece(models.Model):
    abstract = ['present']  # list of method names that each subclass should implement

@implementor
class Music(Piece):
    def present(self, context):
        context['pieces'].append(self.lyrics)

@implementor
class Poem(Piece):
    def present(self, context):
        context['pieces'].append(self.text)
```

As a result of coding your classes this way, you can iterate through objects using the Piece manager and call the correct subclass's method:
```
for piece in Piece.objects.all():
    piece.present(context)
```
