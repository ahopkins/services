# Services

This is a pseudo-framework built in the shoulders of [sanic](sanic.dev/) and inspired by [Django](https://www.djangoproject.com/)

The intention is to provide some tools for web services development with focus on data services. 

The library tries to be the less intrusive possible, it is not intended to be a new framework but more to provided abstractions and code generation tools over 
good established libraries and technologies. 

## Features

- Async Web sever (Sanic)
- Generation code for apps (like Django)
- Multiple databases support (sync and async using SQLAlchemy 1.4/2.0)
- Schema migration tools pre-configurated to work in the first run (Alembic)
- OpenApi/Swagger docs generation (Sanic)
- Simple user system and authentication endpoints
- JWT support


## Quickstart

*Note: please use your favorite tool for python environments and dependencies*

```
python3 -m venv .venv
source .venv/bin/activate
pip install ai-services
```

Then you can initialize a project:

```
create-srv-project .

╭───────────────────────────────────────╮
│ 😸 Hello and welcome to  AI services  │
╰───────────────────────────────────────╯
Write a name for default web app please, avoid spaces and capital letters:  (test_app):
The final name for the project is: test_app
╭─────────────────────────────────────────╮
│ 😸 Congrats!!! Project test_app created │
╰─────────────────────────────────────────╯
 To test if everything is working  you can run the following command:

         srv web -L -D
```

It will ask you for a name for the firts app. 

Then your folder will be:

```
 » tree -a -L 2
.
├── alembic.ini
├── server_conf
│   ├── __init__.py
│   ├── __pycache__
│   └── settings.py
└── test_app
    ├── __init__.py
    ├── __pycache__
    ├── db.py
    ├── migrations
    ├── models.py
    ├── users_bp.py
    ├── views_bp.py
    └── web.py
```

Finally, the last step if you want to use the User system provided in the code, you will need to run a revision and upgrade action:

```
srv db revision test_app -m first -R 0001 -m first
srv db upgrade test_app
```

With the default configuration, it will creates a `db.sqlite` file in the root of your project.

Note: srv db uses alembic under the hood and Alembic is configurated in a way that is possible keep using it outside of `srv db`, it is more like a wrapper. 

## Status

:warning: The library is being in use in one project, but it is still under active development and therefore full backward compatibility is not guaranteed before reaching v1.0.0.


## Roadmap:

- [ ] UserManager stabilization
- [ ] Add groups 
- [ ] Expand command for users administration
- [ ] Custom commands hooks in `srv` 
- [ ] Dev env files {Makefile, Dockerfile, docker-compose, etc}
- [ ] Task Queue abstraction {Redis, Google Cloud Pub/Sub, etc}
- [ ] File upload abstraction? (TBD)
- [ ] OAuth 2.0 integration
- [ ] documentation (guides and reference api)
- [ ] Add `setup.py` by default?
- [ ] Tools and abstraction for logging (stdout, google cloud log, etc)
- [ ] Metrics (prometheus)
- [ ] Update to Sanic 22.9
- [ ] Websockets examples

## FAQ

**Why Sanic?**

Regardless FastAPI is the most popular (50K starts in GH vs 16k for sanic) async framework right know and django is the most feature complete and stable(no proofs) web framework in the python world. What is very appealing for me is the own server implementation of Sanic which seems simpler than WSGI and AWSGI (you can still use ASGI with sanic if you want), and because most of the time I need to build web apis to expose Machine Learning models, I found it to be a good match. 

Usually models are very CPU and Mem intensive (an average Word2vec model needs at least 500mb with peaks of 1gb of RAM), so the strategy here is to load it in one main process and share it between the rest of workers. Sanic has a lot of conversations in their community about how process could be managed https://amhopkins.com/posts/background-job-worker.html 

And why not Django, is because their ORM. I found SQLAlchemy more flexible and lightway than Django ORM. 

In data/machine learning solutions is common to work in environments outside of the request/response cycle of a web app (Jupyter, Batch/ETL process, etc) it seems unnecesary to load a web environment for those cases. The other reason is that SQLAlchemy allows to work directly with RAW sql or table, inspect them and avoid the ORM part of the framework, which is very convenient when working with different sources of data. 


## Release

see docs/release.md


