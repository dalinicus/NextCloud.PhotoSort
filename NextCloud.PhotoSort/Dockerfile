FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["NextCloud.PhotoSort/NextCloud.PhotoSort.csproj", "NextCloud.PhotoSort/"]
RUN dotnet restore "NextCloud.PhotoSort/NextCloud.PhotoSort.csproj"
COPY . .
WORKDIR "/src/NextCloud.PhotoSort"
RUN dotnet build "NextCloud.PhotoSort.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "NextCloud.PhotoSort.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NextCloud.PhotoSort.dll"]
