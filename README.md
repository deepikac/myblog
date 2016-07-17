##Synopsis
--------
This is a Simple blog application above which REST api developed.

##Requirements
-----------
```
django>=1.9
djangorestframework
```

##Installation
------------
By default it is using "sqlite3". And do the following before starting the server.

```
pip install -r requirements.txt

- Go to project dir and then, 

python manage.py makemigrations
python manage.py migrate

```

**settings.py**

- Logger is enabled through settings and currently configured to store log file under [project_dir]/django.log.
- Required apps are updated under INSTALLED_APPS in settings.
- Database configuration can be updated in settings as per requirement. Currenly it is configured to use 'sqlite3'.

``` 
INSTALLED_APPS = (
    'blog',
    'rest_framework',
    'api',
)

* To use MySQL DB  *
DATABASES = {
	'default': {
        'ENGINE': 'django.db.backends.mysql',	#This is driver to use for MySQL
        'NAME': '',								#name of the database to use
        'STORAGE_ENGINE': 'INNODB',
        'USER': '', 							#provide the user name 
        'PASSWORD': '',							#provide the password 
        'HOST': '',								# Set to empty string for localhost.
        'PORT': '',								# Set to empty string for default. 
    }
}

```

## Code Example
Start the server and go to browser and type the **http://<serverip:port>/api/blogs/**

```
Command to start server:

python manage.py runserver
```

**api/urls.py**

```
from django.conf.urls import url

urlpatterns = [
    url(r'^blogs/$',views.blog_list),
    url(r'^blogs/(?P<pk>[0-9]+)$', views.blog_detail),
]
```
**blog/models.py**

```
from django.db import models
from django.utils import timezone

class Blog(models.Model):
    """
    Blog Model
    """
    title = models.CharField(max_length=200)
    body = models.TextField()
    publish =models.BooleanField(default=True)
    created = models.DateTimeField(auto_now_add=True)
    modified = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```
## API Reference
-----
** Blog_list **
```
	Lists all blogs, or create a new blog
	*URL*
		/api/blogs/
	*Method:*
		`GET`,`POST`
	*Data Params*
		Only required for POST
	*URL Params*
		None
	*Success Response:* 
		200 on GET<br/>
		201 on POST<br/>
		Returns status code [json list]
	*Sample Call:*
		command line
		curl http://localhost:8000/api/blogs/
		curl -X POST http://localhost:8000/api/blogs/ -d "title=test blog&body=test description&publish=True"
```
		
** Blog_details **
```
	Get,Update, or delete a specific blog
	
	*URL*
		/api/blogs/:id
	*Method:*
		`GET` `PUT` `DELETE`
	*Data Params*
		Only required for PUT
	*URL Params*
		*Required* `id=[integer]`
	*Success Response:* 
		200 on GET| PUT<br/>
		204 on DELETE<br/>
		Returns status code
	*Sample Call:*
		command line:
		curl -X PUT http://localhost:8000/api/blogs/ -d "title=test blog&body=test description&publish=True"
		curl -X GET http://localhost:8000/api/blogs/1 
		curl -X DELETE http://localhost:8000/api/blogs/1 
```
	
## Tests

The fixtures under [project dir]/fixtures used for testing REST api.

```
Go to project directory and then do:

python manage.py test
```
