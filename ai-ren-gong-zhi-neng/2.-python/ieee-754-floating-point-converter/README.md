# IEEE-754 Floating Point Converter

```
// 64 bit (double)

# 64 bit floats are represented by IEEE 754 floating-point format (not 32 bits)
## 1. float to binary 64 bit 
## 2. binary 64 to float

import struct
 
# float64 to binary64
def floatToBinary64(value):
    val = struct.unpack('Q', struct.pack('d', value))[0]
    getBin = lambda x: x > 0 and str(bin(x))[2:] or "-" + str(bin(x))[3:]
    return getBin(val)
# binary64 to float64 
def binary64ToFloat(value):
    hx = hex(int(value, 2))   
    return struct.unpack("d", struct.pack("q", int(hx, 16)))[0]
 
## begin
float_val = 19.5
bin64str = floatToBinary64(float_val)
print('Binary 64 equivalent of '+ str(float_val) + ':\n' + bin64str + '\n')

# binary64 to float64
float64_val = binary64ToFloat(bin64str)
print('Decimal float(IEEE745 floating-point 64) equivalent of ' + bin64str + ':\n' + str(float64_val))
```

>
>
> ```
> Binary 64 equivalent of 19.5:
> 100000000110011100000000000000000000000000000000000000000000000
>
> Decimal float(IEEE745 floating-point 64) equivalent of 100000000110011100000000000000000000000000000000000000000000000:
> 19.5
> ```

```
// 32 bit (single)

# 32 bit floats are represented by IEEE 754 floating-point format (not 64 bits)
### Decimal (32) to float 32 (IEEE-745)
## 1. decimal to 32-bit binary string 
## 2. binary 32 to float
## 3. float to bianary 32

########## 32 bit range ##########
###### DEC: 4,294,967,295
###### HEX: FFFF FFFF
###### BIN: 1111 1111 1111 1111 1111 1111 1111 1111

import struct

# decimal(32) to binstr   
def decimalToBinary(value):
    if value < 0:    #Base case if number is a negative
        return 'Not positive'
    elif value == '0': #Base case if number is zero
        return 0
    else:
        return str(decimalToBinary(value//2)) + str((value%2))
    
# binary32 str to float   
def binToFloat32(bin32str):
    f = int(bin32str, 2)
    float_val = struct.unpack('f', struct.pack('I', f))[0]
    return float_val

# float to binary32
def floatToBinary32(num):
    bits, = struct.unpack('!I', struct.pack('!f', num))
    return "{:032b}".format(bits)


## sample bin data <<<
print ('---------------------------')
print (binToFloat32('01000001101011000111101011100001'))
print (binToFloat32('11000001101011000111101011100001'))
print (binToFloat32('0111111011001100110011001100110'))
print ('---------------------------\n')

# begin
dec_val = 1056964608 # 1063675494       # DEC: 1108613663 -> floating-point: 37.025508880615234

bin32str= decimalToBinary(dec_val)
print('Decimal(32) equivalent of '+ str(dec_val) + ':\n' + bin32str + '\n')

float32_val = binToFloat32(bin32str)
print('Decimal Float(IEEE745 floating-point 32) equivalent of ' + bin32str + ':\n' + str(float32_val)+'\n')    

bin32str = floatToBinary32(float32_val)
print('Binary 32 equivalent of '+ str(float32_val) + ':\n' + bin32str + '\n')
           
```

```
---------------------------
21.559999465942383
-21.559999465942383
0.8999999761581421
---------------------------

Decimal(32) equivalent of 1056964608:
0111111000000000000000000000000

Decimal Float(IEEE745 floating-point 32) equivalent of 0111111000000000000000000000000:
0.5

Binary 32 equivalent of 0.5:
00111111000000000000000000000000
```

```
# decimal to float (IEEE-754)
def ieee_754_conversion(dec_val):
    bin32str = decimalToBinary(dec_val)
    float32_val = binToFloat32(bin32str)
    return float32_val

ieee_754_conversion(0)
```
