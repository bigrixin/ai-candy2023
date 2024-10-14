# Delete data and reset auto-increment ID



{% code fullWidth="true" %}
```
// Some code

-- VehicleTypes <1----*> VehicleVehicleTypes  <*----1> Vehicles

-- 清理 vehiclevehicletype 表
DELETE FROM VehicleVehicleTypes;
DBCC CHECKIDENT ('VehicleVehicleTypes', RESEED, 0); -- 重置自增ID

-- 清理 vehicle 表
DELETE FROM Vehicles;
DBCC CHECKIDENT ('Vehicles', RESEED, 0); -- 重置自增ID

-- 清理 vehicletype 表
DELETE FROM VehicleTypes;
DBCC CHECKIDENT ('VehicleTypes', RESEED, 0); -- 重置自增ID


```
{% endcode %}
