---
layout: post
title:  "Python requests 모듈(module) 사용법"
tags: [python,requests,Guide]
excerpt: "Python requests 모듈(module) 사용법"
---
---

{% comment %}

# Contents

---

+ [Contents](#contents)
+ [requests 모듈이란?](#requests-모듈이란)
    + [이 글을 작성한 이유](#이-글을-작성한-이유)
    + [References](#references)
+ [requests module FILE 구조](#requests-module-file-구조)
+ [requests module install](#requests-module-install)
+ [requests Example Code](#requests-example-code)
+ [HTTP 요청 메서드(HTTP request method)](#http-요청-메서드http-request-method)
+ [requests GET, POST, PUT , DELETE, HEAD, OPTIONS](#requests-get-post-put--delete-head-options)
+ [requests 모듈 사용법](#requests-모듈-사용법)
    + [Request](#request)
        + [Request Header](#request-header)
        + [Request method](#request-method)
            + [간단히](#간단히)
                + [parameter](#parameter)
                    + [url](#url-필수-)
                    + [params](#params선택-사항)
                    + [data](#data선택-사항)
                    + [json](#json선택-사항)
                    + [**kwargs](#kwargs-선택-사항-)
                    + [**kwargs 매개변수 종류](#kwargs-매개변수-종류)
                    + [return](#return)
                + [PUT](#put)
                + [GET](#get)
                + [POST](#post)
                + [HEAD](#head)
                + [PATCH](#patch)
                + [DELETE](#delete)
                + [OPTIONS](#options)
            + [자세히](#자세히)
                + [PUT](#put-1)
                + [GET](#get-1)
                + [POST](#post-1)
                + [HEAD](#head-1)
                + [PATCH](#patch-1)
                + [DELETE](#delete-1)
                + [OPTIONS](#options-1)
        + [request.request](#requestsrequest)
        + [Request **kwargs](#request-kwargs)
            + [Request **kwargs 종류](#request-kwargs-종류-1)
                + [method( str )](#method-str-)
                + [url( str )](#url-str-)
                + [params( str, dict )](#params-str-dict-)
                + [data( str, dict )](#data-str-dict-)
                + [headers( dict )](#headers-dict-)
                + [cookies( dict )](#cookies-dict)
                + [files( dict, list, tuple )](#files-dict-list-tuple-)
                + [auth( tuple, list )](#auth-tuple-list-)
                + [timeout( float, int )](#timeout-float-int-)
                + [allow_redirects( bool )](#allow_redirects-bool-)
                + [proxies( dict )](#proxies-dict-)
                + [hooks( Function, list )](#hooks-function-list-)
                + [stream( bool )](#stream-bool-)
                + [verify( bool, str )](#verify-bool-str-)
                + [cert( str, tuple )](#cert-str-tuple-)
                + [json( dict )](#json-dict-)

    + [Response](#response)
        + [r.text](#rtext)
        + [r.content](#rcontent)
        + [r.json()](#rjson)
        + [r.status_code](#rstatus_code)
        + [r.url](#rurl)
        + [r.history](#rhistory)
        + [r.links](#rlinks)
        + [r.headers](#rheaders)
        + [r.cookies](#rcookies)
        + [r.connection](#rconnection)
        + [r.elapsed](#relapsed)
        + [r.is_permanent_redirect](#ris_permanent_redirect)
        + [r.is_redirect](#ris_redirect)
        + [r.ok](#rok)
        + [r.reason](#rreason)
        + [r.raise_for_status()](#rraise_for_status)
        + [r.encoding](#rencoding)
        + [r.apparent_encoding](#rapparent_encoding)
        + [r.iter_content()](#riter_content)
        + [r.iter_lines()](#riter_lines)
        + [r.close()](#rclose)
        + [r.request](#rrequest)
            + [r.request.method](#rrequestmethod)
            + [r.request.path_url](#rrequestpath_url)
            + [r.request.url](#rrequesturl)
            + [r.request.headers](#rrequestheaders)
            + [r.request._cookies](#rrequest_cookies)
            + [r.request.copy()](#rrequestcopy)

        + [r.raw](#rraw)
            + [r.raw.read()](#rrawread)

+ [번외...](#번외)
    + [request_short_code.py](#request_short_codepy)
    + [requests_module_code.py](#requests_module_codepy)
+ [작성 후기](#작성-후기)

---

{% endcomment %}

# requests 모듈이란?

requests는 python사용자들을 위해 만들어진 간단한 Python용 HTTP 라이브러리이며, 간단하게는 HTTP, HTTPS 웹 사이트에 요청하기 위해 자주 사용되는 모듈 중 하나이고

Crawling 과정에서 requests모듈을 이용해 웹 사이트의 소스코드를 가져온 다음 파싱을 하는 경우가 많습니다.

requests 코드 예시를 위해 [httpbin](http://httpbin.org/), [example](https://example.com), [google](https://www.google.com) 3개의 사이트를 이용했습니다.

### 이 글을 작성한 이유

그냥 python 쓰면서 requests 모듈을 쓰게 되는 경우가 많은데

내가 모르는 기능이 있는지 궁금하기도 하고 심심해서 한번 requests모듈 분석했습니다.

#### References

+ [requests Github](https://github.com/psf/requests)
+ [requests Documentation](https://docs.python-requests.org/)

---

## requests module FILE 구조

```bash
requests
   ├── __version__.py
   ├── _internal_utils.py
   ├── adapters.py
   ├── api.py
   ├── auth.py
   ├── certs.py
   ├── compat.py
   ├── cookies.py
   ├── exceptions.py
   ├── help.py
   ├── hooks.py
   ├── models.py
   ├── packages.py
   ├── sessions.py
   ├── status_codes.py
   ├── structures.py
   └── utils.py
```

---

### requests module install

pip 명령어를 이용하여 requests 모듈을 다운로드 하면 import 또는 from 으로 사용할 수 있습니다

```py
~$ pip install requests
~$ pip3 install requests
```

## requests Example Code

하단의 코드는 requests 모듈을 이용하여 ``https://example.com`` URL에 요청한 다음 반환되는 값을 출력한 코드입니다.

``[ ... ]`` = 부분 생략

> Example Code

```py
>>> import requests
>>> r = requests.get("https://example.com/")
>>> r.status_code
200
>>> r.text
'<!doctype html>\n<html>\n<head>\n    <title>Example Domain</title[ ... ]\n</body>\n</html>\n'
```

---

## HTTP 요청 메서드(HTTP request method)

http 메서드는 클라이언트가 서버에게 요청하는 목적 및 그 종류를 알리는 수단입니다.

또한 요청 메서드냐에 따라 요청/응답하는 방식 또한 다르며 다양한 메서드가 존재합니다.

대표적으로 쓰이는 메서드는 GET, POST 방식입니다.

#### References

+ [HTTP method Documentation](https://tools.ietf.org/html/rfc2616#section-9.1.2)

---

## requests GET, POST, PUT , DELETE, HEAD, OPTIONS

HTTP 메소드마다 사용되는 함수와 매개변수가 각각 조금씩 다르며 일반적인 요청의 경우 함수명만 변경하여 쉽게 할 수 있습니다.

그리고 요청의 모든 기능은 하단의 코드 예시와 같이 7가지 방법으로 액세스할 수 있습니다.

모두 Response 개체의 인스턴스를 반환합니다.

---

> Example Code

```py
>>> r = requests.get("http://httpbin.org/get")
>>> r
<Response [200]>
>>> r = requests.post("http://httpbin.org/post")
>>> r = requests.put("http://httpbin.org/put")
>>> r = requests.head("http://httpbin.org/get")
>>> r = requests.patch("http://httpbin.org/patch")
>>> r = requests.delete("http://httpbin.org/delete")
>>> r = requests.options("http://httpbin.org/get")
```

---

# requests 모듈 사용법

requests는 from 또는 import로 모듈 불러오면 사용할 수 있습니다.

```py
>>> from requests import *
>>> import requests
```

---

# Request

> ``requests.request(method, url, **kwargs)``

요청의 모든 기능은 7가지 방법으로 액세스할 수 있습니다.

> request.request Function Code

```py
def request(method, url, **kwargs):
    [ ... ]
    with sessions.Session() as session:
        return session.request(method=method, url=url, **kwargs)
```

---

### Request Headers

requests 모듈은 [requests.utils.default_headers](https://github.com/psf/requests/blob/main/requests/utils.py#L808-L826) 함수에 의해 새로운 요청 시 기본값으로 Header 4개가 포함됩니다.

> default_headers Function Code

```py
def default_user_agent(name="python-requests"):
    """
    Return a string representing the default user agent.
    :rtype: str
    """
    return '%s/%s' % (name, __version__)


def default_headers():
    """
    :rtype: requests.structures.CaseInsensitiveDict
    """
    return CaseInsensitiveDict({
        'User-Agent': default_user_agent(),
        'Accept-Encoding': ', '.join(('gzip', 'deflate')),
        'Accept': '*/*',
        'Connection': 'keep-alive',
    })
```

> Example Code

```py
>>> r = requests.get("https://example.com")
>>> r.request.headers
{'User-Agent': 'python-requests/2.22.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}
```

<br>

---

### Request method

requests 클래스에서 지원하는 요청 메서드를 쉽게 사용하기 위해선 총 7가지의 메소드를 이용하여 활용할 수 있습니다.

``HTTP/1.1`` 버전의 경우 여러 가지의 메소드가 존재하는데

requests 모듈은 ``[PUT, GET, POST, HEAD, PATCH, DELETE, OPTIONS]`` 메서드가 존재합니다.

그리고 위의 7가지 메소드는 전부 ``requests.request``로 연결됩니다.

예시로는 많이 쓰이는 requests.get 메소드 또한 ``requests.request``를 사용합니다.

> requests.get Function Code

```py
def get(url, params=None, **kwargs):
    [ ... ]
    kwargs.setdefault('allow_redirects', True)
    return request('get', url, params=params, **kwargs)
```

---

### 간단히

+ ##### parameter

    + ##### url( 필수 )

        url 매개변수는  ``requests.request`` 객체에 사용되기 위한 URL입니다.

        > https://google.com

    + ##### params(선택 사항)

        튜플(tuple), 딕셔너리(dict)형식으로 매개변수에 넣으면 양식이 URL 인코딩이 되어 URL에 추가됩니다.

        > ``URL?key=value&key1=value1``

    + ##### data(선택 사항)

        튜플(tuple), 딕셔너리(dict)형식으로 매개변수에 넣으면 양식이 인코딩되어 요청 본문에 추가됩니다.

        > ``key=value&key1=value1``

    + ##### json(선택 사항)

        JSON 매개변수를 이용하여 요청 본문에 json 형식으로 추가됩니다.

        > ``{ 'key':'value', 'key1':'value1' }``

    + ##### **kwargs( 선택 사항 )

        ``**kwargs``는 요청하기 위한 매개변수이며 [requests.sessions.Session.request](https://docs.python-requests.org/en/latest/_modules/requests/sessions/#Session.request)로 연결되어 처리됩니다.

        [자세한 **kwargs 매개변수 사용법은 여기를 참고하면 됩니다.](#request-kwargs)


    + ##### **kwargs 매개변수 종류

        > ``request(self, method, url, params=None, data=None, headers=None, cookies=None, files=None, auth=None, timeout=None, allow_redirects=True, proxies=None, hooks=None, stream=None, verify=None, cert=None, json=None)``
      
    + ##### return

        ``[PUT, GET, POST, HEEAD, PATCH, DELETE, OPTIONS]``는 기본적으로 [requests.modules.Response](https://docs.python-requests.org/en/master/api/#requests.Response) 객체를 반환합니다.

---

+ ##### PUT

> ``requests.put(url, data=None, **kwargs)`` [[_Source Code_]](https://github.com/psf/requests/blob/main/requests/api.py#L120-L132)

> requests.put Function Code

```py
def put(url, data=None, **kwargs):
    [ ... ]
    return request('put', url, data=data, **kwargs)
```

---

+ ##### GET

> ``requests.get(url, params=None, **kwargs)`` [[_Source Code_]](https://github.com/psf/requests/blob/main/requests/api.py#L64-L75)

> requests.get Function Code

```py
def get(url, params=None, **kwargs):
    [ ... ]
    kwargs.setdefault('allow_redirects', True)
    return request('get', url, params=params, **kwargs)
```

---

+ ##### POST

> ``requests.post(url, data=None, json=None, **kwargs)`` [[_Source Code_]](https://github.com/psf/requests/blob/main/requests/api.py#L105-L117)

> request.post Function Code

```py
def post(url, data=None, json=None, **kwargs):
    [ ... ]
    return request('post', url, data=data, json=json, **kwargs)
```

---

+ ##### HEAD

> ``requests.head(url, **kwargs)`` [[_Source Code_]](https://github.com/psf/requests/blob/main/requests/api.py#L90-L102)

> requests.head Function Code

```py
def head(url, **kwargs):
    [ ... ]
    kwargs.setdefault('allow_redirects', False)
    return request('head', url, **kwargs)
```

---

+ ##### PATCH

> ``requests.patch(url, data=None, **kwargs)`` [[_Source Code_]](https://github.com/psf/requests/blob/main/requests/api.py#L135-L147)

> requests.patch Function Code

```py
def patch(url, data=None, **kwargs):
    [ ... ]
    return request('patch', url, data=data, **kwargs)
```

---

+ ##### DELETE

> ``requests.delete(url, **kwargs)`` [[_Source Code_]](https://github.com/psf/requests/blob/main/requests/api.py#L150-L159)

> requests.delete Function Code

```py
def delete(url, **kwargs):
    [ ... ]
    return request('delete', url, **kwargs)
```

---

+ ##### OPTIONS

> ``requests.options(url, **kwargs)`` [[_Source Code_]](https://github.com/psf/requests/blob/main/requests/api.py#L78-L87)

```py
def options(url, **kwargs):
    [ ... ]
    kwargs.setdefault('allow_redirects', True)
    return request('options', url, **kwargs)
```

---

## 자세히

---

### ``PUT``

> ``requests.put(url, data=None, **kwargs)``

put 메소드는 요청 시 PUT 방식으로 요청되며 data 매개변수를 지원합니다.

> Example Code

```py
>>> r = requests.put("http://httpbin.org/put", data={'put1':'data1', 'put2':'data2'})
>>> r.request.method
'PUT'
>>> r.request.body
'put1=data1&put2=data2'
```

---

### ``GET``

> ``requests.get(url, params=None, **kwargs)``

get 메소드는 요청 시 GET 방식으로 요청되며 ``params`` 매개변수를 지원합니다.

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

---

### ``POST``

> ``requests.post(url, data=None, json=None, **kwargs)``

post 메소드는 요청 시 POST 방식으로 요청되며 ``data``, ``json`` 매개변수가 존재합니다.

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

---

### ``HEAD``

> ``requests.head(url, **kwargs)``

head 메소드는 요청 시 HEAD 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.head("https://www.google.com")
>>> r.request.method
'HEAD'
```

---

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

---

### ``DELETE``

> ``requests.delete(url, **kwargs)``

delete 메소드는 요청 시 DELETE 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.delete("http://httpbin.org/delete")
>>> r.request.method
'DELETE'
```

---

### ``OPTIONS``

> ``requests.options(url, **kwargs)``

options 메소드는 요청 시 OPTIONS 방식으로 요청됩니다.

> Example Code

```py
>>> r = requests.options("http://google.com")
>>> r.request.method
'OPTIONS'
```

---

## ``requests.request``

+ [Core Code](https://github.com/psf/requests/blob/main/requests/api.py#L16-L61)

``requests.request``는 요청하기 위해 사용되는 메소드이며 Response 개체를 반환합니다.

자주 사용되는 메소드중 ``requests.get``, ``requests.post`` 또한 requests.request를 호출합니다.

```py
>>> r = requests.request('GET', 'https://example.com')
>>> r
<Response [200]>
>>> r.status_code
200
```

만약 method를 문자열로 받아 자유롭게 요청해야되는 경우 아래와 같이 변수에 원하는 메서드를 넣은 다음 자유롭게 요청할 수 있습니다.

```py
>>> methods = ['GET','POST']
>>> r = requests.request(methods[0], 'http://httpbin.org/get', params={'test':'GET'})
>>> r
<Response [200]>
>>>
>>> r = requests.request(methods[1], 'http://httpbin.org/post', data={'test':'POST'})
>>> r
<Response [200]>
>>>
```

---

## Request **kwargs

``**kwargs``는 위에서 한번 언급했던 [**kwargs](#kwargs-선택-사항-)를 설명합니다.

예로 [위의 글에서](#request-method)다뤘던 requests 메소드 7개 매개변수 또한 **kwargs가 존재합니다.

---

### **kwargs 매개변수 종류

> ``request(self, method, url, params=None, data=None, headers=None, cookies=None, files=None, auth=None, timeout=None, allow_redirects=True, proxies=None, hooks=None, stream=None, verify=None, cert=None, json=None)``

+ #### method( str )

  method 매개변수는 요청 시 사용될 http 메소드 입니다.

  GET 또는 POST등을 넣으면 됩니다.

  > Example Code

  ```py
  >>> r = requests.request(method = 'GET', url = 'https://example.com')
  >>> r
  <Response [200]>
  >>> r = requests.request(method = 'PUT', url = 'http://httpbin.org/put')
  <Resoinse [200]>
  ```

---

+ #### url( str )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L366-L450)

    url 매개변수는 요청하고 싶은 URL을 넣으면 됩니다.

    > Example Code

    ```py
    >>> r = requests.request('GET', url='https://example.com')
    <Response [200]>
    ```

---

+ #### params( str, dict )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L439-L447)

    params 매개변수는 요청하는 URL뒤에 GET방식으로 파라미터가 붙습니다.

    > Example Code

    ```py
    >>> r = requests.request('GET', url='https://example.com', params={'get1':'value1',   'get2','value2'})
    <Response [200]>
    >>> r.url
    'https://example.com?get=value1&get2=value2'
    >>> r = requests.get("https://www.google.com", params="helloworld")
    >>> r.url
    'https://www.google.com/?helloworld'
    ```

---

+ #### data( str, dict )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L522-L527)

    data 매개변수는 요청될때 본문에 포함되어 서버로 데이터를 전송합니다.

    data 매개변수에 dict 또는 문자열 그대로 담아 요청을 할 수 있습니다.

    > Example Code

    ```py
    >>> r = requests.request('POST', url='http://httpbin.org/post', data={'post1':'value1', 'post2':'value2'})
    >>> r
    <Response [200]>
    >>> r.request.body
    'post1=value1&post2=value2'
    >>> r = requests.post('http://httpbin.org/post', data="hello post data")
    >>> r.request.body
    'hello post data'
    ```

---

+ #### headers( dict )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L452-L461)

    headers 매개변수는 요청할때 기본적인 헤더에 추가/수정/편집하여 서버에 전송합니다.

    > Example Code

    ```py
    >>> r = requests.request('GET', url='http://httpbin.org/get', headers={'header_test':'test'})
    >>> r.request.headers
    {'User-Agent': 'python-requests/2.25.1', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive', 'header_test': 'test'}
    >>> r.request.headers['header_test']
    'test'
    ```

---

+ #### cookies( dict )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L572-L590)

    cookies 매개변수는 요청할때 헤더에 쿠키에 대한 정보를 포함시킵니다.

    ```py
    >>> r = requests.request('GET', url='https://example.com', cookies={'cookies1':'value1'})
    >>> r.request._cookies
    <RequestsCookieJar[Cookie(version=0, name='cookies1', value='value1', port=None, port_specified=False, domain='', domain_specified=False, domain_initial_dot=False, path='/', path_specified=True, secure=False, expires=None, discard=True, comment=None, comment_url=None, rest={'HttpOnly': None}, rfc2109=False)  ]>
    >>> r.request._cookies.get_dict()
    {'cookies1': 'value1'}
    ```

---

+ #### files( dict, List, tuple )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L118-L179)

    files 매개변수는 요청할 때 본문에 파일 내용을 포함시켜 파일 업로드하는 기능으로 사용할 수 있습니다.

    requests를 이용하여 파일 업로드를 하기 위해 로컬에서 환경 구축한 다음 files매개변수를 이용하여 업로드를 했습니다.

    > 127.0.0.1:8080/uploads Code

    ```py
    from flask import Flask, request
    from werkzeug.utils import secure_filename

    app = Flask(__name__)

    @app.route('/uploads', methods=['POST'])
    def FILE_Uploads():
        FILE = request.files['file']
        FILE.save(os.path.join('uploads/', secure_filename(FILE.filename)))

        return 'FILE Uploads Save'

    app.run('127.0.0.1', 8080)
    ```

    ---

    > Example Code

    ```py
    >>> files = {'file':open('test.txt', 'rb')}
    >>> url = 'http://127.0.0.1:8080/uploads'
    >>> r = requests.post(url, files=files)
    >>> r.request.body
    b'--97d3077d15f7c7f9fc153b18d4575e58\r\nContent-Disposition: form-data; name="flie"; filename="test.txt"\r\n\r\nFILE Uploads TEST!!\n\r\n--97d3077d15f7c7f9fc153b18d4575e58--\r\n'
    >>> print(r.request.body.decode())
    --97d3077d15f7c7f9fc153b18d4575e58
    Content-Disposition: form-data; name="flie"; filename="test.txt"

    FILE Uploads TEST!!

    --97d3077d15f7c7f9fc153b18d4575e58--

    >>>
    ```

    또한 Content-Type까지 지정해줄 수 있습니다. ``(Filename, File Content, File Content type)``

    ```py
    >>> import requests
    >>> files = {'files':('filename', 'file content', 'content-type')}
    >>> r = requests.post("http://httpbin.org/post", files=files)
    >>> print(r.json())
    {'args': {}, 'data': '', 'files': {'files': 'file content'}, 'form': {}, 'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Content-Length': '185', 'Content-Type': 'multipart/form-data;    boundary=2dddfaca24abc6196d17482a29880613', 'Host': 'httpbin.org', 'User-Agent': 'python-requests/2.22.0', 'X-Amzn-Trace-Id': 'Root=1-6197c5ce-14e0abef60bf13d62a2d9f36'}, 'json': None, 'origin': '112.154.52.33',    'url': 'http://httpbin.org/post'}
    >>> print(r.request.body.decode())
    --2dddfaca24abc6196d17482a29880613
    Content-Disposition: form-data; name="files"; filename="filename"
    Content-Type: content-type

    file content
    --2dddfaca24abc6196d17482a29880613--
    >>>
    >>> files = {'files':('test.html', open('test.html', 'rb'), 'text/html')}
    >>> r = requests.post("http://httpbin.org/post", files=files)
    >>> print(r.request.body.decode())
    --3cc6772156f774d6c834d9751d35ef44
    Content-Disposition: form-data; name="files"; filename="test.html"
    Content-Type: text/html

    <h1>html</h1>

    --3cc6772156f774d6c834d9751d35ef44--

    >>>
    ```

    다중 파일 업로드를 아는 경우 아래와 같은 코드를 실행하면 됩니다

    ```py
    >>> files = {'files1':open("test1.txt", "rb"), 'files2':open("test2.txt", "rb")}
    >>> r = requests.post("http://httpbin.org/post", files=files)
    >>> print(r.request.body.decode())
    --fdd0472fb8974f00fd2f21306e5711b9
    Content-Disposition: form-data; name="files1"; filename="files1"

    asdasd first file upload
    --fdd0472fb8974f00fd2f21306e5711b9
    Content-Disposition: form-data; name="files2"; filename="files2"

    asdasd second file upload
    --fdd0472fb8974f00fd2f21306e5711b9--

    ```


---

+ #### auth( tuple, list )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/auth.py#L28-L69)

    auth 매개변수는 [Authorization 헤더](https://tools.ietf.org/html/rfc7235#section-4.2)를 생성시켜 사용자 에이전트임을 증명할 수 있습니다.

    만약 authorization 헤더가 필요한 웹 사이트의 경우 auth를 이용하여 ``Basic <base64>`` 형식으로 변환시켜 요청이 됩니다

    > auth Code

    ```py
    class HTTPBasicAuth(AuthBase):
        """Attaches HTTP Basic Authorization to the given Request object."""

        def __init__(self, username, password):
            self.username = username
            self.password = password

        def __eq__(self, other):
            return all([
                self.username == getattr(other, 'username', None),
                self.password == getattr(other, 'password', None)
            ])

        def __ne__(self, other):
            return not self == other

        def __call__(self, r):
            r.headers['Authorization'] = _basic_auth_str(self.username, self.password)
            return r
    ```

    ---

    > Example Code

    ```py
    >>> r = requests.get("https://example.com", auth=("admin","pass"))
    >>> r.request.headers
    {'User-Agent': 'python-requests/2.22.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive', 'Authorization': 'Basic     YWRtaW46cGFzcw=='}
    >>> r.request.headers['Authorization']
    'Basic YWRtaW46cGFzcw=='
    >>> r = requests.get("https://example.com", auth=("admin","pass","test"))
    Traceback (most recent call last):
      [ ... ]
    TypeError: 'tuple' object is not callable
    >>> r = requests.get("https://example.com", auth=("admin"))
    Traceback (most recent call last):
      [ ... ]
    TypeError: 'str' object is not callable
    ```

---

+ #### timeout( float, int )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L423-L451)

    timeout 매개변수는 요청 시간을 제한 시킵니다.

    만약 응답을 받아오는데 10초 걸리는 사이트의 경우 timeout으로 빠르게 예외시킬 수 있습니다.

    요청하고 응답 받는데 timeout에서 설정한 시간을 초과하면 ``requests.exceptions.ReadTimeout`` 에러가 발생합니다.

    > 127.0.0.1:8080/time Code

    ```py
    import time
    from flask import Flask

    app = Flask(__name__)

    @app.route('/time')
    def time():
        sleep(10)
        return 'TIEM Loading'

    app.run('127.0.0.1', 8080)
    ```

    ---

    > Example Code

    ```py
    >>> r = requests.get("http://127.0.0.1:8080/time", timeout=2)
    Traceback (most recent call last):
      File "/usr/li [ ... ]"
        return self._sock.recv_into(b)
    socket.timeout: timed out

    During handling of the above exception, another exception occurred:

    Traceback (most recent call last):
      File "/usr[ ... ]" 
    urllib3.exceptions.ReadTimeoutError: HTTPConnectionPool(host='127.0.0.1', port=8080): Read timed out. (read timeout=2)

    During handling of the above exception, another exception occurred:

    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "/usr/lib[ ... ]"
        raise ReadTimeout(e, request=request)
    requests.exceptions.ReadTimeout: HTTPConnectionPool(host='127.0.0.1', port=8080): Read timed out. (read timeout=2)
    >>> r
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'r' is not defined
    ```

---

+ #### allow_redirects( bool )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/sessions.py#L680-L684)

    allow_redirects 매개변수는 요청하고 응답을 받는 과정에 리다이렉션을 허용하지않게 할 수 있습니다.

    만약 False로 설정하면 리다이렉션이 되지 않습니다.

    로컬에서 Flask 서버를 이용하여 리다이렉션 테스트를 했습니다.

    > 127.0.0.1:8080/redirect/n Code

    ```py
    from flask import Flask, redirect

    app = Flask(__name__)

    @app.route('/redirect/<int:n>')
    def redirects(n):
        return (redirect(f'/redirect/{n-1}', code=302) if n >= 1 else 'redirect TEST end')

    app.run('127.0.0.1', 8080)
    ```
  
    ---

    > Example Code

    ```py
    >>> r = requests.get("http://127.0.0.1:8080/redirect/3", allow_redirects=False)
    >>> r.url
    'http://127.0.0.1:8080/redirect/3'
    >>> r.history
    []
    >>> r = requests.get("http://127.0.0.1:8080/redirect/3", allow_redirects=True)
    >>> r.url
    'http://127.0.0.1:8080/redirect/0'
    >>> r.history
    [<Response [302]>, <Response [302]>, <Response [302]>]
    >>> for History in r.history:
    ...      History.url
    ...
    'http://127.0.0.1:8080/redirect/3'
    'http://127.0.0.1:8080/redirect/2'
    'http://127.0.0.1:8080/redirect/1'
    ```
    
---

+ #### proxies( dict )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/utils.py#L811-L834)

    proxies 매개변수는 proxy 기능을 사용할 수 있습니다.

    만약 requests 모듈로 요청하는걸 proxy로 잡고싶은 경우 porixes 매개변수를 사용하면 됩니다.

    > Example Code 1

    ```py
    import requests

    URL = 'http://httpbin.org'
    HTTP_PROXY = 'http://127.0.0.1:8080'

    PROXY_origin = {
        'http':HTTP_PROXY
    }

    r = requests.get(URL, proxies=PROXY_origin)
    print(r.text)
    ```

    해당 소스코드를 이용하여 ``http://127.0.0.1:8080``으로 프록시를 잡은 다음 proxies 매개변수에 dict 타입으로 호출하면

    테스트 하기 위해 Burp Suite툴을 이용하여 프록시 잡은 헤더를 살펴보면

    ```http
    GET / HTTP/1.1
    Host: httpbin.org
    User-Agent: python-requests/2.22.0
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: close


    ```

    이렇게 헤더와 본문이 나오는것을 볼 수 있습니다.

    만약 cookies나 headers를 추가하면 proxy에 잡히는것을 볼 수 있습니다.

    ---

    > Example Code 2

    ```py
    >>> headers = { 'test-headers':'Proxies' }
    >>> cookies = { 'cookie1':'value1' }
    >>> proxies = { 'http':'http://127.0.0.1:8080' }
    >>> r = requests.get("http://httpbin.org", headers=headers, cookies=cookies, proxies=proxies)
    ```

    ```
    GET / HTTP/1.1
    Host: httpbin.org
    User-Agent: python-requests/2.25.1
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: close
    test-headers: Proxies
    Cookie: cookie1=value1


    ```

    만약 별도의 설정 없이 컴퓨터의 환경 변수를 이용하여 모든 요청을 프록시로 잡을 수 있습니다.

    linux의 경우 export 명령어를 이용하여 환경 변수를 추가할 수 있습니다.

    ```bash
    ~$ export HTTP_PROXY = 'http://127.0.0.1:8080'
    ~$ export HTTPS_PROXY = 'http://127.0.0.1:8080'
    ```
    
---

+ #### hooks( Function, list )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/hooks.py#L14-L34)

    hooks는 요청을 한 뒤 ``{'reponse':value}``의 response Key와 매칭하는 value 부분의 함수 또는 여러 함수가 들어있는 리스트를 실행시킵니다.

    hooks 매개변수를 이용하여 응답 처리를 저작할 수 있습니다.

    hooks 매개변수의 경우 요청한 뒤 응답이 올 때 마다 default_hooks 실행된 다음 dispatch_hook 함수가 실행이 됩니다.

    ```py
    # requests/hooks.py
    HOOKS = ['response']

    def default_hooks():
        a = {event: [] for event in HOOKS}
        print(a)
        return a

    # TODO: response is the only one

    def dispatch_hook(key, hooks, hook_data, **kwargs):
        """Dispatches a hook dictionary on a given piece of data."""
        hooks = hooks or {}
        hooks = hooks.get(key)
        if hooks:
            if hasattr(hooks, '__call__'):
                hooks = [hooks]
            for hook in hooks:
                _hook_data = hook(hook_data, **kwargs)
                if _hook_data is not None:
                    hook_data = _hook_data
        return hook_data
    ```

    hooks 매개변수를 사용하기 위해서는 기본적으로 함수 또는 여러 함수 정보들이 들어있는 리스트 요소를 response 키에 매치되는 Dict 타입으로 삽입해야됩니다.

    > Example Code ( def, lambda / function )

    ```py
    >>> import requests
    >>> def response_class(res, **kwargs):
    ...     print(f"kwargs : {kwargs}")
    ...     print(f"res : {res}")
    ...     print(f"res.status_code : {res.status_code}")
    ...     print(f"res type : {type(res)}")
    ...     print(f"res dir : {dir(res)}")
    ...
    >>> r = requests.get("https://www.google.com", hooks={'response':response_class})
    kwargs : {'timeout': None, 'verify': True, 'proxies': OrderedDict(), 'stream': False, 'cert': None}
    res : <Response [200]>
    res.status_code : 200
    res type : <class 'requests.models.Response'>
    res dir : ['__attrs__', '__bool__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__enter__', '__eq__', '__exit__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__nonzero__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_content', '_content_consumed', '_next', 'apparent_encoding', 'close', 'connection', 'content', 'cookies', 'elapsed', 'encoding', 'headers', 'history', 'is_permanent_redirect', 'is_redirect', 'iter_content', 'iter_lines', 'json', 'links', 'next', 'ok', 'raise_for_status', 'raw', 'reason', 'request', 'status_code', 'text', 'url']
    >>>
    >>> r = requests.get("https://www.google.com", hooks={'response':(lambda res, **kwargs: print(res.status_code))})
    200
    ```

    여러 기능을 세분화하기 위해 여러 함수가 들어있는 리스트를 매개변수에 넘겨주게 되면 모든 함수가 requests 모듈 내부에서 for문으로 인해 실행이 됩니다.

    > Example Code ( List )

    ```py
    >>> import requests
    >>> def res1(res, **kwargs):
    ...     print(f"res1 : {res}")
    ...
    >>> def res2(res, **kwargs):
    ...     print(f"res2 : {res}")
    ...
    >>> def res3(res, **kwargs):
    ...     print(f"res3 : {res}")
    ...
    >>> r = requests.get("https://www.google.com", hooks={'response':[res1, res2, res3]})
    res1 : <Response [200]>
    res2 : <Response [200]>
    res3 : <Response [200]>
    ```

    해당 hooks 매개변수를 이용하여 앞서 언급했던것 처럼 response를 조작할 수 있습니다.

    > Example Code ( set response )

    ```py
    >>> import requests
    >>> def res_set_status_code(res, **kwargs):
    ...     print(f"hooks in {res}")
    ...     print(f"hooks in {res.status_code}")
    ...     res.status_code = 404
    ...     return res
    ...
    >>> r = requests.get("https://www.google.com", hooks={'response':res_set_status_code})
    hooks in <Response [200]>
    hooks in 200
    >>> r.status_code
    404
    ```
    
---

+ #### stream( bool )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/sessions.py#L686-L687)

    stream=True가 설정되면 대용량 응답을 한번에 읽는 것을 방지하여 메모리 사용을 조절할 수 있습니다

    만약 stream=False의 경우 자동으로 r.content를 실행하여( 대용량 응답 한번에 처리 ) ``r.raw.``, ``r.iter_content``와 비슷한 기능을 사용하지 못합니다

    + ``stream=False``

        ```py
        # https://github.com/psf/requests/blob/main/requests/sessions.py#L686-L687
        if not stream:
            r.content
        ```

    > Example Code

    ```py
    >>> import requests
    >>> r = requests.get("https://www.google.com", stream=False)
    >>> r.raw.read()
    b''
    >>> import requests
    >>> r = requests.get("https://www.google.com", stream=True)
    >>> r.raw.read(10)
    b'\x1f\x8b\x08\x00\x00\x00\x00\x00\x02\xff'
    ```
    
---

+ #### verify( bool, str )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L216-L240)

    verify는 서버 TLS 인증서 확인 여부를 제어하기 위해 사용이 되며 True, False 또는 certificate의 경로를 넣어주어 사용도 가능합니다

    > Example Code

    ```py
    >>> import requests
    >>> r = requests.get("<SSL 만료된 URL>")
    Traceback (most recent call last):
      [ ... ]
      File "/home/me2nuk/.local/lib/python3.8/site-packages/requests/adapters.py", line 517, in send
        raise SSLError(e, request=request)
    requests.exceptions.SSLError: HTTPSConnectionPool(host='<SSL 만료된 URL>', port=443): Max retries exceeded with url: / (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: certificate has expired (_ssl.c:1131)')))
    >>>
    >>> r = requests.get("<SSL 만료된 URL>", verify=False)
    /home/me2nuk/.local/lib/python3.8/site-packages/urllib3/connectionpool.py:1013: InsecureRequestWarning: Unverified HTTPS request is being made to host '<SSL 만료된 URL>'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/1.26.x/advanced-usage.html#ssl-warnings
    warnings.warn(
    >>> r
    <Response [200]>
    >>>
    >>> import certifi
    >>> r = requests.get("https://www.google.com", verify=certifi.where())
    >>> r
    <Response [200]>
    ```
    
---

+ #### cert( str, tuple )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L242-L254)

    cert는 클라이언트 측 인증서, 단일 파일 ( 개인 키, 인증서 포함 ) 또는 두 파일 경로의 튜플로 인증서를 지정할 수 있습니다.

    ```py
    >>> import requests
    >>> r = requests.get("https://www.google.com", cert=('/path/client.cert', '/path/client.key'))
    >>> r
    <Response [200]>
    >>> r = requests.get("https://www.google.com", cert='/path/client.cert')
    >>> r
    <Response [200]>
    ```
    
---

+ #### json( dict )

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L479)

    json 매개변수는 어떠한 서버에 json 데이터를 전송해야되는 경우 유용하게 쓰일 수 있습니다.

    json 매개변수를 사용하면 요청 헤더에 기본적으로 Content-Type이 `application/json` 으로 지정이 된 상태로 요청이 됩니다.

    > Example Code

    ```py
    >>> import requests
    >>> r = requests.get("https://example.com", json={'test1':'jsondata2'})
    >>> r.request.headers
    {'User-Agent': 'python-requests/2.22.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive', 'Content-Length': '22', 'Content-Type': 'application/json'}
    >>> r.request.body
    b'{"test1": "jsondata2"}'
    >>> r.request.body.decode()
    '{"test1": "jsondata2"}'
    ```
    
---

<br>

---

# Response

> ``Class requests.modules.Response``

``Response``는 HTTP 요청에 대한 서버의 응답을 포함한 객체 입니다.

requests 모듈을 이용하여 요청 한 다음 그에 대한 Response 결과들이 사용자가 좀 더 편하게 볼 수 있도록 ``request.modules.Response`` 클래스를 이용하여 좀 더 편하게 정리해주게 됩니다.

대부분의 예시는 하단의 코드로 대체됩니다.

> Example Code

```py
>>> import requests
>>> r = requests.get("https:/example.com")
```

---

+ ### ``r.text``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L853-L889)

    text는 요청/응답 본문을 자동으로 디코드시킨 값을 str 타입으로 반환합니다.

    > Example Code

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

    ---

+ ### ``r.content``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L833-L851)

    content는 요청/응답 본문을 byte 타입으로 반환됩니다.

    > Example Code

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

    ---

+ ### ``r.json()``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L891-L925)

    > ``json(self, **kwargs)``

    json() 는 요청/응답 본문을 json 형식으로 디코딩하여 반환합니다.

    만약 올바른 json 형식이 아닌 경우 에러를 반환합니다.

    > Example Code

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

    ---

+ ### ``r.status_code``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L269)

    ``status_code``는 [http 응답 코드](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)를 나타냅니다. 요청에 성공한 경우 일반적으로 200을 반환 합니다.

    > Example Code

    ```py
    >>> r = requests.get("https://example.com")
    >>> r.status_code
    200
    ```

    ---

+ ### ``r.url``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L279-L282)

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

    ---

+ ### ``r.history``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/sessions.py#L665-L687)

    ``history``는 모든 리다이렉션 응답은 가장 오래된 요청에서 최근 요청 순으로 Response 개체 목록을 반환 합니다.

    이해가 더 잘되기 위해 예시로 로컬에서 Flask으로 여러번 리다이렉션을 반복하여 테스트 했습니다.

    > 127.0.0.1:8080/redirect/n Code

    ```py
    from flask import Flask, redirect

    app = Flask(__name__)

    @app.route('/redirect/<int:n>')
    def redirects(n):
        return (redirect(f'/redirect/{n-1}', code=302) if n >= 1 else 'redirect TEST end')

    app.run('127.0.0.1', 8080)
    ```

    ---

    > Example Code

    > ``127.0.0.1:8080/redirect/n`` -> ``/redirect/0``

    ```py
    >>> r = requests.get("https://example.com")
    >>> r.history
    []
    >>> r.url
    'https://example.com'
    >>> r = requests.get("http://127.0.0.1:8080/redirect/2")
    >>> r.history
    [<Response [302]>, <Response [302]>]
    >>> r.url
    'http://127.0.0.1:8080/redirect/0'
    >>>
    >>> r.history[0]
    <Response [302]>
    >>> r.history[0].url
    'http://127.0.0.1:8080/redirect/2'
    >>>
    >>> r.history[1]
    <Response [302]>
    >>> r.history[1].url
    'http://127.0.0.1:8080/redirect/1'
    >>> r.url
    'http://127.0.0.1:8080/redirect/0'
    ```

    ---

+ ### ``r.links``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L927-L943)

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

    ---

+ ### ``r.headers``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L272)

    ``headers``는 요청한 뒤 응답 헤더를 반환 합니다.

    > Example Code

    ```py
    >>> r = requests.get("https://example.com")
    >>> r.headers
    {'Content-Encoding': 'gzip', 'Age': '330304', 'Cache-Control': 'max-age=604800', 'Content-Type': 'text/html; charset=UTF-8', 'Date': 'Fri, 30 Apr 2021 13:58:41 GMT', 'Etag': '"3147526947+gzip"', 'Expires': 'Fri, 07 May 2021 13:58:41 GMT', 'Last-Modified': 'Thu, 17 Oct 2019 07:18:26 GMT', 'Server': 'ECS (sab/56BC)', 'Vary': 'Accept-Encoding', 'X-Cache': 'HIT', 'Content-Length': '648'}
    ```

    ---

+ ### ``r.cookies``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L285)

    ``cookies``는 요청한 뒤 응답 헤더에 있는 쿠키를 편하게 보여줍니다.

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

    ---

+ ### ``r.connection``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L289)

    ``connection``은 requests 모듈 내부에서 요청을 한 다음 response을 처리하는 과정인 context가 들어있습니다

    ```py
    >>> import requests
    >>> r = requests.get("Https://www.google.com")
    >>> r.connection
    <requests.adapters.HTTPAdapter object at 0x7f8dea56f6d0>
    >>> type(r.connection)
    <class 'requests.adapters.HTTPAdapter'>
    >>> r.connection.config
    {}
    >>> r.connection.proxy_manager
    {}
    >>> r.connection.max_retries
    Retry(total=0, connect=None, read=False, redirect=None, status=None)
    >>> r.connection._pool_connections
    10
    >>> r.connection._pool_maxsize
    10
    >>> r.connection._pool_block
    False
    ```

    ---

+ ### ``r.elapsed``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/sessions.py#L648-L649)

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

    ---

+ ### ``r.is_permanent_redirect``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L730-L733)

    ``is_permanent_redirect``는 r.is_redirect와 유사하게 response header에 location헤더가 있고 status_code코드가 301 또는 308인 경우 True를 반환합니다.

    + 조건 코드

        ```py
        return ('location' in self.headers and self.status_code in (codes.moved_permanently, codes.permanent_redirect)) # location header and 301 or 308
        ```

    ```py
    >>> r = requests.get("https://google.com", allow_redirects=False)
    >>> r.status_code
    301
    >>> r.url
    'https://google.com/'
    >>> r.is_permanent_redirect
    True
    >>> r = requests.get("https://www.google.com")
    >>> r.status_code
    200
    >>> r.url
    'https://www.google.com/'
    >>> r.is_permanent_redirect
    False
    ```

    ---

+ ### ``r.is_redirect``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L723-L728)

    ``is_redirect``는 ``is_permanent_redirect``와 유사하지만 True를 반환하는 조건이 넓습니다

    is_redirect의 경우 http 상태코드가 301, 302, 303, 307, 308 중 하나라도 만족하고 response header에 location 헤더가 있는 경우에 True를 반환 합니다

    + 조건 코드

        ```py
        REDIRECT_STATI = (
            codes.moved,               # 301
            codes.found,               # 302
            codes.other,               # 303
            codes.temporary_redirect,  # 307
            codes.permanent_redirect,  # 308
        )
        [ ... ]
        return ('location' in self.headers and self.status_code in REDIRECT_STATI) # location header and 301 or 302 or 303 or 307 or 308
        ```

    ```py
    >>> import requests
    >>> r = requests.get("https://google.com", allow_redirects=False)
    >>> r.status_code
    301
    >>> r.url
    'https://google.com/'
    >>> r.is_redirect
    True
    >>> r = requests.get("https://google.com")
    >>> r.status_code
    200
    >>> r.url
    'https://www.google.com/'
    >>> r.is_redirect
    False
    ```

    ---

+ ### ``r.ok``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L708-L721)

    ``ok``는 요청/응답 코드가 200이면 True 아니면 False를 반환합니다.

    ``raise_for_status()``를 이용하여 예외처리로 True, False를 구별합니다.

    > ok Function Code

    ```py
    @property
    def ok(self):
        [ ... ]
        try:
            self.raise_for_status()
        except HTTPError:
            return False
        return True
    ```

    ---

    > Example Code

    ```py
    >>> r = requests.get("https://example.com")
    >>> r.status_code
    200
    >>> r.ok
    True
    >>> if r.ok:
    ...     print(1)
    ...
    1
    >>>
    >>> r = requests.get("https://example.com/not-found/")
    >>> r.status_code
    404
    >>> r.ok
    False
    >>> if r.ok:
    ...     print(1)
    ...
    >>>
    ```

    ---

+ ### ``r.reason``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L277)

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

    ---

+ ### ``r.raise_for_status()``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L945-L968)

    ``raise_for_status()``는 요청/응답 코드가 200이 아니면 예외를 발생시킵니다.

    > Example Code

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

    ---

+ ### ``r.encoding``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L275)

    ``encoding``는 요청/응답 헤더를 이용하여 데이터의 인코딩 방식을 추측하여 반환합니다.

    > Example Code

    ```py
    >>> r = requests.get("https://example.com")
    >>> r.encoding
    'UTF-8'
    >>> r.headers
    { [ ... ] 'Content-Type': 'text/html; charset=UTF-8', 'Date': 'Thu, 29 Apr 2021 14:21 [ ... ] IT', 'Content-Length': '648'}
    ```

    ---

+ ### ``r.apparent_encoding``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L740-L743)

    ``apparent_encoding``는 ``chardet.detect``를 이용하여 자동으로 인코딩을 인식하고 반환합니다.

    > apparent_encoding Function Code

    ```py
    @property
    def apparent_encoding(self):
        """The apparent encoding, provided by the chardet library."""
        return chardet.detect(self.content)['encoding']
    ```

    ---

    > Example Code

    ```py
    >>> r = requests.get("https://example.com")
    >>> r.apparent_encoding
    'ascii'
    ```

    ---

+ ### ``r.iter_content()``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L745-L800)

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

    ---

+ ### ``r.iter_lines()``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L802-L831)

    > ``ITER_CHUNK_SIZE = 512``

    > ``iter_lines(self, chunk_size=ITER_CHUNK_SIZE, decode_unicode=False, delimiter=None)``

    ``iter_lines()``는 ``iter_content()``를 이용하여 응답 데이터를 한 번에 한 줄씩 반복하면서 대용량 응답으로 인해 콘텐츠를 한 번에 메모리로 읽는 것을 방지합니다.

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

    ---

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

    ---

+ ### ``r.close()``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L970-L981)

    ``close()``는 서버와의 연결을 닫습니다. 

    > Example Code

    ```py
    >>> r = requests.get("https://example.com")
    >>> r
    <Response [200]>
    >>> r.close
    >>> r.close()
    ```

    ---

+ ### ``r.request``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L280-L599)

    ``request``는 PreparedRequest클래스를 반환하며 요청시에 사용했던 정보들을 확인할 수 있습니다.
    
    > Example Code
    
    ```py
    >>> r = requests.get("https://example.com")
    >>> r.request
    <PreparedRequest [GET]>
    >>> dir(r.request)
    ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_body_position', '_cookies', '_encode_files', '_encode_params', '_get_idna_encoded_host', 'body', 'copy', 'deregister_hook', 'headers', 'hooks', 'method', 'path_url', 'prepare', 'prepare_auth', 'prepare_body', 'prepare_content_length', 'prepare_cookies', 'prepare_headers', 'prepare_hooks', 'prepare_method', 'prepare_url', 'register_hook', 'url']
    ```

    + #### ``r.request.method``

        + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L350-L354)

        HTTP/HTTPS 요청 메서드를 나타냅니다

        > Example Code

        ```py
        >>> import requests
        >>> r = requests.get("https://www.google.com")
        >>> r.request.method
        'GET'
        >>> r = requests.post("https://www.google.com")
        >>> r.request.method
        'POST'
        ```

    + #### ``r.request.path_url``

        + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L69-L88)

        요청한 url의 경로를 나타냅니다

        > Example Code

        ```PY
        >>> import requests
        >>> r = requests.get("https://www.google.com")
        >>> r.request.path_url
        '/'
        >>> r = requests.get("https://www.google.com/test/path")
        >>> r.request.path_url
        '/test/path'
        ```

    + #### ``r.request.url``

        + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L366-L450)

        요청한 URL 전체를 나타냅니다
    
        > Example Code    

        ```py
        >>> import requests
        >>> r = requests.get("https://www.google.com")
        >>> r.request.url
        'https://www.google.com'
        >>> r = requests.get("https://www.google.com/test/aps/ds/fasfas")
        'https://www.google.com/test/aps/ds/fasfas'
        ```

    + #### ``r.request.headers``

        + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L452-L461)

        ``request.headers``는 요청할때 사용된 헤더를 dict 타입으로 반환합니다.

        > Example Code

        ```py
        >>> r = requests.get("https://example.com")
        >>> r.request
        <PreparedRequest [GET]>
        >>>
        >>> r.request.headers
        {'User-Agent': 'python-requests/2.22.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}
        >>> r.request.headers['User-Agent']
        'python-requests/2.22.0'
        ```

    + #### ``r.request._cookies``

        + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L572-L599)

        ``request._cookies``는 요청할때 사용된 쿠키 내용을 dict 타입으로 반환됩니다.

        > Example Code

        ```py
        >>> r = requests.get("https://example.com", cookies={'cookie1':'cookie_value'})
        >>> r.request
        <PreparedRequest [GET]>
        >>>
        >>> r.request._cookies
        >>> r.request._cookies['cookie1']
        'cookie_value'
        >>> r.request._cookies.get_dict()
        {'cookie1': 'cookie_value'}
        ```

    + #### ``r.request.copy()``

        + [Core Code](https://github.com/psf/requests/blob/main/requests/models.py#L339-L348)

        ``request.copy()``는 PreparedRequest의 카피본을 반환합니다. 즉 r.request 결과를 카피한다고 생각하면 됩니다.

        > request.copy Source Code

        ```py
        def copy(self):
            p = PreparedRequest()
            p.method = self.method
            p.url = self.url
            p.headers = self.headers.copy() if self.headers is not None else None
            p._cookies = _copy_cookie_jar(self._cookies)
            p.body = self.body
            p.hooks = self.hooks
            p._body_position = self._body_position
            return p
        ```

        ---

        > Example Code

        ```py
        >>> r = requests.get("https://example.com")
        >>> r.request
        <PreparedRequest [GET]>
        >>>
        >>> r.request.copy
        <bound method PreparedRequest.copy of <PreparedRequest [GET]>>
        >>> r.request.copy()
        <PreparedRequest [GET]>
        >>> r.request.method
        'GET'
        >>> r_copy = r.request.copy()
        >>> r_copy
        <PreparedRequest [GET]>
        >>> r_copy.method
        'GET'
        ```

---

+ ### ``r.raw``

    + [Core Code](https://github.com/psf/requests/blob/main/requests/adapters.py#L276)

    서버에서 원시 소켓 응답을 받기 위해 ``r.raw.*``를 사용하기 위해서는 요청 시 ``stream=True``를 추가해줘야 합니다.

    > Example Code

    ```py
    >>> r = requests.get("https://example.com", stream=True)
    >>> r.raw
    <urllib3.response.HTTPResponse object at 0x7f53bba651f0>
    >>> dir(r.raw)
    ['CONTENT_DECODERS', 'DECODER_ERROR_CLASSES', 'REDIRECT_STATUSES', '__abstractmethods__', '__class__', '__del__', '__delattr__', '__dict__', '__dir__', '__doc__', '__enter__', '__eq__', '__exit__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__next__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '_abc_impl', '_body', '_checkClosed', '_checkReadable', '_checkSeekable', '_checkWritable', '_connection', '_decode', '_decoder', '_error_catcher', '_flush_decoder', '_fp', '_fp_bytes_read', '_handle_chunk', '_init_decoder', '_init_length', '_original_response', '_pool', '_request_url', '_update_chunk_length', 'auto_close', 'chunk_left', 'chunked', 'close', 'closed', 'connection', 'data', 'decode_content', 'enforce_content_length', 'fileno', 'flush', 'from_httplib', 'get_redirect_location', 'getheader', 'getheaders', 'geturl', 'headers', 'info', 'isatty', 'isclosed', 'length_remaining', 'msg', 'read', 'read_chunked', 'readable', 'readinto', 'readline', 'readlines', 'reason', 'release_conn', 'retries', 'seek', 'seekable', 'status', 'stream', 'strict', 'supports_chunked_reads', 'tell', 'truncate', 'version', 'writable', 'writelines']
    ```

    ---

    + #### ``r.raw.read()``


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

## 번외..

해당 블로그 작성을 해보면서 requests 모듈을 직접 분석하고

r.history, r.is_redirect, r.request. 등 다양한 기능을 제외하여 요청하고 응답 메세지를 출력하는 짧은 코드를 만들었습니다

requests 모듈에서는 cookies, headers, data, query string, fragment 등 다양한 요청 정보를 가지고 default를 넣어주거나 URL 조합 하여 그대로 넣어준 다음 

``urllib3.pollmanager.PoolManager(num_pools=,maxsize=,block=,**dict).connection_from_url(URL).urlopen(method=,url=,...)`` 다음과 비슷한 형태를 이용해

요청한 다음 requests.modules.Response 클래스를 이용하여 사용자가 r.text, r.reason 등 쉽게 사용할 수 있도록 정보들을 정리하여 반환 해해줍니다.

### request_short_code.py

```py
from urllib3.util.timeout import Timeout as TimeoutSauce
from urllib3.poolmanager import PoolManager
from timeit import default_timer as dt
from urllib3.util.retry import Retry

CONST_CONNECTIONS = 10
CONST_MAXSIZE = 10
CONST_BLOCK = False
CONST_POOL_KWARGS = {}
CONST_REQUEST_INFO_TIMEOUT = None

REQUEST_URL = 'http://127.0.0.1:5000/'

RETRY_TOTAL = 0
RETRY_READ = False

class request:
    def __init__(self) -> None:
        poolmanager = PoolManager(
            num_pools=CONST_CONNECTIONS,
            maxsize=CONST_MAXSIZE,
            block=CONST_BLOCK,
            **CONST_POOL_KWARGS,
        )
        conn = poolmanager.connection_from_url(REQUEST_URL)
        self.resp = conn.urlopen(
            method = 'GET',
            url = '/',
            body = None,
            headers = {'User-Agent': 'python-requests/2.25.1', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'},
            redirect = False,
            assert_same_host = False,
            preload_content=False,
            decode_content=False,
            retries=Retry(
                RETRY_TOTAL,
                read=RETRY_READ,
            ),
            timeout=TimeoutSauce(
                connect=CONST_REQUEST_INFO_TIMEOUT,
                read=CONST_REQUEST_INFO_TIMEOUT
            )
        )

    @property
    def content(self):
        return b''.join(self.read())

    def read(self):
        content = b''
        while True:
            chunk = self.resp.read(10 * 1024)
            if not chunk:
                break
            yield chunk

time = dt()

for i in range(1,10000):
    request().content

print(dt() - time)
```

### requests_module_code.py

```py
from timeit import default_timer as dt
from requests import get

time = dt()

for i in range(1,10000):
    get('http://127.0.0.1:5000/').content

print(dt()-time)
```

위의 소스코드를 가지고 실행해보면 아래의 결과 처럼 request_short_code(requests 소스코드 줄인 코드), requests_module_code(requests 모듈 코드)마다 약 5초 씩 차이납니다.

```bash
~$ python3 request_short_code.py
11.000992600000004
~$ python3 requests_module_code.py
16.258784799999994
```

이처럼 requests 모듈을 분석 한 다음 코드를 줄이기는 했지만 아무래도 단순히 요청 한 다음 결과를 가져오는것이기 때문에 다양한 코드를 작성하기 위해서는 조금 더 많은 코드가 들어가므로 

결론은 귀찮으면 requests 모듈을 사용하는게 제일 무난하다


## 작성 후기

해당 글이 2000줄이 넘어버렸는데 처음엔 requests 모듈을 한번 까보고 싶다 이 생각뿐이였지만 생각보다 흥미로웠고 배워가는점이 많았습니다.

그래서 이러한 지식들을, 매번 사용법을 까먹고 또 까먹어서 하나하나 찾아보면서 맞는 코드를 찾는 것 보단 내가 직접

requests 모듈 코드 전체를 분석해서 블로그에 기능을 적어놓는게 내 입장에서도 사용자들 입장에서도 편할 것 같아 시작하게 된 글입니다.

이 글이 중간중간 귀찮아서 쓰는 시간이 길어졌지만 그래도 이 글을 보고 배워가시는 분들이 있어서 오히려 뿌듯합니다.