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

```py
>>> r = requests.get("https://example.com")
>>> r = requests.post("https://example.com")
>>> r = requests.put("https://example.com")
>>> r = requests.delete("https://example.com")
>>> r = requests.head("https://example.com")
>>> r = requests.options("https://example.com")
```

* * *

# requests 모듈 기본 사용법

대부분의 예시는 하단의 코드로 대체됩니다.

```py
>>> import requests
>>> r = requests.get("https:/example.com")
```

* * *

### r.text

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

### r.content

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

### r.raw.read()