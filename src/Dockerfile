FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY Directory.Packages.props Directory.Packages.props
COPY FormulaApp.sln FormulaApp.sln
COPY WebApi/FormulaApp.WebApi.csproj WebApi/FormulaApp.WebApi.csproj
RUN dotnet restore FormulaApp.sln

COPY . .
RUN dotnet publish WebApi/FormulaApp.WebApi.csproj -c Release -o /src/out

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /src/out .

EXPOSE 5080 5443

ENTRYPOINT ["dotnet", "FormulaApp.WebApi.dll"]
