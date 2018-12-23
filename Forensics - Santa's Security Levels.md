# Forensics - Santa's Security Levels

mp3 파일의 모스 신호를 해독하면 다음과 같다

```
github com gooogal xmas
```

링크 [github.com/gooogal/xmas](https://github.com/gooogal/xmas) 에 들어가면 special message.txt 파일에 다음과 같이 적
혀있다

```
Santa doesn't like people searching for his flags, but you look like a nice person. 
Anyway here's your flag:

vF ur uNq nAlguvat pbasvqraGvNy gb fnl, ur jebgr Vg ia pvcure, gung vF, ol FB punaTvat gur beqre bs gur Yrggref bs gur nycuNorg, gung abg n jbeQ pbhyq or ZnQR bHg.
```

아래 메시지는 ROT13 으로 해독된다

```
iS he hAd aNything confidenTiAl to say, he wrote It vn cipher, that iS, by SO chanGing the order of the Letters of the alphAbet, that not a worD could be MaDE oUt.
```

대문자만 뽑아보면 다음과 같다

```
S  A N TA  ,   I  ,  S,  SO G     L   A,    D   MDE U
```

답은 `X-MAS{santaissogladmdeu}`