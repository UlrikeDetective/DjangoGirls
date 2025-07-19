
````markdown
# 🌸 Django Girls Blog — macOS Setup Guide

This guide walks you through setting up your Django Girls blog on **macOS**.

---

## ✅ 1. Create Virtual Environment

```bash
python3 -m venv myvenv
source myvenv/bin/activate
````

---

## ✅ 2. Install Django

```bash
pip install django
```

---

## ✅ 3. Start Django Project

```bash
django-admin startproject mysite .
```

---

## ✅ 4. Adjust Settings (`mysite/settings.py`)

```python
TIME_ZONE = 'Europe/Berlin'
LANGUAGE_CODE = 'de-ch'

STATIC_URL = 'static/'
STATIC_ROOT = BASE_DIR / 'static'

ALLOWED_HOSTS = ['localhost', '127.0.0.1', '.pythonanywhere.com']
```

---

## ✅ 5. Initialize Database

```bash
python manage.py migrate
```

---

## ✅ 6. Run Local Development Server

```bash
python manage.py runserver
```

Open: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## ✅ 7. Create Blog App

```bash
python manage.py startapp blog
```

Add `'blog',` to `INSTALLED_APPS` in `mysite/settings.py`.

---

## ✅ 8. Define Model (`blog/models.py`)

```python
from django.conf import settings
from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

---

## ✅ 9. Apply Migrations

```bash
python manage.py makemigrations blog
python manage.py migrate blog
```

---

## ✅ 10. Register Model (`blog/admin.py`)

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

Create admin user:

```bash
python manage.py createsuperuser
```

---

## ✅ 11. URL Configuration

**`mysite/urls.py`:**

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

**Create `blog/urls.py`:**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```

---

## ✅ 12. Create View (`blog/views.py`)

```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```

---

## ✅ 13. Create Template

**`blog/templates/blog/post_list.html`:**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Django Blog</title>
</head>
<body>
    <h1>Django Blog</h1>
    {% for post in posts %}
        <article>
            <h2>{{ post.title }}</h2>
            <p><em>{{ post.published_date }}</em></p>
            <div>{{ post.text|linebreaksbr }}</div>
        </article>
    {% endfor %}
</body>
</html>
```

---

## ✅ 14. Commit to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<your-username>/my-first-blog.git
git push -u origin HEAD
```

If GitHub password login fails, [create a personal access token](https://github.com/settings/tokens) and use that as your password.

---

## ✅ 15. Deploy on PythonAnywhere

In **PythonAnywhere Bash console**:

```bash
pip3.10 install --user pythonanywhere
pa_autoconfigure_django.py --python=3.10 https://github.com/<your-username>/my-first-blog.git
python manage.py createsuperuser
```

Your site is live at:

```
https://<your-pythonanywhere-username>.pythonanywhere.com
```

---

🎉 Congrats — your Django blog is now live!

```