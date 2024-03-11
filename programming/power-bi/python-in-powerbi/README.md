---
description: IEEE 754 conversion
---

# Python in PowerBI

```
// Some code
# 'dataset' holds the input data for this script
import struct

# decimal(32) to binstr   
def decimalToBinary(value):
    if value < 0:    #Base case if number is a negative
        return 'Not positive'
    elif value == 0: #Base case if number is zero
        return '0'
    else:
        return str(decimalToBinary(value//2)) + str((value%2))
    
# binary32 str to float   
def binToFloat32(bin32str):
    f = int(bin32str, 2)
    float_val = struct.unpack('f', struct.pack('I', f))[0]
    return float_val
	
# decimal to float (IEEE-754)
def ieee_754_conversion(dec_val):
    bin32str = decimalToBinary(dec_val)
    float32_val = binToFloat32(bin32str)
    return float32_val


## DEC = high * 65536 + low  

for i in range(0, len(dataset)):
   dataset.loc[i,'HumidityValue'] = ieee_754_conversion(dataset.loc[i,'HumidityDEC'])
```
