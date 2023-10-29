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

RUN apt-get update && apt-get install -y curl=7.88.1-10+deb12u4 apt-utils=2.6.1 --no-install-recommends && rm -rf /var/lib/apt/lists/*

RUN groupadd -g 10000 dotnet && useradd -u 10000 -g dotnet -m -d /app dotnet && chown -R dotnet:dotnet /app
USER dotnet:dotnet

COPY --chown=dotnet:dotnet --from=build /src/out .

EXPOSE 5080 5443
ENV ASPNETCORE_URLS=http://+:5080

HEALTHCHECK --interval=5s --timeout=10s --retries=3 CMD curl --fail http://localhost:5080/healthz || exit 1

ENTRYPOINT ["dotnet", "FormulaApp.WebApi.dll"]
