FROM httpd AS base
WORKDIR /usr/local/apache2/htdocs
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["MagicNumber.csproj","."]
RUN dotnet restore "MagicNumber.csproj"
COPY . .

RUN dotnet build "MagicNumber.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MagicNumber.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /usr/local/apache2/htdocs
COPY --from=publish /app/publish/wwwroot .