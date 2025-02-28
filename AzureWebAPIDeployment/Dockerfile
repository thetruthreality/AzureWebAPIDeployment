FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 5097

ENV ASPNETCORE_URLS=http://+:5097

FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
ARG configuration=Release
WORKDIR /src
COPY ["AzureWebAPIDeployment/AzureWebAPIDeployment.csproj", "AzureWebAPIDeployment/"]
RUN dotnet restore "AzureWebAPIDeployment\AzureWebAPIDeployment.csproj"
COPY . .
WORKDIR "/src/AzureWebAPIDeployment"
RUN dotnet build "AzureWebAPIDeployment.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "AzureWebAPIDeployment.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AzureWebAPIDeployment.dll"]
