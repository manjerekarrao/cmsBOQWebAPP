FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
# Setup NodeJs
RUN apt-get update && \
    apt-get install -y wget && \
    apt-get install -y gnupg2 && \
    wget -qO- https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get install -y build-essential nodejs
# End setup
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["CMSApp/CMSApp.csproj", "CMSApp/"]
RUN dotnet restore "CMSApp/CMSApp.csproj"
COPY . .
WORKDIR "/src/CMSApp"
RUN dotnet build "CMSApp.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "CMSApp.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "CMSApp.dll"]