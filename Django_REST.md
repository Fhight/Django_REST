# Django REST Framework Tutorial

## Introduction
Django REST framework (DRF) is a powerful and flexible toolkit for building Web APIs in Django.

## Prerequisites
- Python installed (version 3.6+)
- Django installed (version 3.0+)
- Basic knowledge of Django

## Installation and Setup

1. **Create a new text file called `requirements.txt` and add the following lines:**
    ```
    Django >= 3.0
    djangorestframework >= 3.12
    environs >= 9.3
    ```

2. **Create a new virtual environment:**
    ```bash
    python -m venv venv

    venv\Scripts\activate
    ```

3. **Install the required packages:**
    ```bash
    pip install -r requirements.txt
    ```

4. **Create a new Django project:**
    ```bash
    django-admin startproject [project_name]
    cd [project_name]
    ```

    If the previous command doesn't work, try this one:
    ```bash
    py -m django startproject [project_name]
    cd [project_name]
    ```

5.  **Create a new Django app:**
    ```bash
    python manage.py startapp [app_name]
    ```

6. **Create a new Django app:**
    ```bash
    python manage.py startapp [app_name]
    ```

7. **Add the new app and Django REST framework to `INSTALLED_APPS` in `settings.py`:**
    ```python
    INSTALLED_APPS = [
        ...
        '[app_name]',
        'rest_framework',
    ]
    ```

## Creating a Model

1. **Define a model in `[app_name]/models.py`:**
    ```python
    from django.db import models

    class Item(models.Model):
        name = models.CharField(max_length=100)
        description = models.TextField()
        published = models.BooleanField(default=False)
        created_at = models.DateTimeField(auto_now_add=True)

        def __str__(self):
            return self.name
    ```

2. **Run migrations:**
    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

## Creating a Serializer

1. **Create a serializer in `[app_name]/serializers.py`:**
    ```python
    from rest_framework import serializers
    from .models import Item

    class ItemSerializer(serializers.ModelSerializer):
        class Meta:
            model = Item
            fields = '__all__'
    ```

## Creating Views

1. **Create views in `[app_name]/views.py`:**
    ```python
    from rest_framework import generics
    from .models import Item
    from .serializers import ItemSerializer

    class ItemListCreate(generics.ListCreateAPIView):
        queryset = Item.objects.all()
        serializer_class = ItemSerializer

    class ItemDetail(generics.RetrieveUpdateDestroyAPIView):
        queryset = Item.objects.all()
        serializer_class = ItemSerializer
    ```

## Configuring URLs

1. **Add URLs in `[app_name]/urls.py`:**
    ```python
    from django.urls import path, include
    from .views import ItemListCreate, ItemDetail

    urlpatterns = [
        path('items/', ItemListCreate.as_view(), name='item-list-create'),
        path('items/<int:pk>/', ItemDetail.as_view(), name='item-detail'),
    ]
    ```

2. **Include app URLs in `[project_name]/urls.py`:**
    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/', include('[app_name].urls')),
    ]
    ```

## Testing the API

1. **Run the development server:**
    ```bash
    python manage.py runserver
    ```

2. **Test the API endpoints:**
    - List and create items: `http://127.0.0.1:8000/api/items/`
    - Retrieve, update, and delete an item: `http://127.0.0.1:8000/api/items/<id>/`

For more information, visit the [Django REST framework documentation](https://www.django-rest-framework.org/).