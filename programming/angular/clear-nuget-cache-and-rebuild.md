# Clear NuGet cache & rebuild

dotnet nuget locals all --clear\
dotnet clean\
dotnet restore\
dotnet build
