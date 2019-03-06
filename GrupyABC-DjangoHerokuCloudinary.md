Do Zero ao Deploy com Django
============================

--

> ### O que precisa:
* git instalado
* Python >= 3.7
* Conta na Heroku

---

Ambiente de Desenvolvimento
---------------------------
```bash
python3 -m venv venv
source venv/bin/activate
```

--

### Dependencias do projeto
```bash
pip install --upgrade pip
pip install django
pip freeze > requirements.txt
```

--

### Criando o projeto
```
django-admin startproject website .
```

--

### Aplicando as migrações.
```
python manage.py migrate
```

--

### Rodando o projeto
```
python manage.py runserver
```

---

## Heroku
> (Preparação)

O arquivo `Procfile`
```
web: gunicorn myproject.wsgi
```

--

### Dependencias Heroku
```
pip install gunicorn
pip install django-heroku
```

--

Add the following import statement to the top of `settings.py`:
```python
import django_heroku
```

Then add the following to the bottom of `settings.py`:
```python
### Activate Django-Heroku.
django_heroku.settings(locals())
```

--

Download and install the Heroku CLI.
If you haven't already, log in to your Heroku account and follow the prompts to create a new SSH public key.
```
$ heroku login
```

--

Create a new Git repository
Initialize a git repository in a new or existing directory

```
$ cd my-project/
$ git init
$ heroku git:remote -a palestra-teste-demo
```

--

Deploy your application
Commit your code to the repository and deploy it to Heroku using Git.

```
$ git add .
$ git commit -am "make it better"
$ git push heroku master
```

--

Existing Git repository
For existing repositories, simply add the heroku remote

```
$ heroku git:remote -a palestra-teste-demo
```

---

## Primeira aplicação (core)
```
django-admin startapp core
```

Adicionar aplicação ao `settings.py` `(INSTALLED_APPS)`

--

## Testes, Views, URLs
### View (Index / Home)
```python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
    return HttpResponse('<h1>Olá Mundo</h1>')
    # return render(request, 'index.html')
```

--

### URL (core)
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

--

#### Arquivos Estáticos.
Em `settings.py`:
```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.1/howto/static-files/

STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

# Media Files (Uploads)
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'mediafiles')
```

--

### URL (website)
Adicionar aos paths em 'website/urls.py'
```python
...
from django.conf.urls.static import static
from website import settings
...
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('core.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
...
```

--

### Templates
Nos templates, precisa carregar os arquivos estáticos no topo do `template.html`.
```django
{% load static %}
<!-- e para usar... -->
<link rel="icon" href="{% static 'favicon.png' %}" type="image/png">
```

--

##### Templates: Tags mais comuns.
```django
{% extends "base.html" %}

{% include 'navbar.html' %}
```

--

#### Templates: Exemplo de laço `for`.
```django
<ul>
{% for aluno in aluno_lista %}
    <li>{{ aluno.nome }}</li>
{% endfor %}
</ul>
```

--

#### Templates: Exemplo de condicional `if`.
```django
{% if alunos_list %}
    Número de alunos: {{ alunos_list|length }}
{% elif alunos_no_laboratorio_list %}
    Alunos devem deixar o laboratório em breve!
{% else %}
    Nenhum aluno.
{% endif %}
```

`if` pode ser usado com os operadores `==`, `!=`, `<`, `>`, `<=`, `>=`, `in`, `not in`, `is` e `is not`


---

### Testes



## Cloudinary



## Desacoplando dados sensíveis

## Models

## Forms

## CI / CD

