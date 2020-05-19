FROM mcr.microsoft.com/dotnet/core/sdk:3.1.102-alpine AS build-env
WORKDIR /build

COPY CatalogApi.csproj .
COPY --from=srl/shared /root/.nuget/packages /root/.nuget/packages
COPY --from=srl/shared /shared /libraries/shared
RUN dotnet restore CatalogApi.csproj --no-dependencies

COPY . .
RUN dotnet publish CatalogApi.csproj --no-restore --no-dependencies -c Release -o /out



FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-alpine
WORKDIR /app

COPY --from=build-env /out ./

ENV ASPNETCORE_URLS http://*

EXPOSE 80
ENV ASPNETCORE_ENVIRONMENT Development
ENV IN_DOCKER true
ENTRYPOINT dotnet CatalogApi.dll
