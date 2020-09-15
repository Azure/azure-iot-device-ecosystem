FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.csproj .
RUN dotnet restore

# copy and publish app and libraries
WORKDIR /app/
COPY . .
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/runtime:2.2-stretch-slim-arm32v7 AS runtime
ENV IOTHUB_DEVICE_CONN_STRING <your_device_connection_string>
WORKDIR /app
COPY --from=build /app/out ./
ENTRYPOINT ["dotnet", "MessageSample.dll"]
