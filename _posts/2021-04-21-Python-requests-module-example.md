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