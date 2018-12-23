# Crypto - Special Christmas Wishlist

디따 큰 이미지가 주어지는데 이미지를 다음 코드로 잘라서 알파벳으로 치환해준다

```python
from PIL import Image, ImageChops

WIDTH = 90
HEIGHT = 180

PATH = 'wishlist.png'
image = Image.open(PATH)

def compare(a, b):
    return ImageChops.difference(a, b).getbbox() is None

imgs = []
imgwidth, imgheight = image.size
for j in range(0, imgheight, HEIGHT):
    for i in range(0, imgwidth, WIDTH):
        box = (i, j, i + WIDTH, j + HEIGHT)
        crop = image.crop(box)
        isexist = False
        for img in imgs:
            if compare(img, crop):
                isexist = True
                break
        if not isexist:
            imgs.append(crop)

print(f'total: {len(imgs)} images found')

blank = imgs.pop(7) # 7번째 이미지가 공백이다
alphs = 'abcdefghijklmnopqrstuvwxyz'
res = ''

for j in range(0, imgheight, HEIGHT):
    for i in range(0, imgwidth, WIDTH):
        box = (i, j, i + WIDTH, j + HEIGHT)
        crop = image.crop(box)
        isexist = False
        idx = 0
        if compare(crop, blank):
            res += ' '
        else:
            for img in imgs:
                if compare(img, crop):
                    isexist = True
                    res += alphs[idx]
                idx += 1
            if not isexist:
                raise Exception('shit')
    res += '\n'

print(res)
```

그렇게 해서 구한 평문 암호문은 다음과 같다

```
abcdcefg ahijdcefg khelg ldji
jdmbnng nbodap qhhrgifl
kcoa qggm jabllgl lgc hn csh
qbf fhj sdlfho ceoqagml
bftgicemgm oeacdchha uadv sbcuk
csdj obmlkobaahs lrgsgm
iglcdij lchmbjg uhicbdigml
lodadij wdxh jbmfgi lueavcemg
ahij fdlcbiug cheuk abov
oeacduhahm hoqmg lcgoagll sdig jabll lgc
dvkhig ukbmjgm lcdurgm nbugl lgc
vgfglcba wgsgamp khafgm
bmcdlbiba qboqhh lbac ukglc
bemhmb lobmc adjkcdij vbigal
ckg lgb lgmvgic jbmfgi lueavcemg
ckg yedcg bobxdij yeglc nhm yegmdgl
mgupuagf jabll cmgg jahqgl mgabcdhilkdvl
igs phmr cdogl uelcho nmhic vbjg vexxag
ckg qglc ladij qgtgmbjg uhhagm
cmdh odzgf ogcbal gbmmdijl
qdmck ohick nahsgm igurabug
b ohckgml ahtg dl qgphif ogblemg lvhhi lgc
sgffdij sbacx vgmlhibadxgf bmc
vgmlhibadxgf jhhfidjkc adccag og qhhr
uelcho vgc ihlg vmdic igurabugl
uelcho vgc vdaahsl
ysgmcp rgpqhbmf
aabobmbob abmjg xdvvgm vheuk
odohlb fdbjmbo jabllsbmg lgc hn ckmgg
ckbir phe nhm phem vbmc di op whemigp igurabug
phjb vhlg kbijdij lueavcemgl
ckg qdrg ukbdi qhsa
umdolhi kgbmc eoqmgaab
ckg yedx dildfg ckg obxg
ogil kgmqba sbmodij ladvvgml
pheml odig bif heml gijmbtgf fgubicgm lgc
ckg biidtgmlbmp whemiba
ohoob qdmf uenn
sdig qbmmga jedcbm mbur
ckg vaelk hmjbil
jbmadu jmbcgm bif hda fdvvdij fdlk
hrchqgmnglc bag qggm qmgsdij rdc
obcgl nhm adng
qhhr ahtgml adjkcsgdjkc lubmtgl
uhilcdcecdhi hn eidcgf lcbcgl hn bogmdub jabll
jhan jabllgl vgscgm bijga uhdil lgc hn csgatg
qeiip ngac qbqp ladvvgml
vgmlhibadxgf cmgg cmeir jabllsbmg feh
gzvgucdij phe b rggvlbrg vmgjibiup whemiba
ckg qgmmp qeffp
ckg qdmfdg pbmi qhsa
rbickb ukbifgadgml gbmmdijl
vgmlhibadxgf op tgmp hsi ibog qhhr
eiduhmi bif mbdiqhs odlobcukgf gbmmdijl
lahck vbal ohqdag
qbc hi b qmbiuk
vgmlhibadxgf ueccdij qhbmf
lohrg fgcguchm fgbucdtbcdhi chsga
ckg aeig adjkc
vgmlhibadxgf biidtgmlbmp velkvdi elb obv
ogil cbuh lhurl
gashhf ckg eiduhmi ugmgba qhsa
dicgmlgucdhi hn ahtg vkhch vmdic
ckg adccag vbcdgic
udmuag hn nbodap bif nmdgifl lgmtdij qhsa
jabll nahsgm jbmfgi ugicgmvdgug
qdmcklchig odigmba lhbvl
lgc hn nhem jhan jabllgl
ckg ugfbm ckeoq vdbihl
uelcho obv uhblcgm lgc
qdj vgmlhibadcp fglr ldjil
lgb lchig lvablk lvhijg khafgm
phem ohlc fgldmgf hqwguc ckdl ukmdlobl
ckg nabj dl
zoblphebmglhjhhfbcleqlcdcecdhiudvkgml
```

마지막 두 줄을 주목해보자
`the flag is / xmas` 일테다
그리고 빈도를 봐서 가장 많은 `g` 가 `e` 일테다

그렇게 해서 하나 하나 알아가면 된다
대충 알파벳 `abcd...z` 는 `LATIUDEONGHSRFMYB_WVCPJ__X` 으로 치환되었다. `_` 는 잘 모르지만 플래그를 알아내는데 굳이 필요하지는 않았다

그렇게 해서 복호화된 암호문의 끝부분은 다음과 같다

```
sea stone splash sponge holder
your most desired object this chrismas
the flag is
xmasyouaresogoodatsubstitutionciphers
```

플래그는 소문자에 일반적인 `X-MAS{...}` 꼴이 아니랬으니

```
xmasyouaresogoodatsubstitutionciphers
```