# each as represents sage name, we can use the stage name to refer to the stage in later stages.
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
# / means apsolute path
WORKDIR /app

 # just for documentation if you need to force docker use a specific port you can set the environment variable ASPNETCORE_URLS to http://+:8080
EXPOSE 8080
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY ["Shopping.API/Shopping.API.csproj", "Shopping.API/"]

# runs from inside /src
RUN dotnet restore "Shopping.API/Shopping.API.csproj"
# runs from inside /src
COPY . .
# change teh folder to build the project
WORKDIR "/src/Shopping.API"

RUN dotnet build "Shopping.API.csproj" -c Release -o /app/build

FROM build AS publish

RUN dotnet publish "Shopping.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app

COPY --from=publish /app/publish .

ENTRYPOINT ["dotnet", "Shopping.API.dll"]

# in this file we have defined a multi-stage (base - build - publish - final).
#  (base) uses the ASP.NET runtime image to run the applcation, 
# (build) uses the SDK image to restore dependencies, build the application( compile the code),
#  and publish to collect the necessary files for deployment.(restored dependencies, compiled code, published files) 
# Finally, we copy the published output to the final stage and set the entry point to run the application.

#each stage can be considered as a separate environment with seperact working directory and files.