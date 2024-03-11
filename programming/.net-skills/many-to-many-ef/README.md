---
description: LINQ and AutoMap
---

# Many to Many EF

{% code fullWidth="true" %}
```
[Bin] 1 ---- * [LocationBin] * ---- 1 [Location]
```
{% endcode %}

{% code fullWidth="true" %}
```
public async Task<IEnumerable<Bin>> GetAllBinsAsync()
{
    return await _context.Set<Bin>()
        .Include(c => c.LocationBins)
        .ThenInclude(a => a.Location)
        .Where(l => l.LocationBins.Count() > 0)
        .ToListAsync();
}

public async Task<IEnumerable<Bin>> GetAllBinsByLocationIdAsync(int locationId)
{
    return await _context.Set<Bin>()
        .Include(c => c.LocationBins)
        .ThenInclude(a => a.Location)
        .Where(l => l.LocationBins.Any(b => b.LocationId == locationId))
        .ToListAsync();
}


```
{% endcode %}

{% code fullWidth="true" %}
```

CreateMap<Bin, BinViewModel>()
 .ForMember(dest => dest.Location, opt => opt.MapFrom(src => src.LocationBins.Select(a=>a.Location).FirstOrDefault().Name));
```
{% endcode %}

{% code fullWidth="true" %}
```csharp

            modelBuilder.Entity<CompanyVehicle>(entity =>
            {
                entity.ToTable("COMPANY_VEHICLE");

                entity.HasIndex(e => e.Companyid, "COMPANYVEHICLE_TO_COMPANY");

                entity.HasIndex(e => e.Vehicleid, "COMPANYVEHICLE_TO_VEHICLE");

                entity.Property(e => e.Companyid).HasColumnName("COMPANYID");

                entity.Property(e => e.Companyvehicleid)
                    .HasColumnName("COMPANYVEHICLEID")
                    .HasDefaultValueSql("([dbo].[seqno_COMPANY_VEHICLE_COMPANYVEHICLEID]())");

                entity.Property(e => e.Vehicleid).HasColumnName("VEHICLEID");

                entity.HasOne(d => d.Company)
                    .WithMany(p => p.CompanyVehicles)
                    .HasForeignKey(d => d.Companyid)
                    // .OnDelete(DeleteBehavior.ClientSetNull)
                    .HasConstraintName("COMPANY_VEHICLE_COMPANYVEHICLE_TO_COMPANY");    ///<<<<<<<<<<<

                entity.HasOne(d => d.Vehicle)
                    .WithMany(p => p.CompanyVehicles)
                    .HasForeignKey(d => d.Vehicleid)
                    //   .OnDelete(DeleteBehavior.ClientSetNull)
                    .HasConstraintName("COMPANY_VEHICLE_COMPANYVEHICLE_TO_VEHICLE");
            });
```
{% endcode %}
