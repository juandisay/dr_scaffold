<p align="center"><a href="https://github.com/Abdenasser/dr_scaffold"><img src="https://ph-files.imgix.net/99f3cc0a-58b1-4c16-bb41-1963b0a692fc.png" alt="dr_scaffold blueprint icon" height="80"/></a></p>
<h1 align="center">dr_scaffold</h1>
<p align="center">Scaffold django rest apis like a champion ⚡. said no one before</p>

<p align="center">
    <a href="https://codecov.io/gh/Abdenasser/dr_scaffold"><img src="https://codecov.io/gh/Abdenasser/dr_scaffold/branch/main/graph/badge.svg?token=VLUZWSTJV2"/></a> <a href="https://app.travis-ci.com/Abdenasser/dr_scaffold"><img src="https://app.travis-ci.com/Abdenasser/dr_scaffold.svg?branch=main"/></a> <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/pypi/l/ansicolortags.svg"/></a> <a href="https://pypi.org/project/dr-scaffold/"><img src="https://d25lcipzij17d.cloudfront.net/badge.svg?id=py&r=r&type=6e&v=2.0.2&x2=0"/></a> <a href="https://twitter.com/intent/tweet?text=Scaffold django rest apis like a champion ⚡. said no one before.&url=https://github.com/Abdenasser/dr_scaffold&hashtags=python,opensource,django,api,developers"><img src="http://randojs.com/images/tweetShield.svg" alt="Tweet" height="20"/></a>

</p>

# Overview

This library will help you to scaffold full **Restful API Resources** in seconds using only one command:

```console
$ python manage.py dr_scaffold blog Post body:textfield author:foreignkey:Author
```

- `models.py` with Models and fields generated by the CLI ⚡
- `admin.py` with Models registered and ready ⚡
- `views.py` with appropriate ViewSets ready⚡
- `urls.py` with appropriate URLs ready.⚡
- `serializers.py` with Model Serializers ready ⚡

- and more ...

# Installation and usage

> For a detailed guide read [scaffold django apis like a champion](https://abdenasser.com/scaffold-django-apis-like-a-champion/), this library assumes that you have **Django Rest Framework**. if not, please refer to [this guide](https://www.django-rest-framework.org/#installation).

#### Install dr_scaffold package :

```console
$ pip install dr-scaffold
```

#### Add dr_scaffold to your INSTALLED_APPS like this:

```python
INSTALLED_APPS = [
    ...
    'dr_scaffold'
]
```

#### Add CORE_FOLDER and API_FOLDER to your settings.py (optional):

```python
CORE_FOLDER = "core_dir/"
API_FOLDER = "api_dir/"
```

#### Run your scaffolds like this:

```console
$ python manage.py dr_scaffold blog Post body:textfield author:foreignkey:Author
```

# Generate tests

We support generating tests for your models and apis, you can generate tests by adding `--tests` to your command like follow:

```console
$ python manage.py dr_scaffold blog Author name:charfield --tests
```

This will generate factories for your models and their tests (ViewSets tests will be added soon), we depend on `pytest` and `factory_boy` so you should have them installed in order to run your tests.

Also bare in mind that you should run your migrations before running the tests

# Generate ViewSet Mixins

We support two types of ViewSets, we support **ModelViewSet** and we
support **ViewSets** with Mixins.

- ModelViewSets are the default that get generated with the
  dr_scaffold command
- To generate a view with Mixins pass a value of what mixins you want
  to include like `--mixins CRUD` this will result in a view with the
  Create, List, Retrieve, Update, Destroy actions.

Let's generate an API that does only support the **Create** and **Read**
methods (Read is both list and retrieve):

```console
$ python manage.py dr_scaffold blog Author name:charfield --mixins CR
```

The command will generate an Author API with a ViewSet like the
following:

```python
class AuthorViewSet(
    mixins.CreateModelMixin,
    mixins.ListModelMixin,
    mixins.RetrieveModelMixin,
    viewsets.GenericViewSet
):
    queryset = Author.objects.all()
    serializer_class = AuthorSerializer
    #permission_classes = (permissions.IsAuthenticated,)

    def get_queryset(self):
        #user = self.request.user
        queryset = Author.objects.all()
        #insert specific queryset logic here
        return queryset

    def get_object(self):
        #insert specific get_object logic here
        return super().get_object()

    def create(self, request, *args, **kwargs):
        serializer = AuthorSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        serializer.save()
        return Response(serializer.data)

    def list(self, request, *args, **kwargs):
        queryset = self.get_queryset()
        serializer = AuthorSerializer(queryset, many=True)
        return Response(serializer.data)

    def retrieve(self, request, *args, **kwargs):
        instance = self.get_object()
        serializer = AuthorSerializer(instance=instance)
        return Response(serializer.data)
```

# Supported field types

We support most of django field types.

# Contributors

This project exists thanks to all the people who contribute. [[Contribute](CONTRIBUTING.md)]

<a href="https://github.com/Abdenasser/dr_scaffold/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Abdenasser/dr_scaffold" />
</a>
