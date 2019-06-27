FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
 
FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
RUN apt-get update -yq && apt-get upgrade -yq && apt-get install -yq curl git nano
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && apt-get install -yq nodejs build-essential
RUN npm install -g npm
RUN npm install -g @angular/cli
COPY ["dotnet-angular-sample.csproj", ""]
RUN dotnet restore "dotnet-angular-sample.csproj"
COPY . .
WORKDIR "/src/"
RUN dotnet build "dotnet-angular-sample.csproj" -c Release -o /app
 
FROM build AS publish
RUN dotnet publish "dotnet-angular-sample.csproj" -c Release -o /app
 
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "dotnet-angular-sample.dll"]
