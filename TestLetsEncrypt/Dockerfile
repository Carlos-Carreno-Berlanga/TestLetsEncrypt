FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["TestLetsEncrypt/TestLetsEncrypt.csproj", "TestLetsEncrypt/"]
COPY ["LetsEncrypt/McMaster.AspNetCore.LetsEncrypt.csproj", "LetsEncrypt/"]
RUN dotnet restore "TestLetsEncrypt/TestLetsEncrypt.csproj"
COPY . .
WORKDIR "/src/TestLetsEncrypt"
RUN dotnet build "TestLetsEncrypt.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TestLetsEncrypt.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TestLetsEncrypt.dll"]