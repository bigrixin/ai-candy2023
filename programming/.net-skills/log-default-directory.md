# Log default directory

```
// nlog.config

<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      autoReload="true"
      internalLogLevel="Trace"    // INFO, WARN, and ERROR   <<<<<<<
      internalLogFile="${basedir}/logs/internal-nlog-AspNetCore.txt">   <<<<<<<<
```