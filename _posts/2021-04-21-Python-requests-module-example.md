---
layout: post
title:  "Python requests 모듈(module) 사용법"
tags: [python,requests,Guide]
excerpt: "Python requests 모듈(module) 사용법"
---

* * *

# requests 모듈이란?

requests는 python사용자들을 위해 만들어진 간단한 Python용 HTTP 라이브러리입니다.

requests 코드 예시를 위해 [httpbin](http://httpbin.org/), [example](https://example.com), [google](https://www.google.com) 3개의 사이트를 이용했습니다.

#### References

+ [requests Github](https://github.com/psf/requests)
+ [requests Documentation](https://docs.python-requests.org/)

* * *

### requests module install

pip 명령어를 이용하여 requests 모듈을 다운로드 하면 import 또는 from 으로 사용 가능합니다.

```py
~$ pip install requests
~$ pip3 install requests
```

## requests Example Code

하단의 코드는 requests 모듈을 이용하여 ``https://example.com`` URL에 요청한 다음 반환되는 값을 출력한 코드입니다.

``[ ... ]`` = 부분 생략

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
>>> r = requests.get("http://httpbin.org/get")
>>> r = requests.post("http://httpbin.org/post")
>>> r = requests.put("http://httpbin.org/put")
>>> r = requests.head("http://httpbin.org/get")
>>> r = requests.patch("http://httpbin.org/patch")
>>> r = requests.delete("http://httpbin.org/delete")
>>> r = requests.options("http://httpbin.org/get")
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

### 간단히

+ parameter

    + url( 필수 )

        url 매개 변수는  ``requests.request`` 객체에 사용되기 위한 URL 입니다.

        > https://google.com

    + params(선택 사항)

        튜플(tuple), 딕셔너리(dict)형식으로 매개변수에 넣으면 양식이 URL 인코딩이 되어 URL에 추가됩니다.

        > URL?key=value&key1=value1

    + data(선택 사항)

        튜플(tuple), 딕셔너리(dict)형식으로 매개변수에 넣으면 양식이 인코딩되어 요청 본문에 추가됩니다.

        > key=value&key1=value1

    + json(선택 사항)

        JSON 매개 변수를 이용하여 요청 본문에 json 형식으로 추가됩니다.

        > { 'key':'value', 'key1':'value1' }

    + **kwargs( 선택 사항 )

        ``**kwargs``는 요청하기 위한 매개변수 옵션이며 [requests.sessions.Session.request](https://docs.python-requests.org/en/latest/_modules/requests/sessions/#Session.request)로 연결됩니다.

        [자세한 옵션 사용법은 여기를 참고하면 됩니다.](#request-kwargs)

        + **kwargs 옵션 종류

            > ``request(self, method, url,
            params=None, data=None, headers=None, cookies=None, files=None,
            auth=None, timeout=None, allow_redirects=True, proxies=None,
            hooks=None, stream=None, verify=None, cert=None, json=None)``

    + return
        
        ``[PUT, GET, POST, HEEAD, PATCH, DELETE, OPTIONS]``는 기본적으로 [requests.Response](https://docs.python-requests.org/en/master/api/#requests.Response) 객체를 반환합니다.
 
+ PUT

> ``requests.put(url, data=None, **kwargs)`` [_Souce Code_](https://docs.python-requests.org/en/latest/_modules/requests/api/#put)

+ GET

> ``requests.get(url, params=None, **kwargs)`` [_Souce Code_](https://docs.python-requests.org/en/latest/_modules/requests/api/#get)

+ POST

> ``requests.post(url, data=None, json=None, **kwargs)`` [_Souce Code_](https://docs.python-requests.org/en/latest/_modules/requests/api/#post)

+ HEAD

> ``requests.head(url, **kwargs)`` [_Souce Code_](https://docs.python-requests.org/en/latest/_modules/requests/api/#head)

+ PATCH

> ``requests.patch(url, data=None, **kwargs)`` [_Souce Code_](https://docs.python-requests.org/en/latest/_modules/requests/api/#patch)

+ DELETE

> ``requests.delete(url, **kwargs)`` [_Souce Code_](https://docs.python-requests.org/en/latest/_modules/requests/api/#delete)

+ OPTIONS

> ``requests.options(url, **kwargs)`` [_Souce Code_](https://docs.python-requests.org/en/latest/_modules/requests/api/#options)

* * *

## 자세히

### ``PUT``

> ``requests.put(url, data=None, **kwargs)``

```py
>>> r = requests.put("http://httpbin.org/put", data={'put1':'data1', 'put2':'data2'})
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
>>> r = requests.get("http://httpbin.org/get")
>>> r.request.method
'GET'
>>>
>>> r.url
'http://httpbin.org/get'
>>> r = requests.get("http://httpbin.org/get", params={'data1':'value1','data2':'value2'})
>>> r.url
'http://httpbin.org/get/?data1=value1&data2=value2'
```

* * *

### ``POST``

> ``requests.post(url, data=None, json=None, **kwargs)``

post 메소드는 요청 시 POST 방식으로 요청되며 ``data``, ``json`` 매개 변수가 존재합니다.

data, json 두 개의 매개변수는 비슷해 보이지만 요청할 때 헤더의 Content-Type이 달라집니다.

> Example Code

```py
>>> r = requests.post("http://httpbin.org/post", data={'post1':'data1', 'post2':'data2'})
>>> r.request.method
'POST'
>>>
>>> r.request.body
'post1=data1&post2=data2'
>>> r.request.headers['Content-Type']
'application/x-www-form-urlencoded'
>>>
>>>
>>> r = requests.post("http://httpbin.org/post", json={'post1':'data1', 'post2':'data2'})
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
>>> r = requests.put("https://www.google.com")
>>> r.request.method
'HEAD'
```

* * *

### ``PATCH``

> ``requests.patch(url, data=None, **kwargs)``

patch 메소드는 요청 시 PATCH 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.patch("http://httpbin.org/patch", data={'patch1':'data1', 'patch2':'data2'})
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
>>> r = requests.delete("http://httpbin.org/delete")
>>> r.request.method
'DELETE'
```

* * *

### ``OPTIONS``

> ``requests.options(url, **kwargs)``

options 메소드는 요청 시 OPTIONS 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.options("http://google.com")
>>> r.request.method
'OPTIONS'
```

* * *

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

text는 요청/응답 본문을 자동으로 디코드시킨 값을 str 타입으로 반환합니다.

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

content는 요청/응답 본문을 byte 타입으로 반환됩니다.

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

> ``json(self, **kwargs)``

json() 는 요청/응답 본문을 json 형식으로 디코딩하여 반환합니다.

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

### ``r.status_code``

``status_code``는 [http 응답 코드](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)를 나타냅니다. 요청에 성공한 경우 일반적으로 200을 반환 합니다.

```py
>>> r = requests.get("https://example.com")
>>> r.status_code
200
```

* * *

### ``r.url``

``url``은 요청한 뒤 응답의 최종 URL을 반환합니다.

URL redirection이 되는 경우에도 리다이렉션이 된 최종 URL을 출력합니다.

> Example Code

> ``http://127.0.0.1:8080/redirect`` -> ``/redirect_test``

```py
>>> r = requests.get("https://example.com")
>>> r.url
'https://example.com'
>>>
>>> r = requests.get("http://127.0.0.1:8080/redirect")
>>> r.url
'http://127.0.0.1:8080/redirect_test'
```

* * *

### ``r.history``

``history``는 모든 리다이렉션 응답은 가장 오래된 요청에서 최근 요청 순으로 Response 개체 목록을 반환 합니다.

> Example Code

> ``http://127.0.0.1:8080/redirect`` -> ``/redirect_test``

> ``http://127.0.0.1:8080/redirect2`` -> ``/redirect_test`` -> ``/redirect_test2``

```py
>>> r = requests.get("http://127.0.0.1:8080/redirect")
>>> r.history
[<Response [302]>]
>>> r.url
'http://127.0.0.1:8080/redirect_test'
>>> r = requests.get("http://127.0.0.1:8080/redirect2")
>>> r.history
[<Response [302]>, <Response [302]>]
>>> r.url
'http://127.0.0.1:8080/redirect_test2'
>>>
>>> r.history[0]
<Response [302]>
>>> r.history[0].url
'http://127.0.0.1:8080/redirect2'
>>>
>>> r.history[1]
<Response [302]>
>>> r.history[1].url
'http://127.0.0.1:8080/redirect_test'
>>> r.url
'http://127.0.0.1:8080/redirect_test2'
```

* * *

### ``r.links``

``links``는 요청/응답 [헤더의 link](https://tools.ietf.org/html/rfc8288#section-3)를 파싱한 결과를 반환합니다.

만약 존재하지 않는 경우 ``{}`` 빈 딕셔너리를 반환합니다.

> Example Code

```py
>>> r = requests.get("https://api.github.com/users/kennethreitz/repos?page=1&per_page=10")
>>> r.headers['link']
'<https://api.github.com/user/119893/repos?page=2&per_page=10>; rel="next", <https://api.github.com/user/119893/repos?page=5&per_page=10>; rel="last"'
>>> r.links
{'next': {'url': 'https://api.github.com/user/119893/repos?page=2&per_page=10', 'rel': 'next'}, 'last': {'url': 'https://api.github.com/user/119893/repos?page=5&per_page=10', 'rel': 'last'}}
>>>
>>>
>>> r = requests.get("https://example.com")
>>> r.headers['link']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3/dist-packages/requests/structures.py", line 52, in __getitem__
    return self._store[key.lower()][1]
KeyError: 'link'
>>> r.links
{}
```

* * *

### ``r.headers``

``headers``는 요청한 뒤 응답 헤더를 반환 합니다.

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r.headers
{'Content-Encoding': 'gzip', 'Age': '330304', 'Cache-Control': 'max-age=604800', 'Content-Type': 'text/html; charset=UTF-8', 'Date': 'Fri, 30 Apr 2021 13:58:41 GMT', 'Etag': '"3147526947+gzip"', 'Expires': 'Fri, 07 May 2021 13:58:41 GMT', 'Last-Modified': 'Thu, 17 Oct 2019 07:18:26 GMT', 'Server': 'ECS (sab/56BC)', 'Vary': 'Accept-Encoding', 'X-Cache': 'HIT', 'Content-Length': '648'}
```

* * *

### ``r.cookies``

``cookies``

> Example Code

```py
>>> r = requests.get("https://google.com")
>>> r.headers['set-cookie']
'1P_JAR=2021-04-30-14; expires=Sun, 30-May-2021 14:02:07 GMT; path=/; domain=.google.com; Secure, NID=214=V54rp0jqnDG7IFhEI8bUU1DhK8FERCtCfFYzPlPNdCgFLZTmwxQpUhUEzc5xtK_p_4BByikl28WX7558B2WWmY7iJPMMPiMmhnwvZbftcazRwyPLjDjgaA_3GBKRMkipp7qD0ONumogYbm9tbjaRCYjp08qNxfeDjOIgLiGSdaU; expires=Sat, 30-Oct-2021 14:02:07 GMT; path=/; domain=.google.com; HttpOnly'
>>>
>>> r.cookies
<RequestsCookieJar[Cookie(version=0, name='1P_JAR', value='2021-04-30-14', port=None, port_specified=False, domain='.google.com', domain_specified=True, domain_initial_dot=True, path='/', path_specified=True, secure=True, expires=1622383327, discard=False, comment=None, comment_url=None, rest={}, rfc2109=False), Cookie(version=0, name='NID', value='214=V54rp0jqnDG7IFhEI8bUU1DhK8FERCtCfFYzPlPNdCgFLZTmwxQpUhUEzc5xtK_p_4BByikl28WX7558B2WWmY7iJPMMPiMmhnwvZbftcazRwyPLjDjgaA_3GBKRMkipp7qD0ONumogYbm9tbjaRCYjp08qNxfeDjOIgLiGSdaU', port=None, port_specified=False, domain='.google.com', domain_specified=True, domain_initial_dot=True, path='/', path_specified=True, secure=False, expires=1635602527, discard=False, comment=None, comment_url=None, rest={'HttpOnly': None}, rfc2109=False)]>
>>>
>>> for key,value in r.cookies.items():
...     print(key, value)
...
1P_JAR 2021-04-30-14
NID 214=V54rp0jqnDG7IFhEI8bUU1DhK8FERCtCfFYzPlPNdCgFLZTmwxQpUhUEzc5xtK_p_4BByikl28WX7558B2WWmY7iJPMMPiMmhnwvZbftcazRwyPLjDjgaA_3GBKRMkipp7qD0ONumogYbm9tbjaRCYjp08qNxfeDjOIgLiGSdaU
```

* * *

### ``r.connection``

* * *

### ``r.elapsed``

``elapsed``는 요청을 보낸 후 응답이 도착할 때까지의 경과한 시간을 datetime.timedelta 객체로 반환합니다.

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r.elapsed
datetime.timedelta(microseconds=643703)
>>> r.elapsed.microseconds
643703
>>> print(r.elapsed)
0:00:00.643703
```

* * *

### ``r.is_permanent_redirect``

* * *

### ``r.is_redirect``

* * *

### ``r.ok``

``ok``는 요청/응답 코드가 200이면 True 아니면 False를 반환합니다.

``raise_for_status()``를 이용하여 예외처리로 True, False를 구별합니다.

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r.ok
True
>>> if r.ok:
...     print(1)
...
1
>>>
>>> r = requests.get("https://example.com/not-found/")
>>> r.ok
False
>>> if r.ok:
...     print(1)
...
>>>
```

* * *

### ``r.reason``

``reason``는 요청/응답 http 상태 코드의 텍스트를 출력합니다. 

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r.ok
True
>>> r.status_code
200
>>> r.reason
'OK'
>>>
>>> r = requests.get("https://example.com/a/")
>>> r.ok
False
>>> r.status_code
404
>>> r.reason
'Not Found'
```

* * *

### ``r.raise_for_status()``

``raise_for_status()``는 요청/응답 코드가 200이 아니면 예외를 발생시킵니다.

```py
>>> r = requests.put("https://google.com")
>>> r.raise_for_status()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3/dist-packages/requests/models.py", line 940, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 405 Client Error: Method Not Allowed for url: https://google.com/
>>> r = requests.get("https://example.com")
>>> r.raise_for_status()
>>> type(r.raise_for_status())
<class 'NoneType'>
>>> r = requests.get("https://example.com/a")
>>>
>>> r.raise_for_status()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3/dist-packages/requests/models.py", line 940, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 404 Client Error: Not Found for url: https://example.com/a
```

* * *

### ``r.encoding``

``encoding``는 요청/응답 헤더를 이용하여 데이터의 인코딩 방식을 추측하여 반환합니다.

```py
>>> r = requests.get("https://example.com")
>>> r.encoding
'UTF-8'
>>> r.headers
{ [ ... ] 'Content-Type': 'text/html; charset=UTF-8', 'Date': 'Thu, 29 Apr 2021 14:21 [ ... ] IT', 'Content-Length': '648'}
```

* * *

### ``r.apparent_encoding``

``apparent_encoding``는 ``chardet.detect``를 이용하여 자동으로 인코딩을 인식하고 반환합니다.

> apparent_encoding Function Code

```py
@property
    def apparent_encoding(self):
        """The apparent encoding, provided by the chardet library."""
        return chardet.detect(self.content)['encoding']
```

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r.apparent_encoding
'ascii'
```

* * *

### ``r.iter_content()``

> ``iter_content(self, chunk_size=1, decode_unicode=False)``

``iter_content()``는 요청/응답 데이터를 반복하여 대용량 응답을 위해 콘텐츠를 한 번에 메모리로 읽는 것을 방지하며 byte 타입으로 반환합니다.

chunk_size는 메모리로 읽어햐하는 바이트 수 입니다.

또한 decode_unicode가 True면 콘텐츠가 디코딩됩니다.

> Example Code

```py
>>> import requests
>>> r = requests.get("https://example.com", stream=True)
>>>
>>> r.iter_content()
<generator object Response.iter_content.<locals>.generate at 0x7fee03824890>
>>> content = r.iter_content()
>>> for i in content:
...     print(i)
...
b'<'
b'!d'
b'o'
b'ct'
[ ... ]
b'</'
b'body>\n</'
b'html>\n'
>>> r = requests.get("https://example.com", stream=True)
>>> for i in r.iter_content(chunk_size=128):
...     print(i)
...
b'<!doctype html>\n<html>\n<head>\n    <title>Example Domain</title>\n\n    <meta charset="utf-8" />\n    <meta http-'
b'equiv="Content-type" content="text/html; charset=utf-8" />\n    <meta name="viewport" content="width=device-width, initial-scale=1" />\n    <style type="text/css">\n    body {\n        background-color: #f0f0f2;\n        margin: 0;\n        padding: 0;\n        font-family: -ap'
b'ple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;\n        \n    }\n    div {\n        width: 600px;\n        margin: 5em auto;\n        padding: 2em;\n        background-color: #fdfdff;\n        border-radius: 0.5em;\n        box-shadow: 2px '
b'3px 7px 2px rgba(0,0,0,0.02);\n    }\n    a:link, a:visited {\n        color: #38488f;\n        text-decoration: none;\n    }\n    @media (max-width: 700px) {\n        div {\n            margin: 0 auto;\n            width: auto;\n        }\n    }\n    </style>    \n</head>\n\n<body>\n<div>\n    <h1>Example Domain</h1>\n    <p>This domain is for use '
b'in illustrative examples in documents. You may use this\n    domain in literature without prior coordination or asking for permission.</p>\n    <p><a href="https://www.iana.org/domains/example">More information...</a></p>\n</div>\n</body>\n</html>\n'
>>> r = requests.get("https://example.com", stream=True)
>>> content = r.iter_content(chunk_size=128, decode_unicode=True)
>>> for i in content:
...     print(i)
...
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    [ ... ]
in illustrative examples in documents. You may use this
    domain in literature without prior coordination or asking for permission.</p>
    <p><a href="https://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>
```

* * *

### ``r.iter_lines()``

> ``ITER_CHUNK_SIZE = 512``

> ``iter_lines(self, chunk_size=ITER_CHUNK_SIZE, decode_unicode=False, delimiter=None)``

``iter_lines()``는 ``iter_content()``를 이용하여 응답 데이터를 한 번에 한 줄씩 반복하면서 대용량 응답을 위해 콘텐츠를 한 번에 메모리로 읽는 것을 방지합니다.

> iter_lines() Function Code

```py
def iter_lines(self, chunk_size=ITER_CHUNK_SIZE, decode_unicode=False, delimiter=None):
    ...
    for chunk in self.iter_content(chunk_size=chunk_size, decode_unicode=decode_unicode):
        ...
        if delimiter:
                lines = chunk.split(delimiter)
            else:
                lines = chunk.splitlines()
        ...
```

> Example Code

```py
>>> r = requests.get("https://example.com", stream=True)
>>> lines = r.iter_lines()
>>> for i in lines:
...     print(i)
...
b'<!doctype html>'
b'<html>'
b'<head>'
b'    <title>Example Domain</title>'
b''
b'    <meta charset="utf-8" />'
[ ... ]
b'<div>'
b'    <h1>Example Domain</h1>'
b'    <p>This domain is for use in illustrative examples in documents. You may use this'
b'    domain in literature without prior coordination or asking for permission.</p>'
b'    <p><a href="https://www.iana.org/domains/example">More information...</a></p>'
b'</div>'
b'</body>'
b'</html>'
>>> r = requests.get("https://example.com", stream=True)
>>> lines = r.iter_lines(delimiter = b'<')
>>> for i in lines:
...     print(i)
...
b''
b'!doctype html>\n'
b'html>\n'
b'head>\n    '
b'title>Example Domain'
[ ... ]
b'/div>\n'
b'/body>\n'
b'/html>\n'
```

* * *

### ``r.close()``

``close()``는 서버와의 연결을 닫습니다.

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r
<Response [200]>
>>> r.close
>>> r.close()
```

* * *

### ``r.request``

* * *

### ``r.raw``

서버에서 원시 소켓 응답을 받기 위해 ``r.raw.*``를 사용하기 위해서는 요청 시 ``stream=True``를 추가해줘야 합니다.

> Example Code

```py
>>> r = requests.get("https://example.com", stream=True)
>>> r.raw
<urllib3.response.HTTPResponse object at 0x7f53bba651f0>
```

* * *

#### ``r.raw.read()``

> ``read(self, amt=None, decode_content=None, cache_content=False)``

``r.raw.read()``함수를 이용하여 응답 본문 컨텐츠를 원하는 만큼 인코딩된 값을 출력할 수 있습니다.

해당 기능은 open.read 함수와 유사합니다.

> Example Code

```py
>>> r = requests.get("https://example.com" ,stream = True)
>>> r.raw
<urllib3.response.HTTPResponse object at 0x7f8f692dc8b0>
>>> r.raw.read()
b'\x1f\x8b\x08\x00\xc2\x15\xa8]\x00\x03}TMs\xdb \x10\xbd\xfbWl\xd5K2#$\'i\x1a\x8f-i\xfa\x99i\x0fi\x0fi\x0f=\x12\xb1\xb2\x98\x08P\x01\xc9\xf6t\xf2\xdf\xbbB\x8e#7\x99\x9a\x91[ ... ]d0x\x11\x10\xb34\x88\x93\xa5{\xa9\xd2\xf1A\xfb\x0b(\xeb|o\xe8\x04\x00\x00'
>>> r.raw.read(10)
b''
>>> r = requests.get("https://example.com", stream=True)
>>>
>>> r.raw.read(10)
b'\x1f\x8b\x08\x00\xc2\x15\xa8]\x00\x03'
>>> r.raw.read(10)
b'}TMs\xdb \x10\xbd\xfbW'
```

# 현재 작성중...