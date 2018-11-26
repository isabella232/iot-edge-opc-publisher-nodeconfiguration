ARG runtime_base_tag=2.1-runtime-nanoserver-1803
ARG build_base_tag=2.1-sdk-nanoserver-1803

FROM microsoft/dotnet:${build_base_tag} AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY publisherdiag/*.csproj ./publisherdiag/
WORKDIR /app/publisherdiag
RUN dotnet restore

# copy and publish app
WORKDIR /app
COPY publisherdiag/. ./publisherdiag/
WORKDIR /app/publisherdiag
RUN dotnet publish -c Release -o out

# start it up
FROM microsoft/dotnet:${runtime_base_tag} AS runtime
WORKDIR /app
COPY --from=build /app/publisherdiag/out ./
WORKDIR /appdata
ENTRYPOINT ["dotnet", "/app/publisherdiag.dll"]