---
description: get local time in Azure SQL
---

# SQL Azure time convert

**SQL function 1 (it is work)**

```
// SQL timezone convert function

CREATE OR ALTER FUNCTION dbo.CurrentLocalTime
( 
    @TimeZoneName sysname
)
RETURNS datetime2
AS
BEGIN
 
--
-- Test examples: 
/*
SELECT dbo.CurrentLocalTime('AUS Eastern Standard Time');
*/
 
  RETURN DATEADD(hour, 
                 TRY_CAST((SELECT REPLACE(current_utc_offset, N':', N'.')
                           FROM sys.time_zone_info
                           WHERE [name] = @TimeZoneName) AS decimal(18,2)), 
                 SYSDATETIME());
END;
GO


select GETDATE() AS SystemTime, dbo.CurrentLocalTime('AUS Eastern Standard Time') as LocalTime
```

>

&#x20;

function 2

```
// SQL Function 2  (not work)

CREATE FUNCTION [dbo].f_CurrentDateTime RETURNS DATETIME AS 
BEGIN 
   RETURN DATEADD(HOUR,CONVERT(INT,(SELECT is_currently_dst FROM sys.time_zone_info WHERE 1=1 AND NAME = 'AUS Eastern Standard Time')),GETDATE())
 END

f_CurrentDateTime
```





