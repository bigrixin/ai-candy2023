# Database first, scaffold to class

### In Package Manager Console, generate classes from the database

{% code fullWidth="true" %}
```
// Chang NameOfConnection
Scaffold-DbContext "Name=ConnectionStrings:NameOfConnection" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables dbo.***,
```
{% endcode %}
