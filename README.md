# Creating a Basic API
--------------------

Creating a basic API with the Django stack involves setting up your development environment, creating a Django project, and using Django REST Framework (DRF) to build the API endpoints. Here's a step-by-step guide to get you started:

### 1\. Set Up Your Development Environment

Before you begin, ensure you have Python installed. You can download it from the [official Python website](https://www.python.org/downloads/).

1.  
    *   python3 -m venv envActivate the virtual environment:
    
    *   env\\Scripts\\activate
        
    *   source env/bin/activate
        
2.  
    *   pip install django djangorestframework
    

### 2\. Create a Django Project and App

1.  django-admin startproject myprojectcd myproject
    
2.  python manage.py startapp api
    
3.  INSTALLED\_APPS = \[ ... 'rest\_framework', 'api',\]
    

### 3\. Define a Model

In api/models.py, define a simple model. For example, a Book model:

```bash
from django.db import models  

class Book(models.Model):      
    title = models.CharField(max_length=100)      
    author = models.CharField(max_length=100)      
    published_date = models.DateField()      
    
    def __str__(self):          
        return self.title
```

### 4\. Create and Apply Migrations

1.  python manage.py makemigrations
    
2.  python manage.py migrate
    

### 5\. Create a Serializer

Serializers convert model instances to JSON so that they can be easily rendered into API responses. In api/serializers.py, create a serializer for the Book model:

```bash
from rest_framework import serializers  
from .models import Book  

class BookSerializer(serializers.ModelSerializer):      
    class Meta:          
        model = Book          
        fields = '__all__'
```

### 6\. Create API Views

In api/views.py, create views to handle API requests:

```bash
from rest_framework import generics  
from .models import Book  
from .serializers import BookSerializer  

class BookListCreate(generics.ListCreateAPIView):      
    queryset = Book.objects.all()      
    serializer_class = BookSerializer  
    
class BookRetrieveUpdateDestroy(generics.RetrieveUpdateDestroyAPIView):      
    queryset = Book.objects.all()      
    serializer_class = BookSerializer
```

### 7\. Define URL Patterns

Create a urls.py file inside the api app directory and define the API endpoints:

```bash
from django.urls import path  
from .views import BookListCreate, BookRetrieveUpdateDestroy  

urlpatterns = [      
    path('books/', BookListCreate.as_view(), name='book-list-create'),      
    path('books//', BookRetrieveUpdateDestroy.as_view(), name='book-detail'),  
    ]
```

Include the api URLs in the project's main urls.py (myproject/urls.py):

```bash
from django.contrib import admin  
from django.urls import path, include  
urlpatterns = [      
    path('admin/', admin.site.urls),      
    path('api/', include('api.urls')),  
    ]
```

### 8\. Test the API

1.  python manage.py runserver
    
2.  **Access the API Endpoints**:
    
    *   **List/Create Books**: [http://localhost:8000/api/books/](http://localhost:8000/api/books/)
        
    *   **Retrieve/Update/Delete a Book**: [http://localhost:8000/api/books/1/](http://localhost:8000/api/books/1/)
        

You can use tools like [Postman](https://www.postman.com/) or [cURL](https://curl.se/) to interact with the API endpoints.

### 9\. (Optional) Use the Django Admin Interface

To add books via the Django admin interface:

1.  python manage.py createsuperuser
    
2.  from django.contrib import adminfrom .models import Bookadmin.site.register(Book)
    
3.  **Access the Admin Interface**: Navigate to [http://localhost:8000/admin/](http://localhost:8000/admin/) and log in with the superuser credentials to add or manage books.