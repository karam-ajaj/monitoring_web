#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["monitoring_web/monitoring_web.csproj", "monitoring_web/"]
RUN dotnet restore "monitoring_web/monitoring_web.csproj"
COPY . .
WORKDIR "/src/monitoring_web"
RUN dotnet build "monitoring_web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "monitoring_web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "monitoring_web.dll"]