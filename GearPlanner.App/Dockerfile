FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y \
        nodejs \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /src
COPY ["GearPlanner.App/GearPlanner.App.csproj", "GearPlanner.App/"]
RUN dotnet restore "GearPlanner.App/GearPlanner.App.csproj"
COPY . .
WORKDIR "/src/GearPlanner.App"
RUN dotnet build "GearPlanner.App.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "GearPlanner.App.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "GearPlanner.App.dll"]
