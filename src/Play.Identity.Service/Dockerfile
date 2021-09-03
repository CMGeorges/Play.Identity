#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Play.Identity/src/Play.Identity.Service/Play.Identity.Service.csproj", "Play.Identity/src/Play.Identity.Service/"]
COPY ["../Play.Common/src/Play.Common/Play.Common.csproj", "../Play.Common/src/Play.Common/"]

WORKDIR /src

RUN dotnet restore "Play.Identity/src/Play.Identity.Service/Play.Identity.Service.csproj"
COPY . .
WORKDIR "/src/Play.Identity/src/Play.Identity.Service"
RUN dotnet build "Play.Identity.Service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Play.Identity.Service.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Play.Identity.Service.dll"]