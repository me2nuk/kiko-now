---
layout: post
title:  "Flask 서버 구축하는법"
tags: [python,Flask,Server]
excerpt: "Flask 서버 구축하는법"
---

* * *

# Flask이란?

Flask는 Jinja 템플릿 엔진과 Werkzeug WSGI 툴킷에 의존하며 파이썬으로 제작된 웹 프레임워크입니다.

Flask를 이용하여 다양한 웹 서비스 또는 API를 구축할 수 있고 문법이 간결하며 환경을 구축하기 쉽습니다.

<br>

<p align=center>
    <img rel="Flask logo" src="/post-images/Flask/flask-logo.png">
</p>
<br>

* * *

## > <span style="color:#4B758D;">Flask를 쓰는 이유</span>

django 또한 Flask와 비슷한 웹 프레임워크지만 django가 훨 씬 더 유지보수가 쉽고 어느정도 규모가 있는 web application 개발해도 문제가 없지만 Flask는 django와 다르게 환경 구축를 하기 쉽고 코드 길이가 엄청 간결합니다.

django는 여러 파일을 구성해야되는 반면 Flask는 한개의 파일의 5줄로도 웹 서버를 가동시킬 수 있습니다.

> 이처럼 환격 구축과 코드 길이가 간결하여 임시적으로 테스트할때 빠르게 쓰기 쉽고 구축이 빠릅니다.

[Flask Documentation](https://flask.palletsprojects.com/en/1.1.x/)

* * *

# Flask 서버 구축하기

Flask를 사용하기 위해선 Python 3.5 이상, Python 2.7 및 PyPy의 버전을 사용해야 합니다.

* * *

## <span style="color:#4B758D;">> Flask 다운로드</span>

pip 또는 pip3 명령어로 Python의 패키지를 다운로드하면 됩니다.

* * *

```bash
~$pip install flask
~$pip3 install flask
```

해당 flask 패키지가 설치됐는지 확인하기 위해선 많이 사용하는 3가지의 방법이 존재합니다.

#### <span style="color:#71B091;"># 한개의 패키지 정보 확인</span>

```bash
~$pip show flask
```

#### <span style="color:#71B091;"># Python 코드로 직접 패키지 불러오기</span>

```bash
python -c "import flask"
```

#### <span style="color:#71B091;"># 패키지 리스트중에 있는지 확인하기</span>

```bash
~$pip list | grpe "flask"
```

* * *

## <span style="color:#4B758D;">> Flask 실행하기</span>

Python으로 ``Hello World!``를 출력하는 웹 사이트 소스코드를 만들면 하단의 소스코드처럼 구성됩니다.

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello World!'

app.run(host = '127.0.0.1', port=8080)
```

해당 파이썬 소스코드를 ``app.py`` 파일로 실행할시에

```bash
~$python3 app.py
    * Serving Flask app "app" (lazy loading)
    * Environment: production
      WARNING: This is a development server. Do not use it in a production deployment.
      Use a production WSGI server instead.
    * Debug mode: off
    * Running on http://127.0.0.1:8080/ (Press CTRL+C to quit)
```

이렇게 로컬에 8080 포트로 제작된 웹 서버가 제작된것을 볼 수 있는데

해당 ``127.0.0.1:8080``으로 들어가보면 

```python
@app.route('/')
def index():
    return 'Hello World!'
```

이부분으로 인해 return에 있는 ``Hello World!``가 웹 페이지에 노출되면서 기본적인 웹 사이트가 구성되었습니다.

만약 Flask로 개발된 웹 서버를 외부로 배포하고싶다면 Python를 실행시킬 수 있는 서버 또는 리눅스를 호스팅하거나 포트포워딩을 이용하여 외부에 배포할 수 있습니다.

## 마치며

django도 좋은 웹 프레임 워크지만 Flask는 문법이 간결하여 초보자가 공부하기에 좋은 웹 프레임 워크라고 생각한다.