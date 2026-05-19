# Replace IdentityServer4

```
// replace IdentityServer4


<ItemGroup>
<!--  uninstall below package   -->
<!--   <PackageReference Include="IdentityServer4.AspNetIdentity" Version="4.1.2" /> -->
<!--         -->
<!-- use Duende to replace IdentityServer4 -->

<PackageReference Include="Duende.IdentityServer" Version="7.4.7" />
<PackageReference Include="Duende.IdentityServer.AspNetIdentity" Version="7.4.7" />

<!-- Entity Framework Core package is used for migration-->
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="8.0.8" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="8.0.8" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="8.0.8" />
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="8.0.8" />
</ItemGroup>
```
