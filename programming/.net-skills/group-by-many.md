---
description: Lambda and SQL
---

# Group by many



```
// Lambda code
public async Task<IEnumerable<BatchVM>> GetSummaryAsync(DateTime beginDate, DateTime endDate)
{
  return await _context.Set<Batch>()
        .Where(c => c.StartTime >= beginDate
            && c.EndTime <= endDate)
        .GroupBy(a=>new {a.Location, a.Name})   //  group by many
        .Select(a=> new BatchVM                         //  new class BatchVM 
        {
            CompactorNumber = a.Key.Location,           //  name from group key  
            CompactorLocation = a.Key.Name,
            TotalCounter = a.Sum(a=>a.TotalCounter),
            TotalWeight = a.Sum(a=>a.TotalWeight)
        })
        .ToListAsync();
}
```

>

```
// SQL Code

select
    location,
    name,
    type,
    count(type) as errortypecount,
    datediff(hour,  min(occurrence_time),max(resolved_time)) as downhours,
    datediff(hour,  min(occurrence_time),max(resolved_time))/24 as percentage
from dbo.error
group by location, name, type;
```

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>
