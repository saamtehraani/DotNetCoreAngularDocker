#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

# BEGIN MODIFICATION - Node is needed for development (but not production)
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install --assume-yes nodejs
# END MODIFICATION


FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["DotNetCoreAngularDocker/DotNetCoreAngularDocker.csproj", "DotNetCoreAngularDocker/"]
RUN dotnet restore "DotNetCoreAngularDocker/DotNetCoreAngularDocker.csproj"
COPY . .
WORKDIR "/src/DotNetCoreAngularDocker"
RUN dotnet build "DotNetCoreAngularDocker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DotNetCoreAngularDocker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
COPY --from=client /app/dist /app/dist
ENTRYPOINT ["dotnet", "DotNetCoreAngularDocker.dll"]