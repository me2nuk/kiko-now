---
layout: post
title:  "Python requests 모듈(module) 사용법"
tags: [python,requests,Guide]
excerpt: "Python requests 모듈(module) 사용법"
---

* * *

# requests 모듈이란?

requests는 python사용자들을 위해 만들어진 간단한 Python용 HTTP 라이브러리입니다.

#### References

+ [requests Github](https://github.com/psf/requests)
+ [requests Documentation](https://docs.python-requests.org/)

* * *

## requests Example Code

하단의 코드는 requests 모듈을 이용하여 ``https://example.com`` URL에 요청한 다음 반환되는 값을 출력한 코드입니다.

```py
>>> import requests
>>> r = requests.get("https://example.com/")
>>> r.status_code
200
>>> r.text
'<!doctype html>\n<html>\n<head>\n    <title>Example Domain</title[ ... ]\n</body>\n</html>\n'
```

* * *

## HTTP 요청 메서드(HTTP request method)

http 메서드는 클라이언트가 서버에게 요청하는 목적 및 그 종류를 알리는 수단 입니다.

또한 요청 메서드냐에 따라 요청/응답하는 방식 또한 다르며 다양한 메서드가 존재합니다.

대표적으로 쓰이는 메서드는 GET, POST 방식입니다.

#### References

+ [HTTP method Documentation](https://tools.ietf.org/html/rfc2616#section-9.1.2)

* * *

## requests GET, POST, PUT , DELETE, HEAD, OPTIONS

HTTP 메소드마다 사용되는 함수와 매개변수가 각각 조금씩 다르며 일반적인 요청의 경우 함수명만 변경하여 쉽게 할 수 있습니다.

그리고 요청의 모든 기능은 하단의 코드 예시와 같이 7가지 방법으로 액세스할 수 있습니다. 

모두 Response 개체의 인스턴스를 반환 합니다.

* * *

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r = requests.post("https://example.com")
>>> r = requests.put("https://example.com")
>>> r = requests.head("https://example.com")
>>> r = requests.patch("https://example.com")
>>> r = requests.delete("https://example.com")
>>> r = requests.options("httpS://example.com")
```

* * *

# requests 모듈 기본 사용법

requests는 from 또는 import로 모듈 불러오면 사용할 수 있습니다.

```py
>>> from requests import *
>>> import requests
```

* * *

# Request

> ``requests.request(method, url, **kwargs)``


요청의 모든 기능은 7가지 방법으로 액세스 할 수 있습니다.

``**kwargs``에 들어가는 옵션은 ``request`` 메소드에 있는

* * *

### Request Headers

requests 모듈은 [``requests.utils.default_headers``](https://github.com/psf/requests/blob/master/requests/utils.py#L808-L826) 함수에 의해 새로운 요청 시 기본값으로 Header 4개가 포함됩니다.

<br>

* * *

### Request method

requests 클래스에서 지원하는 요청 메서드를 쉽게 사용하기 위해선 총 7가지의 메소드를 이용하여 활용할 수 있습니다.

``HTTP/1.1`` 버전의 경우 여러가지의 메소드가 존재하는데 

requests 모듈은 ``[PUT, GET, POST, HEAD, PATCH, DELETE, OPTIONS]`` 메서드가 존재합니다.

* * *

## 요약

+ parameter

    + url( 필수 )
        url 매개 변수는  ``requests.request`` 객체에 사용되기 위한 URL 입니다.

        > url=https://google.com

    + params

        튜플(tuple), 딕셔너리(dict)형식으로 매개변수에 넣으면 양식이 URL 인코딩이 되어 URL에 추가됩니다.

        > URL?key=value&key1=value1

    + data(선택 사항)

        튜플(tuple), 딕셔너리(dict)형식으로 매개변수에 넣으면 양식이 인코딩되어 요청 본문에 추가됩니다.

        > key=value&key1=value1

    + json(선택 사항)

        JSON 매개 변수를 이용하여 요청 본문에 json 형식으로 추가됩니다.

        > { 'key':'value', 'key1':'value1' }

    + return
        
        ``[PUT, GET, POST, HEEAD, PATCH, DELETE, OPTIONS]``는 기본적으로 [requests.Response](https://docs.python-requests.org/en/master/api/#requests.Response) 객체를 반환합니다.
 
+ PUT

> ``requests.put(url, data=None, **kwargs)``

+ GET

> ``requests.get(url, params=None, **kwargs)``

+ POST

> ``requests.post(url, data=None, json=None, **kwargs)``

+ HEAD

> ``requests.head(url, **kwargs)``

+ PATCH

> ``requests.patch(url, data=None, **kwargs)``

+ DELETE

> ``requests.delete(url, **kwargs)``

+ OPTIONS

> ``requests.options(url, **kwargs)``

* * *

## 자세히

### ``PUT``

> ``requests.put(url, data=None, **kwargs)``

```py
>>> r = requests.put("http://127.0.0.1:8080", data={'put1':'data1', 'put2':'data2'})
>>> r.request.method
'PUT'
>>> r.request.body
'put1=data1&put2=data2'
```

* * *

### ``GET``

> ``requests.get(url, params=None, **kwargs)``

get 메소드는 요청 시 GET 방식으로 요청되며 ``params`` 매개 변수를 지원합니다.

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r.request.method
'GET'
>>>
>>> r.url
'https://example.com'
>>> r = requests.get("https://example.com", params={'data1':'value1','data2':'value2'})
>>> r.url
'https://example.com/?data1=value1&data2=value2'
```

* * *

### ``POST``

> ``requests.post(url, data=None, json=None, **kwargs)``

post 메소드는 요청 시 POST 방식으로 요청되며 ``data``, ``json`` 매개 변수가 존재합니다.

data, json 두 개의 매개변수는 비슷해 보이지만 요청할 때 헤더의 Content-Type이 달라집니다.

> Example Code

```py
>>> r = requests.post("https://example.com", data={'post1':'data1', 'post2':'data2'})
>>> r.request.method
'POST'
>>>
>>> r.request.body
'post1=data1&post2=data2'
>>> r.request.headers['Content-Type']
'application/x-www-form-urlencoded'
>>>
>>>
>>> r = requests.post("https://example.com", json={'post1':'data1', 'post2':'data2'})
>>> r.request.body
b'{"post1": "data1", "post2": "data2"}'
>>> r.request.headers['Content-Type']
'application/json'
```

* * *

### ``HEAD``

> ``requests.head(url, **kwargs)``

head 메소드는 요청 시 HEAD 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.put("http://127.0.0.1:8080")
>>> r.request.method
'HEAD'
```

* * *

### ``PATCH``

> ``requests.patch(url, data=None, **kwargs)``

patch 메소드는 요청 시 PATCH 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.patch("http://127.0.0.1:8080", data={'patch1':'data1', 'patch2':'data2'})
>>> r.request.method
'PATCH'
>>> r.request.body
'patch1=data1&patch2=data2'
```

* * *

### ``DELETE``

> ``requests.delete(url, **kwargs)``

delete 메소드는 요청 시 DELETE 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.delete("http://127.0.0.1:8080")
>>> r.request.method
'DELETE'
```

* * *

### ``OPTIONS``

> ``requests.options(url, **kwargs)``

options 메소드는 요청 시 OPTIONS 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.options("http://127.0.0.1:8080")
>>> r.request.method
'OPTIONS'
```

<br>

* * *

<br>

# Response

> ``Class requests.Response``

``Response``는 HTTP 요청에 대한 서버의 응답을 포함한 객체 입니다.

대부분의 예시는 하단의 코드로 대체됩니다.

```py
>>> import requests
>>> r = requests.get("https:/example.com")
```

+ ### ``r.text``

text는 서버의 콘텐츠를 자동으로 디코딩하여 출력해줍니다.

```py
>>> r.text
'<!doctype html>\n<html>\n<head>\n    <title>Example Domain</title>[ ... ]v>\n</body>\n</html>\n'
>>> print(r.text)
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
    [ ... ]
</body>
</html>
```

* * *

+ ### ``r.content``

content는 서버의 콘텐츠를 byte 타입으로 반환됩니다.

```py
>>> r.content
b'<!doctype html>\n<html>\n<head>\n    <title>Example Domain</title> [ ... ] </body>\n</html>\n'
>>> type(r.content)
<class 'bytes'>
>>> print(r.content.decode())
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;

    }
    div {
        width: 600px;
    [ ... ]
</body>
</html>
>>>
```

* * *

+ ### ``r.json()``

json() 는 requests 요청한 뒤 응답을 json 형식으로 디코딩하여 반환합니다.

만약 올바른 json 형식이 아닌 경우 에러를 반환합니다.

```py
>>> import requests
>>> r = requests.get("https://example.com")
>>> r.json()
Traceback (most recent call last):
  File "<[ ... ] "
    raise JSONDecodeError("Expecting value", s, err.value) from None
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
>>> r = requests.get("https://api.github.com/events")
>>> r.json()
[{'id': '16070464781', 'type': 'ForkEvent', 'actor': [ ... ] 'at': '2021-04-24T10:30:31Z'}]
>>> type(r.json())
<class 'list'>
```

* * *

### ``r.raw.read()``

``raw.read()``는 요청시 ``stream=True``를 추가해줘야 사용이 가능하며

read() 함수를 호출할떄마다 하단의 결과대로 나옵니다.

```py
>>> r = requests.get("https://example.com", stream=True)
>>>
>>> r.raw
<urllib3.response.HTTPResponse object at 0x7fd2c459ad90>
>>> r.raw.read(10)
b'\x1f\x8b\x08\x00\xc2\x15\xa8]\x00\x03'
>>> r.raw.read(10)
b'}TMs\xdb \x10\xbd\xfbW'
```

# 현재 작성중...