FROM microsoft/dotnet:2.1-sdk-alpine AS build-env
WORKDIR /app

# copy and build everything else
COPY . ./
WORKDIR Thrust.SCooB.Agents.Integration

RUN dotnet publish -c Release -o out Thrust.SCooB.Agents.Integration.csproj


# Build da imagem
FROM microsoft/dotnet:2.1-sdk-alpine
WORKDIR /app

COPY --from=build-env /app/Thrust.SCooB.Agents.Integration/out .
ENTRYPOINT ["dotnet", "Thrust.SCooB.Agents.Integration.dll"]