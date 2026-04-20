# Base runtime image
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copiar solo el csproj primero (mejor cache)
COPY HelloRealWorldAPI/HelloRealWorldAPI.csproj HelloRealWorldAPI/
RUN dotnet restore "HelloRealWorldAPI/HelloRealWorldAPI.csproj"

# Copiar todo el código
COPY . .

# Compilar
WORKDIR /src/HelloRealWorldAPI
RUN dotnet build "HelloRealWorldAPI.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "HelloRealWorldAPI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENTRYPOINT ["dotnet", "HelloRealWorldAPI.dll"]