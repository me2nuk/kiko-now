---
layout: post
title:  "Flask URL 라우팅(Routing) 방법(URL Route Registrations)"
tags: [python,Flask,Server,Guide]
excerpt: "Flask URL 라우팅(Routing) 방법(URL Route Registrations)"
---

* * *

# Flask

Flask에서 자주 사용하는 route 데코레이터와 관련된것들에 대해 정리

만약 Flask에 대해 잘 모르거나 Python으로 기본적인 구축할 줄 모른다면 [해당 블로그](/Flask-Server-Basic-building/)를 참고하면 됩니다.

* * *

## Flask Route란?

> ### ``route(rule, **options)``

여기에서 말하는 [route](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Flask.route)는 Flask 모듈에 존재하는 URL을 등록하기 위해 사용되는 [데코레이터](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Flask.route) 이며 ``add_route_rule()`` 데코레이터 사용을 위한 것입니다.

route 데코레이터를 사용하는 기본적인 예시는

```py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello Index'

app.run('127.0.0.1', 8080)
```

위의 예시 코드와 같습니다.

해당 코드를 해석하자면 ``@app.route('/')`` 데코레이터와 ``index()`` 함수와 연결되면서 해당 함수의 반환값을 ``route(URL)`` URL으로 접속하면 웹 페이지에 나오게 됩니다.

* * *

> ### ``add_url_rule(rule, endpoint=None, view_func=None,   provide_automatic_options=None, **options)``

하지만 위에서 언급했던 [``add_route_rule()``](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Flask.add_url_rule) 데코레이터 또한 ``route()`` 데코레이터와 똑같이 작동하지만 사용 방법이 다릅니다.

```py
def index():
        return 'Hello Index'
app.add_url_rule('/', 'index', index)
```

``route()`` 데코레이터와 사용 방법은 다르지만 위의 코드처럼 동일한 동작을 할 수 있습니다.

* * *

## Route 데코레이터 매개변수

> ### ``route(rule, **options)``

route 데코레이터는 ``rule, **options`` 총 2개의 매개 변수를 가지고 있습니다.

* * *

#### # <span style="color:#4B758D;">rule</span>

``rule``는 URL의 규칙을 가리킵니다.

> example code

```py
@app.route('/')
```

해당 코드는 ``/`` URL 경로가 생성됩니다.

``route( URL )`` 안에 들어가는 문자열의 URL 경로가 생성되는것입니다.

* * *

#### # <span style="color:#4B758D;">**options</span>

``**options``는 기본 [Rule개체](https://werkzeug.palletsprojects.com/en/1.0.x/routing/#werkzeug.routing.Rule)로 전달할 옵션입니다. rule 으로 생성된 하나의 경로에 옵션을 제한할 수 있습니다.

> example code

```py
@app.route('/', methods=['GET'])
```

해당 코드는 ``/`` URL 경로에 요청 메소드가 ``GET`` 방식으로 제한됩니다.

> ### ``Rule(string, defaults=None, subdomain=None, methods=None, build_only=False, endpoint=None, strict_slashes=None, merge_slashes=None, redirect_to=None, alias=False, host=None, websocket=False)``

위의 예시코드에서 사용된 methods 또한 Rule 개체와 연결되며 methods 매개 변수 외에도 많은 옵션이 존재합니다.

* * *

## URL Routing

Flask에서 URL 라우팅 하는 방법은 세 가지의 방법이 존재합니다.

### route 데코레이터

> ##### ```route(rule, **options)```

> example code

```py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Routing'

app.run('127.0.0.1', 8080)
```

``Flask.route`` 데코레이터를 이용하여 URL을 등록한 다음 index 함수와 연결시킬 수 있습니다.

### add_url_rule 데코레이터

> ##### ```add_url_rule(rule, endpoint=None, view_func=None, provide_automatic_options=None, **options)```

> example code

```py
from flask import Flask

app = Flask(__name__)

def index():
    return 'Routing'

app.add_url_rule('/', 'index', index)
app.run('127.0.0.1', 8080)
```

``Flask.add_url_rule`` 데코레이터는 ``Flask.route``와 URL 등로갛는 과정은 다르지만 작동되는 구조는 동일합니다.

### url_map 