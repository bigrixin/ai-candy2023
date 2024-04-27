# 10进制36进制互转

```
// Some code

string timeString = DateTime.Now.ToString("yyyyMMddHHmmss");    


//10 to 36    
string string36 = StringDecimalTo2_36(timeString, 36);
//36 to 10
long decimalfrom36 = String36toDecimal(string36, 36);

////////////////////////////////////////////////////////////////////////////////

//10 to 2~36
private string StringDecimalTo2_36(long decimalNumber, int radix)
{
    const int BitsInLong = 64;
    const string Digits = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";

    if (radix < 2 || radix > Digits.Length)
        throw new ArgumentException("The radix must be >= 2 and <= " +
            Digits.Length.ToString());

    if (decimalNumber == 0)
        return "0";

    int index = BitsInLong - 1;
    long currentNumber = Math.Abs(decimalNumber);
    char[] charArray = new char[BitsInLong];

    while (currentNumber != 0)
    {
        int remainder = (int)(currentNumber % radix);
        charArray[index--] = Digits[remainder];
        currentNumber = currentNumber / radix;
    }

    string result = new System.String(charArray, index + 1, BitsInLong - index - 1);
    if (decimalNumber < 0)
    {
        result = "-" + result;
    }

    return result;
}

//36 to 10
private long String36toDecimal(string input, long bs)
{
    try
    {
        long Bigtemp = 0, temp = 1;
        int len = input.Length;
        for (int i = len - 1; i >= 0; i--)
        {
            if (i != len - 1)
                temp *= bs;
            long num = changeDec(input[i]);
            Bigtemp += temp * num;
        }

        return Bigtemp;
    }
    catch
    {
        return 0;
    }
}

//十进制转换中把字符转换为数
private int changeDec(char ch)
{
    int num = 0;
    if (ch >= 'A' && ch <= 'Z')
        num = ch - 'A' + 10;
    else if (ch >= 'a' && ch <= 'z')
        num = ch - 'a' + 36;
    else
        num = ch - '0';
    return num;
}

//数字转换为字符
private char changToNum(int temp)
{
    int n = temp;


    if (n >= 10 && n <= 35)
        return (char)(n - 10 + 'A');


    else if (n >= 36 && n <= 61)
        return (char)(n - 36 + 'a');


    else
        return (char)(n + '0');
}


references:
https://tool.oschina.net/hexconvert/
https://developer.aliyun.com/article/318675
https://www.jianshu.com/p/b498c0eb55e9
```
