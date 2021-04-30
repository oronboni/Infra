FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app

# TODO : Copy csproj and restore as distinct layers
# Hits : use COPY of csproj and the RUN dotnet restore

# TODO Copy everything else and build
# Hint: use COPY and than use Run to do 'dotnet publish'

###########Runtime image #######################
# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
ENV TITLE="ILDC Docker Workshop"

# TODO : change the port 

# TODO : Expose the port


WORKDIR /app
COPY --from=build-env /app/out .
CMD ["dotnet", "core-webapp.dll"]