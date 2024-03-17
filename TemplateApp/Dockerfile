FROM mcr.microsoft.com/dotnet/aspnet:8.0.3-bullseye-slim-arm64v8 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0.203-bullseye-slim-amd64 AS build
WORKDIR /src
COPY ["TemplateApp/TemplateApp.csproj", "TemplateApp/"]
RUN dotnet restore "TemplateApp/TemplateApp.csproj"
COPY . .
WORKDIR "/src/TemplateApp"
RUN dotnet build "TemplateApp.csproj" -r linux-arm64 -c Release -o /app/build --self-contained

FROM build AS publish
RUN dotnet publish "TemplateApp.csproj" -r linux-arm64 -c Release -o /app/publish --self-contained

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TemplateApp.dll"]
