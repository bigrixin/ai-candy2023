# Data

**Normalization** - data between 0-1

```
import numpy as np

def NormalizeData(data):
    return (data - np.min(data)) / (np.max(data) - np.min(data))

X = np.array([[0, 1], [2, 3], [4, 5], [6, 7], [8, 9], [10, 11], [12, 13], [14, 15]])

scaled_x = NormalizeData(X)

print(scaled_x)

---------------------------------------------------
[[0.         0.06666667]
 [0.13333333 0.2       ]
 [0.26666667 0.33333333]
 [0.4        0.46666667]
 [0.53333333 0.6       ]
 [0.66666667 0.73333333]
 [0.8        0.86666667]
 [0.93333333 1.        ]]
```
