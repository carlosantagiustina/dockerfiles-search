FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY skrrt/*.csproj ./skrrt/
RUN dotnet restore

# copy everything else and build app
COPY skrrt/. ./skrrt/
WORKDIR /app/skrrt
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS runtime
WORKDIR /app
COPY --from=build /app/skrrt/out ./
ENTRYPOINT ["dotnet", "skrrt.dll"]