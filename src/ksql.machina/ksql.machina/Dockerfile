#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-alpine AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine AS build
WORKDIR /src
COPY ["ksql.machina/ksql.machina.csproj", "ksql.machina/"]
RUN dotnet restore "ksql.machina/ksql.machina.csproj"
COPY . .
WORKDIR "/src/ksql.machina"
RUN dotnet build "ksql.machina.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ksql.machina.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ksql.machina.dll"]