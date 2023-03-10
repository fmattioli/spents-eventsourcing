#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Spents.EventSourcing.API/Spents.EventSourcing.API.csproj", "Spents.EventSourcing.API/"]
COPY ["Spents.EventSourcing.CrossCuting/Spents.EventSourcing.CrossCuting.csproj", "Spents.EventSourcing.CrossCuting/"]
COPY ["Spents.EventSourcing.Infra.Data/Spents.EventSourcing.Infra.Data.csproj", "Spents.EventSourcing.Infra.Data/"]
COPY ["Spents.EventSourcing.Domain/Spents.EventSourcing.Domain.csproj", "Spents.EventSourcing.Domain/"]
COPY ["Spents.EventSourcing.Kafka.Core/Spents.EventSourcing.Kafka.Core.csproj", "Spents.EventSourcing.Kafka.Core/"]
RUN dotnet restore "Spents.EventSourcing.API/Spents.EventSourcing.API.csproj"
COPY . .
WORKDIR "/src/Spents.EventSourcing.API"
RUN dotnet build "Spents.EventSourcing.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Spents.EventSourcing.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Spents.EventSourcing.API.dll"]