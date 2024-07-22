FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5001
EXPOSE 5000

ENV ASPNETCORE_URLS=https://+:5000

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["src/Service/MangaHome.Service.csproj", "src/Service/"]
RUN dotnet restore "src/Service/MangaHome.Service.csproj"
COPY . .
WORKDIR "/src/src/Service"
RUN dotnet build "MangaHome.Service.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "MangaHome.Service.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MangaHome.Service.dll"]
