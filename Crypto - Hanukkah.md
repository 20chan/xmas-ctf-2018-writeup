# Crypto - Hanukkah

문제의 키 생성기를 보자

```python
def genKey(k):
	while True:
		r=getrandbits(k)
		while(r%2):
			r=getrandbits(k)
	
		p =  3 * r**2 +  2 * r + 7331
		q = 17 * r**2 + 18 * r + 1339
		n = p * q

		if(isPrime(p) and isPrime(q)):
			return (p,q) , n
```

보면 n은 r에 대한 사차함수의 꼴임을 알 수 있고, 이로써 방정식을 세워 r의 값을 알아낼 수 있다.

이분법으로 방정식의 해(r의 값)를 알아내는 코드는 다음과 같다.

```csharp
using num = System.Numerics.BigInteger;

static void guessR()
{
    num i;
    for (i = 1; ; i *= 2)
    {
        var res = eval(i);
        Console.WriteLine($"{i}: {res}");

        if (res > 100) break;
    }

    num begin = i / 2;
    num end = i;

    while (true)
    {
        i = (begin + end) / 2;
        var res = eval(i);
        Console.WriteLine($"{i}: {res}");
        if (res > 0) end = i;
        else if (res < 0) begin = i;
        else if (res == 0)
            break;
    }
}

// 이 함수는 guessR() 을 돌려서 r 의 값을 알아낸 후 맞는지 검증하려고 넣은 코드이다
static void assertR()
{
    num r = num.Parse("57998468644974352708871490365213079390068504521588799445473981772354729547806");

    num p = 3 * num.Pow(r, 2) + 2 * r + 7331;
    num q = 17 * num.Pow(r, 2) + 18 * r + 1339;
    num n = p * q;

    Console.WriteLine($"p: {p}");
    Console.WriteLine($"q: {q}");
    Console.WriteLine($"n == pubkey: {n == pub}");
}

static num pub = num.Parse("577080346122592746450960451960811644036616146551114466727848435471345510503600476295033089858879506008659314011731832530327234404538741244932419600335200164601269385608667547863884257092161720382751699219503255979447796158029804610763137212345011761551677964560842758022253563721669200186956359020683979540809");
static num eval(num x)
{
    num a = 51 * num.Pow(x, 4);
    num b = 88 * num.Pow(x, 3);
    num c = 128680 * num.Pow(x, 2);
    num d = 134636 * x;
    num e = 9816209 - pub;

    return a + b + c + d + e;
}
```

그렇게 알아낸 r의 값으로 개인키 p와 q를 알 수 있고 이제 복호화만 시키면 된다.

복호화를 하는 코드는 다음과 같다.

```csharp
static num ct = num.Parse("66888784942083126019153811303159234927089875142104191133776750131159613684832139811204509826271372659492496969532819836891353636503721323922652625216288408158698171649305982910480306402937468863367546112783793370786163668258764837887181566893024918981141432949849964495587061024927468880779183895047695332465");
static void Main(string[] args)
{
    num r = num.Parse("57998468644974352708871490365213079390068504521588799445473981772354729547806");

    num p = 3 * num.Pow(r, 2) + 2 * r + 7331;
    num q = 17 * num.Pow(r, 2) + 18 * r + 1339;
    num n = p * q;
    
    var m_p1 = num.ModPow(ct, (p + 1) / 4, p);
    var m_p2 = p - m_p1;
    var m_q1 = num.ModPow(ct, (q + 1) / 4, q);
    var m_q2 = q - m_q1;

    var ext = gcd(p, q);
    var y_p = ext[1];
    var y_q = ext[2];

    var d1 = (y_p * p * m_q1 + y_q * q * m_p1) % n;
    var d2 = (y_p * p * m_q2 + y_q * q * m_p1) % n;
    var d3 = (y_p * p * m_q1 + y_q * q * m_p2) % n;
    var d4 = (y_p * p * m_q2 + y_q * q * m_p2) % n;
    
    var b1 = d1.ToByteArray();
    var b2 = d2.ToByteArray();
    var b3 = d3.ToByteArray();
    var b4 = d4.ToByteArray();

    var s1 = Encoding.ASCII.GetString(b1);
    var s2 = Encoding.ASCII.GetString(b2);
    var s3 = Encoding.ASCII.GetString(b3);
    var s4 = Encoding.ASCII.GetString(b4);
    
    Console.WriteLine(s1);
    Console.WriteLine(s2);
    Console.WriteLine(s3);
    Console.WriteLine(s4);

    Console.Read();
}

static num[] gcd(num a, num b)
{
    num s = 0, prev_s = 1, t = 1, prev_t = 0;
    num r = b, prev_r = a;

    while (r != 0)
    {
        num q = prev_r / r;
        num tr = r;
        r = prev_r - q * r;
        prev_r = tr;

        num ts = s;
        s = prev_s - q * s;
        prev_s = ts;

        num tt = t;
        t = prev_t - q * t;
        prev_t = tt;
    }

    return new num[] { prev_r, prev_s, prev_t };
}

```

그렇게 하면 패딩을 시킨 복호화된 플래그의 값이 뒤집어서 나온다

```
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX}h4kkun4H_3kila_hcum_yr3v_dnu0s_dn4_ko0l_n1baR_dna_1bbaR_h4kkun4H{SAM-X
```

뒤집으면 플래그

```
X-MAS{H4nukk4h_Rabb1_and_Rab1n_l0ok_4nd_s0und_v3ry_much_alik3_H4nukk4h}
```
