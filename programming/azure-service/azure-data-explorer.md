---
description: Delete records
---

# Azure Data Explorer



```
// view data
Compass
| where deviceId == 'pvzhdc8b1sn1'
| take 1000

// delete data
.delete table Compass records <|
    Compass
    | where deviceId == 'pvzhdc8b1sn1'

```
