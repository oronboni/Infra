FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app

# TODO : Copy csproj and restore as distinct layers
COPY *.csproj  ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

###########Runtime image #######################
# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
ENV TITLE="ILDC Docker Workshop"
# TODO : change the port 
ENV PORT=5000
# TODO : Expose the port
EXPOSE $PORT

WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "core-webapp-final.dll"]