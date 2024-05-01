# Lambda group by many



```
// Some code
public async Task<IEnumerable<BatchVM>> GetSummaryAsync(DateTime beginDate, DateTime endDate)
{
  return await _context.Set<Batch>()
        .Where(c => c.StartTime >= beginDate
            && c.EndTime <= endDate)
        .GroupBy(a=>new {a.Location, a.LocationName})   //  group by many
        .Select(a=> new BatchVM                         //  new class BatchVM 
        {
            CompactorNumber = a.Key.Location,           //  name from group key  
            CompactorLocation = a.Key.LocationName,
            TotalCounter = a.Sum(a=>a.TotalCounter),
            TotalWeight = a.Sum(a=>a.TotalWeight)
        })
        .ToListAsync();
}
```

>

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>
