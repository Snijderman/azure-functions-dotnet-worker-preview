#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:5.0 AS base

#RUN apt -y update
#RUN apt -y upgrade
#RUN apt install -y curl
#RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
#RUN apt-get install -y nodejs
#RUN npm i -g azure-functions-core-tools@3 --unsafe-perm true 

RUN apt -y update && \
    apt -y upgrade && \
    apt install -y curl && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    npm i -g azure-functions-core-tools@3 --unsafe-perm true 


WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["FunctionApp.csproj", "FunctionApp/"]
RUN dotnet restore "FunctionApp/FunctionApp.csproj"
COPY [ "." , "FunctionApp/"]
WORKDIR "/src/FunctionApp"
RUN dotnet build "FunctionApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "FunctionApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV AzureFunctionsJobHost__Logging__Console__IsEnabled=true
#ENTRYPOINT ["func", "start", "-p 80"]
CMD func start -p 80 --verbose
EXPOSE 80 # niet echt nodig natuurlijk, maar voor extra duidelijkheid